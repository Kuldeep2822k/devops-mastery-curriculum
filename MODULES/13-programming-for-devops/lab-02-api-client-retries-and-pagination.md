---
title: "Lab 02: API Client (Timeouts, Retries, Pagination)"
tags:
  - lab
  - programming
  - apis
module: "13"
---

# Lab 02 — API Client (Timeouts, Retries, Pagination)

## Goal

Build a local API server that simulates:

- paginated responses
- transient 500 errors
- rate limiting (429)

Then build a client that:

- uses timeouts
- retries only on transient errors
- respects Retry-After for 429
- processes pages with a max-items bound

## Prereqs

- `python3`

## Setup

```bash
mkdir -p ~/work/devops-labs/mod13-api
cd ~/work/devops-labs/mod13-api
```

Create fake server `server.py`:

```bash
cat > server.py <<'EOF'
import json
import time
from http.server import BaseHTTPRequestHandler, HTTPServer
from urllib.parse import parse_qs, urlparse

DATA = [{"id": i} for i in range(1, 101)]

class Handler(BaseHTTPRequestHandler):
    def do_GET(self):
        q = parse_qs(urlparse(self.path).query)
        page = int(q.get("page", ["1"])[0])
        per = int(q.get("per", ["10"])[0])
        mode = q.get("mode", ["ok"])[0]

        if mode == "rate":
            self.send_response(429)
            self.send_header("Retry-After", "1")
            self.end_headers()
            self.wfile.write(b"rate limited")
            return
        if mode == "flaky" and page % 3 == 0:
            self.send_response(500)
            self.end_headers()
            self.wfile.write(b"transient error")
            return

        start = (page - 1) * per
        end = start + per
        items = DATA[start:end]
        out = {
            "page": page,
            "per": per,
            "items": items,
            "next_page": page + 1 if end < len(DATA) else None,
        }
        body = json.dumps(out).encode("utf-8")
        self.send_response(200)
        self.send_header("Content-Type", "application/json")
        self.end_headers()
        self.wfile.write(body)

    def log_message(self, format, *args):
        return

def main():
    HTTPServer(("127.0.0.1", 18010), Handler).serve_forever()

if __name__ == "__main__":
    main()
EOF
```

Run server:

```bash
python3 server.py
```

Create client `client.py` (in another terminal):

```bash
cat > client.py <<'EOF'
import json
import time
import urllib.error
import urllib.request

BASE = "http://127.0.0.1:18010/items"

def fetch(url, timeout_s=2):
    req = urllib.request.Request(url)
    with urllib.request.urlopen(req, timeout=timeout_s) as r:
        return r.getcode(), dict(r.headers), r.read()

def get_json(url):
    code, headers, body = fetch(url)
    if code == 200:
        return "ok", headers, json.loads(body.decode("utf-8"))
    return "bad", headers, {"code": code, "body": body.decode("utf-8", "replace")}

def main(mode="ok", max_items=25):
    page = 1
    per = 10
    out = []
    retries = 0
    while True:
        url = f"{BASE}?page={page}&per={per}&mode={mode}"
        try:
            status, headers, payload = get_json(url)
        except urllib.error.HTTPError as e:
            code = e.code
            if code == 429:
                ra = int(e.headers.get("Retry-After", "1"))
                time.sleep(ra)
                continue
            if code >= 500 and retries < 5:
                retries += 1
                time.sleep(min(2 ** retries, 10))
                continue
            raise

        if status != "ok":
            raise RuntimeError(payload)

        for item in payload["items"]:
            out.append(item)
            if len(out) >= max_items:
                print(json.dumps({"count": len(out), "items": out[:3], "truncated": True}))
                return

        if payload["next_page"] is None:
            break
        page = payload["next_page"]

    print(json.dumps({"count": len(out), "items": out[:3], "truncated": False}))

if __name__ == "__main__":
    main()
EOF
```

## Steps

### 1) Happy Path

```bash
python3 client.py
```

Expected:

- prints JSON summary with count <= max_items

### 2) Flaky Mode (Retries)

```bash
python3 client.py flaky
```

Expected:

- completes despite transient 500s due to retries

### 3) Rate Limit Mode (Retry-After)

```bash
python3 client.py rate
```

Expected:

- client waits and continues (or you stop after observing behavior)

## Verify

- timeouts used
- retries only for transient errors
- max_items bounds total work

## Cleanup

- stop server (Ctrl+C)
- remove lab dir:

```bash
cd ~
rm -rf ~/work/devops-labs/mod13-api
```

## Troubleshooting

### Symptom: client never finishes in rate mode

Fix:

- add a retry budget (max retries or max wall clock) before running against real APIs
- log the number of retries and stop when budget is exhausted

### Symptom: flaky mode fails repeatedly

Fix:

- confirm you only retry on transient errors (5xx) and that you back off
- confirm your client has a timeout and bounded `max_items`

## Why This Matters in Production

- Robust API clients prevent self-inflicted incidents (retry storms, runaway scripts).

## Definition of Done

- your client behaves safely under transient errors and rate limits.
