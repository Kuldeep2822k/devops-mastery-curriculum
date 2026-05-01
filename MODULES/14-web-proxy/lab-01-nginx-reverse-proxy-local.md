---
title: "Lab 01: Nginx Reverse Proxy (Local)"
tags:
  - lab
  - web-proxy
  - nginx
module: "14"
---

# Lab 01 — Nginx Reverse Proxy (Local)

## Goal

Run:

- a local upstream HTTP service
- nginx as a reverse proxy in front of it

Then verify:

- proxy routing works
- headers and client IP forwarding basics

## Prereqs

- nginx installed (or use container if you prefer)
- `python3` and `curl`

## Setup

Create a lab directory:

```bash
mkdir -p ~/work/devops-labs/mod14-proxy
cd ~/work/devops-labs/mod14-proxy
```

## Steps

### 1) Create and Run the Upstream Server


```bash
cat > upstream.py <<'EOF'
import json

class H(BaseHTTPRequestHandler):
    def do_GET(self):
        body = json.dumps({
            "path": self.path,
            "host": self.headers.get("Host"),
            "xff": self.headers.get("X-Forwarded-For"),
            "xproto": self.headers.get("X-Forwarded-Proto"),
        }, sort_keys=True).encode("utf-8")
        self.send_response(200)
        self.send_header("Content-Type", "application/json")
        self.end_headers()
        self.wfile.write(body)
    def log_message(self, format, *args):
        return

HTTPServer(("127.0.0.1", 18020), H).serve_forever()
EOF
python3 upstream.py
```

### 2) Create Nginx Config

In another terminal:

```bash
cat > nginx.conf <<'EOF'
events {}
http {
  server {
    listen 18021;

    location / {
      proxy_pass http://127.0.0.1:18020;
      proxy_set_header Host $host;
      proxy_set_header X-Forwarded-For $remote_addr;
      proxy_set_header X-Forwarded-Proto $scheme;
    }
  }
}
EOF
```

### 3) Run Nginx (Reverse Proxy)

Command may vary by distro:

```bash
nginx -c "$PWD/nginx.conf" -p "$PWD" -g "daemon off;"
```

## Verify

From a third terminal:

```bash
curl -fsS http://127.0.0.1:18021/ | jq . || curl -fsS http://127.0.0.1:18021/
```

Expected:

- JSON response shows forwarded headers

## Cleanup

- stop nginx and upstream (Ctrl+C in each terminal)
- remove directory:

```bash
cd ~
rm -rf ~/work/devops-labs/mod14-proxy
```

## Troubleshooting

### Symptom: nginx won’t start

Diagnosis:

- port already in use

Fix:

- change listen port and retry

## Definition of Done

- proxy routes successfully and forwarded headers are visible at upstream.
