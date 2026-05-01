---
title: "Lab 01: Instrument a Service (Local Logs + Metrics Endpoint)"
tags:
  - lab
  - observability
  - logs
  - metrics
module: "11"
---

# Lab 01 — Instrument a Service (Local Logs + Metrics Endpoint)

## Goal

Build a tiny local HTTP service that exposes:

- structured logs (request events)
- a metrics endpoint (`/metrics`) with counters and latency buckets

Then use those signals to debug a controlled failure.

## Prereqs

- `python3`
- `curl`

## Setup

```bash
mkdir -p ~/work/devops-labs/mod11-observability
cd ~/work/devops-labs/mod11-observability
```

Create `app.py`:

```bash
cat > app.py <<'EOF'
import json
import time
from http.server import BaseHTTPRequestHandler, HTTPServer

metrics = {
    "requests_total": 0,
    "errors_total": 0,
    "latency_ms_le_50": 0,
    "latency_ms_le_200": 0,
    "latency_ms_le_1000": 0,
    "latency_ms_gt_1000": 0,
}

def log(event):
    print(json.dumps(event, sort_keys=True), flush=True)

class Handler(BaseHTTPRequestHandler):
    def do_GET(self):
        start = time.time()
        status = 200
        body = b"ok"

        if self.path == "/metrics":
            self.send_response(200)
            self.send_header("Content-Type", "text/plain; version=0.0.4")
            self.end_headers()
            lines = []
            for k, v in metrics.items():
                lines.append(f\"{k} {v}\")
            self.wfile.write((\"\\n\".join(lines) + \"\\n\").encode(\"utf-8\"))
            return

        if self.path == "/slow":
            time.sleep(1.2)
        if self.path == "/fail":
            status = 500
            body = b"fail"

        self.send_response(status)
        self.end_headers()
        self.wfile.write(body)

        dur_ms = int((time.time() - start) * 1000)
        metrics["requests_total"] += 1
        if status >= 500:
            metrics["errors_total"] += 1
        if dur_ms <= 50:
            metrics["latency_ms_le_50"] += 1
        elif dur_ms <= 200:
            metrics["latency_ms_le_200"] += 1
        elif dur_ms <= 1000:
            metrics["latency_ms_le_1000"] += 1
        else:
            metrics["latency_ms_gt_1000"] += 1

        log({
            "ts": int(time.time()),
            "path": self.path,
            "status": status,
            "dur_ms": dur_ms,
        })

    def log_message(self, format, *args):
        return

def main():
    host = "127.0.0.1"
    port = 18000
    httpd = HTTPServer((host, port), Handler)
    log({"ts": int(time.time()), "event": "start", "addr": f"{host}:{port}"})
    httpd.serve_forever()

if __name__ == "__main__":
    main()
EOF
```

## Steps

### 1) Run the Service

```bash
python3 app.py
```

### 2) Generate Traffic (In Another Terminal)

```bash
curl -fsS http://127.0.0.1:18000/
curl -fsS http://127.0.0.1:18000/slow || true
curl -fsS http://127.0.0.1:18000/fail || true
```

### 3) Read Logs as Evidence

Expected signals:

- JSON log lines for each request
- `/fail` shows status 500
- `/slow` shows dur_ms > 1000

### 4) Query Metrics (Simple)

```bash
curl -fsS http://127.0.0.1:18000/metrics | head
curl -fsS http://127.0.0.1:18000/metrics | grep -E '^requests_total\\s+' || true
curl -fsS http://127.0.0.1:18000/metrics | grep -E '^errors_total\\s+' || true
```

## Verify

- You can identify:
  - error rate spikes via status=500 logs
  - latency spikes via dur_ms
  - counters via /metrics

## Cleanup

- stop the process (Ctrl+C)
- remove the directory if desired:

```bash
cd ~
rm -rf ~/work/devops-labs/mod11-observability
```

## Troubleshooting

### Symptom: address already in use

Fix:

- change port in app.py and rerun

## Why This Matters in Production

- Structured logs provide fast root-cause hints.
- Metrics enable alerting and SLO math; traces connect multi-service latency.

## Definition of Done

- You can explain what each signal (logs/metrics/traces) would tell you for slow vs failing requests.
