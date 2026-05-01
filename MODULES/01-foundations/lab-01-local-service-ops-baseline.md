---
title: "Lab 01: Local Service Ops Baseline"
tags:
  - lab
  - foundations
module: "01"
---

# Lab 01 — Local Service Ops Baseline

## Goal

Create a minimal service locally with:

- a health endpoint
- structured logs (at least consistent key/value)
- a simple “deploy/run” script
- verification signals
- an initial runbook draft entry

## Prereqs

- Python 3 installed (from setup)
- `curl` installed
- A workspace directory: `~/work/devops-labs`

## Setup

Create a project directory:

```bash
mkdir -p ~/work/devops-labs/mod01-service
cd ~/work/devops-labs/mod01-service
```

Create the service file:

```bash
cat > app.py <<'EOF'
import json
import os
import time
from http.server import BaseHTTPRequestHandler, HTTPServer

SERVICE_NAME = os.getenv("SERVICE_NAME", "mod01-service")
PORT = int(os.getenv("PORT", "8080"))
FAIL_HEALTH = os.getenv("FAIL_HEALTH", "0") == "1"
LATENCY_MS = int(os.getenv("LATENCY_MS", "0"))

def log(event, **fields):
    payload = {"ts": int(time.time()), "service": SERVICE_NAME, "event": event}
    payload.update(fields)
    print(json.dumps(payload), flush=True)

class Handler(BaseHTTPRequestHandler):
    def do_GET(self):
        if LATENCY_MS > 0:
            time.sleep(LATENCY_MS / 1000.0)
        if self.path == "/healthz":
            if FAIL_HEALTH:
                self.send_response(500)
                self.end_headers()
                log("health", status="fail")
                self.wfile.write(b"fail")
                return
            self.send_response(200)
            self.end_headers()
            log("health", status="ok")
            self.wfile.write(b"ok")
            return
        self.send_response(404)
        self.end_headers()
        log("request", path=self.path, status=404)
        self.wfile.write(b"not found")

    def log_message(self, format, *args):
        return

def main():
    server = HTTPServer(("0.0.0.0", PORT), Handler)
    log("start", port=PORT)
    server.serve_forever()

if __name__ == "__main__":
    main()
EOF
```

Create a runner script:

```bash
cat > run.sh <<'EOF'
set -eu
PORT="${PORT:-8080}"
SERVICE_NAME="${SERVICE_NAME:-mod01-service}"
export PORT SERVICE_NAME
python3 app.py
EOF
chmod +x run.sh
```

## Steps

### 1) Start the Service

In one terminal:

```bash
cd ~/work/devops-labs/mod01-service
./run.sh
```

### 2) Send Health Checks

In another terminal:

```bash
curl -fsS http://localhost:8080/healthz
```

## Verify

### Verify Endpoint Behavior

```bash
curl -i http://localhost:8080/healthz
```

Expected signals:

- HTTP 200
- body `ok`

### Verify Logs are High Signal

Observe service logs; expected signals:

- JSON lines containing `event` and `status`
- a `start` event with a port field

### Verify “Failure Toggle” Works (Controlled Fault)

Stop and restart with a fault:

```bash
export FAIL_HEALTH=1
./run.sh
```

Then:

```bash
curl -i http://localhost:8080/healthz || true
```

Expected signals:

- HTTP 500
- logs show `event=health status=fail`

## Cleanup

Stop the service process (Ctrl+C).

Verify cleanup:

```bash
ss -tulpn | grep -E ':(8080)\b' || true
```

Expected signals:

- no process listening on 8080

## Troubleshooting

### Symptom: “Address already in use”

Diagnosis:

```bash
ss -tulpn | grep -E ':(8080)\b' || true
```

Fix:

- stop the conflicting process
- or run with a different port:

```bash
PORT=18080 ./run.sh
curl -fsS http://localhost:18080/healthz
```

### Symptom: curl fails with connection refused

Diagnosis:

- confirm the server is running and listening
- check logs for startup failure

Fix:

- restart the service and re-run health checks

### Symptom: health always fails

Diagnosis:

```bash
env | grep -E '^FAIL_HEALTH=' || true
```

Fix:

- unset the flag:

```bash
unset FAIL_HEALTH
./run.sh
```

## Why This Matters in Production

- Health endpoints are a core operational interface for orchestrators and load balancers.
- Structured logs reduce time-to-diagnosis under pressure.
- A controlled failure toggle is a safe way to practice incident response without guessing.

## What to Write in a Runbook

- How to verify health (`curl` command and expected output)
- How to check if the port is bound (`ss` command)
- What log lines indicate success vs failure
- Common failure: port collision and how to fix

## Definition of Done

- Service starts and passes `/healthz` checks.
- Failure toggle produces a predictable failing signal and recoverable behavior.
- Cleanup leaves no listener on the service port.
