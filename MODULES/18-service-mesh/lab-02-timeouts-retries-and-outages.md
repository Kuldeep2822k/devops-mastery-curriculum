---
title: 'Timeouts, Retries, and Outages'
tags:
  - lab
  - service-mesh
  - resilience
module: "18"
---

# Lab 02 — Timeouts, Retries, and Outages (Retry Storm Drill)

## Goal

Practice safe client-side resilience patterns and failure amplification:

- timeouts that bound latency
- retries with backoff and a retry budget
- how naive retries amplify outages
- how to recover safely without “turn retries to 100”

## Prereqs

- `python3`
- `curl`

## Setup

```bash
mkdir -p ~/work/devops-labs/mod18-retries
cd ~/work/devops-labs/mod18-retries
```

Create an upstream service with slow and failing paths:

```bash
cat > upstream.py <<'EOF'
import time
from http.server import BaseHTTPRequestHandler, HTTPServer

class H(BaseHTTPRequestHandler):
    def do_GET(self):
        if self.path == "/ok":
            self.send_response(200)
            self.end_headers()
            self.wfile.write(b"ok")
            return
        if self.path == "/slow":
            time.sleep(1.0)
            self.send_response(200)
            self.end_headers()
            self.wfile.write(b"slow_ok")
            return
        if self.path == "/fail":
            self.send_response(503)
            self.end_headers()
            self.wfile.write(b"fail")
            return
        self.send_response(404)
        self.end_headers()
        self.wfile.write(b"not found")

    def log_message(self, format, *args):
        return

def main():
    HTTPServer(("127.0.0.1", 18500), H).serve_forever()

if __name__ == "__main__":
    main()
EOF
```

Create a retrying client:

```bash
cat > client.py <<'EOF'
import argparse
import time
import urllib.error
import urllib.request

def now_ms():
    return int(time.time() * 1000)

def main():
    ap = argparse.ArgumentParser()
    ap.add_argument("--url", required=True)
    ap.add_argument("--timeout-ms", type=int, default=300)
    ap.add_argument("--retries", type=int, default=0)
    ap.add_argument("--backoff-ms", type=int, default=100)
    ap.add_argument("--budget-ms", type=int, default=1000)
    args = ap.parse_args()

    start = now_ms()
    attempt = 0
    while True:
        attempt += 1
        try:
            req = urllib.request.Request(args.url, method="GET")
            with urllib.request.urlopen(req, timeout=args.timeout_ms / 1000.0) as r:
                body = r.read().decode("utf-8")
                dur = now_ms() - start
                print(f"ok attempt={attempt} dur_ms={dur} status={r.status} body={body}")
                return 0
        except Exception as e:
            dur = now_ms() - start
            print(f"err attempt={attempt} dur_ms={dur} err={type(e).__name__}")
            if dur >= args.budget_ms:
                print("budget_exhausted")
                return 2
            if attempt > (args.retries + 1):
                return 1
            sleep_ms = min(args.backoff_ms * (2 ** (attempt - 1)), 1000)
            time.sleep(sleep_ms / 1000.0)

if __name__ == "__main__":
    raise SystemExit(main())
EOF
```

## Steps

### 1) Start Upstream

```bash
python3 upstream.py
```

### 2) Baseline Success (No Retries Needed)

In another terminal:

```bash
python3 client.py --url http://127.0.0.1:18500/ok --timeout-ms 300 --retries 0
```

Expected:

- one attempt, fast response

### 3) Timeout Behavior (Bounded Latency)

Call the slow endpoint with a low timeout:

```bash
python3 client.py --url http://127.0.0.1:18500/slow --timeout-ms 200 --retries 0 --budget-ms 800 || true
```

Expected:

- timeouts occur
- budget eventually exhausted or non-zero exit

### 4) Naive Retries Amplify Failure

Call failing endpoint with aggressive retries:

```bash
python3 client.py --url http://127.0.0.1:18500/fail --timeout-ms 300 --retries 8 --backoff-ms 0 --budget-ms 800 || true
```

Expected:

- many attempts in a short time window
- this is the “retry storm” pattern

### 5) Safer Retries With Backoff + Budget

```bash
python3 client.py --url http://127.0.0.1:18500/fail --timeout-ms 300 --retries 3 --backoff-ms 100 --budget-ms 800 || true
```

Expected:

- fewer attempts
- bounded time spent

### 6) Outage Drill (Dependency Down)

Stop upstream (Ctrl+C) and then:

```bash
python3 client.py --url http://127.0.0.1:18500/ok --timeout-ms 200 --retries 3 --backoff-ms 100 --budget-ms 800 || true
```

Expected:

- fast failures (connection refused) and bounded budget

Recovery:

- restart upstream and re-run step 2 to confirm success

## Verify

```bash
python3 client.py --url http://127.0.0.1:18500/ok --timeout-ms 300 --retries 0
python3 client.py --url http://127.0.0.1:18500/fail --timeout-ms 300 --retries 3 --backoff-ms 100 --budget-ms 800 || true
```

Expected signals:

- success works when upstream is healthy
- failures are bounded by budget and limited retries

## Cleanup

Stop upstream (Ctrl+C), then:

```bash
cd ~
rm -rf ~/work/devops-labs/mod18-retries
```

## Troubleshooting

### Symptom: timeout test doesn’t time out

Fix:

- reduce `--timeout-ms` (e.g., 100–200ms) and ensure you call `/slow`

### Symptom: too many retries still happen

Fix:

- use a retry budget (`--budget-ms`) and backoff
- cap retries to a small number (3–5) and rely on containment/rollback instead
