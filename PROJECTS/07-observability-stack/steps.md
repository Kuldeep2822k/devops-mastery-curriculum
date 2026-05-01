---
title: '07-observability-stack: Steps'
tags:
  - project
---

# 07-observability-stack — Steps

## Prereqs

- `python3`, `curl`

## Setup

```bash
mkdir -p ~/work/devops-labs/07-observability-stack/evidence
cd ~/work/devops-labs/07-observability-stack
```

Create a small service with slow/fail paths and structured logs:

```bash
cat > service.py <<'EOF'
import json
import time
from http.server import BaseHTTPRequestHandler, HTTPServer

class H(BaseHTTPRequestHandler):
    def do_GET(self):
        t0=time.time()
        code=200
        body=b"ok"
        if self.path=="/fail":
            code=500
            body=b"fail"
        if self.path=="/slow":
            time.sleep(0.5)
            body=b"slow_ok"
        self.send_response(code)
        self.end_headers()
        self.wfile.write(body)
        line=json.dumps({"path":self.path,"status":code,"dur_ms":int((time.time()-t0)*1000)})
        print(line, flush=True)
    def log_message(self, format, *args):
        return

HTTPServer(("127.0.0.1", 18090), H).serve_forever()
EOF
```

Start service:

```bash
python3 service.py | tee evidence/service.log
```

## Steps

### 1) Generate Traffic

In another terminal:

```bash
for i in $(seq 1 30); do curl -fsS http://127.0.0.1:18090/ >/dev/null; done
for i in $(seq 1 10); do curl -s http://127.0.0.1:18090/slow >/dev/null; done
for i in $(seq 1 5); do curl -s http://127.0.0.1:18090/fail >/dev/null || true; done
```

### 2) Derive Signals from Logs

```bash
grep '"status":500' evidence/service.log | wc -l | tr -d ' '
awk -F'dur_ms":' '{print $2}' evidence/service.log | tr -d '}' | sort -n | tail -n 5
```

### 3) Create Alert + Runbook Artifacts

```bash
cat > evidence/alerts.md <<'EOF'
# Alerts

## Availability

- SLI: 5xx rate
- Trigger: > 1% over 5m
- Runbook: runbook.md

## Latency

- SLI: p95 dur_ms
- Trigger: p95 > 500ms over 10m
- Runbook: runbook.md
EOF

cat > evidence/runbook.md <<'EOF'
# Runbook

## First 10 Minutes

- Check error count and slow requests

## Commands

- grep '"status":500' evidence/service.log | tail
- grep '"dur_ms"' evidence/service.log | tail

## Containment

- rollback recent deploy / disable slow path / shed load
EOF
```

## Verify

```bash
test -s evidence/service.log
test -s evidence/alerts.md
test -s evidence/runbook.md
```

## Cleanup

Stop the service (Ctrl+C), then:

```bash
cd ~
rm -rf ~/work/devops-labs/07-observability-stack
```

## Troubleshooting

- If logs are empty, confirm you are tee-ing stdout and generating traffic.
