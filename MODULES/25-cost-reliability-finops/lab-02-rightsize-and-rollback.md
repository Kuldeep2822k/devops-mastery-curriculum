---
title: 'Rightsize and Rollback'
tags:
  - lab
  - finops
  - reliability
module: "25"
---

# Lab 02 — Rightsize and Rollback (Cost vs Reliability Drill)

## Goal

Practice a realistic tradeoff loop:

- reduce resource limits to cut cost
- detect reliability impact using explicit signals
- roll back to a safer configuration
- quantify the cost delta (from Lab 01 style thinking)

## Prereqs

- Docker installed and working: `docker version`
- `curl`
- `python3`

## Setup

```bash
mkdir -p ~/work/devops-labs/mod25-rightsize
cd ~/work/devops-labs/mod25-rightsize
```

Create a small HTTP service that allocates memory at startup:

```bash
cat > app.py <<'EOF'
import os
import time
from http.server import BaseHTTPRequestHandler, HTTPServer

ALLOC_MB = int(os.getenv("ALLOC_MB", "0"))
PORT = int(os.getenv("PORT", "18060"))

buf = []
if ALLOC_MB > 0:
    try:
        for _ in range(ALLOC_MB):
            buf.append("x" * 1024 * 1024)
    except Exception:
        pass

class H(BaseHTTPRequestHandler):
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
    s = HTTPServer(("0.0.0.0", PORT), H)
    s.serve_forever()

if __name__ == "__main__":
    main()
EOF
```

Dockerfile:

```bash
cat > Dockerfile <<'EOF'
FROM python:3.12-slim
WORKDIR /app
COPY app.py /app/app.py
ENV PORT=18060
CMD ["python3","/app/app.py"]
EOF
```

Build:

```bash
docker build -t mod25-rightsize:dev .
```

## Steps

### 1) Baseline “Safe” Run

Run with a reasonable memory limit:

```bash
docker run -d --name mod25-ok -p 18060:18060 --memory 256m -e ALLOC_MB=64 mod25-rightsize:dev
sleep 1
curl -fsS http://127.0.0.1:18060/healthz
```

Expected:

- health returns `ok`

### 2) Rightsize Too Aggressively (Introduce Failure)

```bash
docker rm -f mod25-ok
docker run -d --name mod25-bad -p 18060:18060 --memory 64m -e ALLOC_MB=200 mod25-rightsize:dev || true
sleep 2
docker ps -a --filter name=mod25-bad
docker logs --tail 50 mod25-bad || true
curl -i http://127.0.0.1:18060/healthz || true
```

Expected signals:

- container exits or becomes unhealthy
- health check fails

### 3) Roll Back (Right-Size Safely)

```bash
docker rm -f mod25-bad 2>/dev/null || true
docker run -d --name mod25-ok -p 18060:18060 --memory 256m -e ALLOC_MB=64 mod25-rightsize:dev
sleep 1
curl -fsS http://127.0.0.1:18060/healthz
```

Expected:

- health returns `ok`

### 4) Quantify the Cost Tradeoff (Toy Model)

```bash
python3 - <<'PY'
baseline_mb = 256
bad_mb = 64
price_per_gb_month = 5.0
baseline_cost = (baseline_mb/1024.0) * price_per_gb_month
bad_cost = (bad_mb/1024.0) * price_per_gb_month
print("baseline_cost_month", round(baseline_cost, 2))
print("bad_cost_month", round(bad_cost, 2))
print("delta_month", round(baseline_cost - bad_cost, 2))
PY
```

Expected:

- delta is positive (cost saving), but reliability impact required rollback

## Verify

```bash
curl -fsS http://127.0.0.1:18060/healthz
docker inspect mod25-ok --format '{{.HostConfig.Memory}}'
```

Expected signals:

- health returns `ok`
- memory limit is set (non-zero bytes)

## Cleanup

```bash
docker rm -f mod25-ok mod25-bad 2>/dev/null || true
docker image rm -f mod25-rightsize:dev 2>/dev/null || true
cd ~
rm -rf ~/work/devops-labs/mod25-rightsize
```

## Troubleshooting

### Symptom: container does not fail under low memory

Fix:

- increase `ALLOC_MB` further and decrease `--memory`
- check container exit codes and logs:

```bash
docker inspect mod25-bad --format '{{.State.ExitCode}}' || true
docker logs --tail 80 mod25-bad || true
```
