---
title: "Lab 02: Break and Fix Runtime Failures"
tags:
  - lab
  - containers
  - troubleshooting
  - incident
module: "05"
---

# Lab 02 — Break and Fix Runtime Failures

## Goal

Practice diagnosing common container failures by injecting faults and recovering:

- wrong command / missing file → container exits
- port mapping mismatch → service unreachable
- permissions mismatch on volume → startup failure
- memory limit too low → OOMKilled-like behavior

## Prereqs

- Docker installed
- `curl` installed

## Setup

Create a lab directory:

```bash
mkdir -p ~/work/devops-labs/mod05-breakfix
cd ~/work/devops-labs/mod05-breakfix
```

Create a small app that writes to a data directory:

```bash
cat > app.py <<'EOF'
import os
import time
from http.server import BaseHTTPRequestHandler, HTTPServer

PORT = int(os.getenv("PORT", "8080"))
DATA_DIR = os.getenv("DATA_DIR", "/data")

def init_data():
    os.makedirs(DATA_DIR, exist_ok=True)
    with open(os.path.join(DATA_DIR, "boot.txt"), "w") as f:
        f.write(str(int(time.time())) + "\n")

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
    init_data()
    s = HTTPServer(("0.0.0.0", PORT), Handler)
    print(f"start port={PORT} data_dir={DATA_DIR}", flush=True)
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
ENV PORT=8080
ENV DATA_DIR=/data
EXPOSE 8080
CMD ["python3", "/app/app.py"]
EOF
```

Build:

```bash
docker build -t mod05-breakfix:dev .
```

## Steps

### Fault 1 — Wrong Command / Missing File (Crash on Start)

Run with an invalid command:

```bash
docker run --rm --name mod05-badcmd mod05-breakfix:dev python3 /app/missing.py || true
```

Diagnosis:

- inspect error output
- understand “no such file” at entrypoint time

Fix:

```bash
docker run -d --name mod05-ok -p 18081:8080 mod05-breakfix:dev
curl -fsS http://localhost:18081/healthz
```

Expected signals:

- container runs
- health returns `ok`

### Fault 2 — Port Mapping Mismatch

Remove and rerun with wrong mapping:

```bash
docker rm -f mod05-ok
docker run -d --name mod05-ok -p 18081:9999 mod05-breakfix:dev
```

Diagnosis commands:

```bash
docker logs --tail 50 mod05-ok
curl -i http://localhost:18081/healthz || true
docker inspect mod05-ok | grep -n "HostPort" -n || true
```

Root cause:

- host maps to container port 9999 but app listens on 8080.

Fix:

```bash
docker rm -f mod05-ok
docker run -d --name mod05-ok -p 18081:8080 mod05-breakfix:dev
curl -fsS http://localhost:18081/healthz
```

### Fault 3 — Volume Permission Mismatch (Non-Root Pattern)

Simulate a common failure by mounting a read-only directory:

```bash
mkdir -p data
chmod 555 data
docker rm -f mod05-ok
docker run -d --name mod05-vol -p 18082:8080 -v "$PWD/data":/data mod05-breakfix:dev || true
sleep 1
docker ps -a --filter name=mod05-vol
docker logs --tail 50 mod05-vol || true
```

Expected signals:

- container exits
- logs show permission denied writing to /data

Fix (local-only):

```bash
chmod 755 data
docker rm -f mod05-vol || true
docker run -d --name mod05-vol -p 18082:8080 -v "$PWD/data":/data mod05-breakfix:dev
curl -fsS http://localhost:18082/healthz
ls -la data
```

### Fault 4 — Memory Limit Too Low

Run with a very low memory limit and observe behavior. This is a simulation; not every system will OOM this app.

```bash
docker rm -f mod05-vol
docker run -d --name mod05-mem -p 18083:8080 --memory 32m mod05-breakfix:dev
sleep 1
docker ps -a --filter name=mod05-mem
docker logs --tail 50 mod05-mem || true
docker inspect mod05-mem --format '{{.State.ExitCode}}' || true
```

Diagnosis:

- check if it exits unexpectedly
- check for exit code 137 patterns (if present)

Fix:

```bash
docker rm -f mod05-mem || true
docker run -d --name mod05-mem -p 18083:8080 --memory 256m mod05-breakfix:dev
curl -fsS http://localhost:18083/healthz
```

## Verify

For each fault:

- capture the symptom
- capture diagnosis commands and their outputs (redacted if needed)
- prove root cause
- apply fix
- verify health endpoint works

Verification commands:

```bash
curl -fsS http://localhost:18081/healthz
curl -fsS http://localhost:18082/healthz
curl -fsS http://localhost:18083/healthz
```

## Cleanup

```bash
docker rm -f mod05-ok mod05-vol mod05-mem 2>/dev/null || true
docker image rm mod05-breakfix:dev || true
chmod 755 data || true
rm -rf data
```

Verify cleanup:

```bash
docker ps -a | grep -E 'mod05-(ok|vol|mem|badcmd)' || true
```

## Troubleshooting

### Symptom: volume permission test doesn’t fail

Diagnosis:

- your host filesystem permissions might not map as expected

Fix:

- force read-only mount:

```bash
docker run -d --name mod05-vol -p 18082:8080 -v "$PWD/data":/data:ro mod05-breakfix:dev
```

### Symptom: memory test doesn’t OOM

Interpretation:

- the app may fit in the limit on your system

Fix:

- treat the exercise as learning how to check limits and exit codes
- use `docker stats` to observe memory usage

## Why This Matters in Production

- Most container incidents are basic runtime issues: wrong command, wrong ports, bad mounts, resource limits.
- Operators must move from symptom → evidence → root cause → fix quickly.

## What to Write in a Runbook

- common container exit patterns and exit codes
- port mapping diagnosis
- mount and permission checks
- resource limit checks and safe tuning steps

## Definition of Done

- You can reproduce at least 3 failure modes and recover with evidence-based debugging.
