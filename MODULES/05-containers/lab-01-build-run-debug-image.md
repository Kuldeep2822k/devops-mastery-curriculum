---
title: "Lab 01: Build, Run, and Debug an Image"
tags:
  - lab
  - containers
  - docker
module: "05"
---

# Lab 01 — Build, Run, and Debug an Image

## Goal

Build a container image for a tiny HTTP service and practice:

- building reproducibly
- running with port mapping
- inspecting logs, env, filesystem
- verifying health signals

## Prereqs

- Docker working: `docker run --rm hello-world`
- `curl` installed

## Setup

Create a lab directory:

```bash
mkdir -p ~/work/devops-labs/mod05-container-service
cd ~/work/devops-labs/mod05-container-service
```

Create an app:

```bash
cat > app.py <<'EOF'
import os
import time
from http.server import BaseHTTPRequestHandler, HTTPServer

PORT = int(os.getenv("PORT", "8080"))

class Handler(BaseHTTPRequestHandler):
    def do_GET(self):
        if self.path == "/healthz":
            self.send_response(200)
            self.end_headers()
            self.wfile.write(b"ok")
            return
        self.send_response(404)
        self.end_headers()
        self.wfile.write(b"not found")

    def log_message(self, format, *args):
        return

def main():
    s = HTTPServer(("0.0.0.0", PORT), Handler)
    print(f"start port={PORT} ts={int(time.time())}", flush=True)
    s.serve_forever()

if __name__ == "__main__":
    main()
EOF
```

Create Dockerfile:

```bash
cat > Dockerfile <<'EOF'
FROM python:3.12-slim
WORKDIR /app
COPY app.py /app/app.py
ENV PORT=8080
EXPOSE 8080
CMD ["python3", "/app/app.py"]
EOF
```

## Steps

### 1) Build Image

```bash
docker build -t mod05-service:dev .
```

### 2) Run Container

```bash
docker run -d --name mod05-service -p 18080:8080 mod05-service:dev
```

### 3) Verify Health

```bash
curl -fsS http://localhost:18080/healthz
```

Expected signal:

- output `ok`

### 4) Inspect Logs and Runtime State

```bash
docker logs --tail 50 mod05-service
docker inspect mod05-service | head -n 60
docker exec -it mod05-service sh -lc "ls -la /app && env | grep -E '^PORT=' || true"
```

Expected signals:

- logs include `start port=8080`
- `/app/app.py` exists inside container

### 5) Confirm Port Binding and Listener

Host:

```bash
ss -tulpn | grep -E ':(18080)\b' || true
```

Inside container:

```bash
docker exec -it mod05-service sh -lc "ss -tulpn || netstat -tulpn || true"
```

## Verify

```bash
docker ps --filter name=mod05-service
curl -i http://localhost:18080/healthz
```

Expected signals:

- container is running
- HTTP 200 on health

## Cleanup

```bash
docker rm -f mod05-service
docker image rm mod05-service:dev || true
docker ps -a --filter name=mod05-service
```

Expected signals:

- container removed
- no leftover container named mod05-service

## Troubleshooting

### Symptom: port already in use

Diagnosis:

```bash
ss -tulpn | grep -E ':(18080)\b' || true
```

Fix:

- choose another host port:

```bash
docker run -d --name mod05-service -p 28080:8080 mod05-service:dev
curl -fsS http://localhost:28080/healthz
```

### Symptom: health check fails but container running

Diagnosis:

- check logs: `docker logs mod05-service`
- exec and confirm app file exists

Fix:

- rebuild image, ensure Dockerfile copies correct file

## Why This Matters in Production

- Container debugging is a core on-call skill.
- Being able to prove “is it listening?” and “what version/config is running?” reduces MTTR.

## What to Write in a Runbook

- `docker ps`, `docker logs`, `docker inspect`, `docker exec` baseline
- health check commands and expected output
- port collision diagnosis and fix

## Definition of Done

- Image builds successfully.
- Container serves /healthz through host port mapping.
- You can capture logs and runtime info without guessing.
