---
title: '03-containerized-service: Steps'
tags:
  - project
---

# 03-containerized-service — Steps

## Prereqs

- Docker installed and working
- `curl`

## Setup

```bash
mkdir -p ~/work/devops-labs/03-containerized-service
cd ~/work/devops-labs/03-containerized-service
```

Create a small HTTP service:

```bash
cat > app.py <<'EOF'
import os
from http.server import BaseHTTPRequestHandler, HTTPServer

PORT = int(os.getenv("PORT", "18080"))
VERSION = os.getenv("VERSION", "dev")

class H(BaseHTTPRequestHandler):
    def do_GET(self):
        if self.path == "/healthz":
            self.send_response(200)
            self.end_headers()
            self.wfile.write(b"ok")
            return
        if self.path == "/version":
            self.send_response(200)
            self.end_headers()
            self.wfile.write(VERSION.encode("utf-8"))
            return
        self.send_response(404)
        self.end_headers()
        self.wfile.write(b"not found")
    def log_message(self, format, *args):
        return

HTTPServer(("0.0.0.0", PORT), H).serve_forever()
EOF
```

Dockerfile:

```bash
cat > Dockerfile <<'EOF'
FROM python:3.12-slim
WORKDIR /app
COPY app.py /app/app.py
ENV PORT=18080
ENV VERSION=dev
CMD ["python3","/app/app.py"]
EOF
```

## Steps

### 1) Build and Run

```bash
docker build -t proj03:dev .
docker run -d --name proj03 -p 18080:18080 proj03:dev
sleep 1
curl -fsS http://127.0.0.1:18080/healthz
curl -fsS http://127.0.0.1:18080/version
```

### 2) Break/Fix: Port Mismatch

Break:

```bash
docker rm -f proj03
# run container on wrong published port
docker run -d --name proj03 -p 18081:18080 proj03:dev
curl -fsS http://127.0.0.1:18080/healthz || true
```

Fix:

```bash
docker rm -f proj03
docker run -d --name proj03 -p 18080:18080 proj03:dev
curl -fsS http://127.0.0.1:18080/healthz
```

### 3) Break/Fix: Bad Env

```bash
docker rm -f proj03
docker run -d --name proj03 -p 18080:18080 -e PORT=99999 proj03:dev || true
sleep 1
docker ps -a --filter name=proj03
docker logs --tail 50 proj03 || true
```

Fix:

```bash
docker rm -f proj03
docker run -d --name proj03 -p 18080:18080 -e VERSION=1.0.0 proj03:dev
sleep 1
curl -fsS http://127.0.0.1:18080/version
```

## Verify

```bash
curl -fsS http://127.0.0.1:18080/healthz
curl -fsS http://127.0.0.1:18080/version | grep -q '1.0.0'
```

## Cleanup

```bash
docker rm -f proj03 2>/dev/null || true
docker image rm -f proj03:dev 2>/dev/null || true
cd ~
rm -rf ~/work/devops-labs/03-containerized-service
```

## Troubleshooting

- If build fails, confirm Docker daemon permissions and free disk.
