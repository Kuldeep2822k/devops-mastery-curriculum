---
title: 'Local Event Processor'
tags:
  - lab
  - serverless
  - events
module: "16"
---

# Lab 01 — Local Event Processor (Queue + Worker)

## Goal

Build a small local-first event-driven system and practice controlled failure + recovery with evidence:

- a producer that enqueues events
- a worker loop that processes events
- observable signals: queue depth, processed log, error events
- a safe “pause/stop” and clean cleanup path

## Prereqs

- `python3`
- a writable workspace directory (example used below)

## Setup

```bash
mkdir -p ~/work/devops-labs/mod16-events
cd ~/work/devops-labs/mod16-events
mkdir -p queue processed
```

Create an event producer:

```bash
cat > enqueue.py <<'EOF'
import argparse
import json
import os
import time
import uuid

def main():
    ap = argparse.ArgumentParser()
    ap.add_argument("--queue-dir", default="queue")
    ap.add_argument("--id", default="")
    ap.add_argument("--type", default="demo")
    ap.add_argument("--fail", action="store_true")
    args = ap.parse_args()

    event_id = args.id or str(uuid.uuid4())
    payload = {
        "id": event_id,
        "type": args.type,
        "ts": int(time.time()),
        "fail": bool(args.fail),
    }

    os.makedirs(args.queue_dir, exist_ok=True)
    tmp_path = os.path.join(args.queue_dir, f".{event_id}.json")
    out_path = os.path.join(args.queue_dir, f"{event_id}.json")
    with open(tmp_path, "w", encoding="utf-8") as f:
        json.dump(payload, f, sort_keys=True)
        f.write("\n")
    os.replace(tmp_path, out_path)
    print(out_path)

if __name__ == "__main__":
    main()
EOF
```

Create a worker:

```bash
cat > worker.py <<'EOF'
import argparse
import json
import os
import shutil
import time

def list_events(queue_dir):
    items = [p for p in os.listdir(queue_dir) if p.endswith(".json") and not p.startswith(".")]
    items.sort()
    return [os.path.join(queue_dir, p) for p in items]

def main():
    ap = argparse.ArgumentParser()
    ap.add_argument("--queue-dir", default="queue")
    ap.add_argument("--processed-dir", default="processed")
    ap.add_argument("--max", type=int, default=20)
    ap.add_argument("--poll-ms", type=int, default=200)
    args = ap.parse_args()

    os.makedirs(args.queue_dir, exist_ok=True)
    os.makedirs(args.processed_dir, exist_ok=True)

    processed = 0
    while processed < args.max:
        events = list_events(args.queue_dir)
        if not events:
            time.sleep(args.poll_ms / 1000.0)
            continue

        path = events[0]
        with open(path, "r", encoding="utf-8") as f:
            event = json.load(f)

        if event.get("fail") is True:
            raise SystemExit(f"event_failed id={event.get('id')}")

        line = json.dumps({"event": "processed", "id": event.get("id"), "type": event.get("type")}, sort_keys=True)
        print(line, flush=True)

        dst = os.path.join(args.processed_dir, os.path.basename(path))
        shutil.move(path, dst)
        processed += 1

if __name__ == "__main__":
    main()
EOF
```

## Steps

### 1) Enqueue Events

```bash
python3 enqueue.py --type ok
python3 enqueue.py --type ok
ls -la queue | head
```

Expected signals:

- queue contains 2 JSON files

### 2) Process Events

```bash
python3 worker.py --max 2 | tee worker.log
ls -la processed | head
```

Expected signals:

- `worker.log` contains `event=processed` JSON lines
- queue is empty (or reduced)
- processed contains the event files

### 3) Inject a Failure (Poison Event)

Enqueue a failing event:

```bash
python3 enqueue.py --type poison --fail
ls -la queue | head
```

Run worker and observe failure:

```bash
python3 worker.py --max 1 || true
```

Expected signals:

- worker exits non-zero with message `event_failed ...`
- the event remains in `queue/` (not moved)

### 4) Recover Safely

In real systems, you would quarantine or dead-letter. For this lab, recover by rewriting the event to remove the failure flag:

```bash
event_file="$(ls queue/*.json | head -n 1)"
python3 - <<'PY'
import json, os
path = os.environ["event_file"]
with open(path, "r", encoding="utf-8") as f:
  e = json.load(f)
e["fail"] = False
tmp = path + ".tmp"
with open(tmp, "w", encoding="utf-8") as f:
  json.dump(e, f, sort_keys=True)
  f.write("\n")
os.replace(tmp, path)
print("updated", path)
PY
python3 worker.py --max 1 | tee -a worker.log
```

Expected signals:

- worker processes the event
- file moves to `processed/`

## Verify

```bash
echo "queue_count=$(ls -1 queue/*.json 2>/dev/null | wc -l | tr -d ' ')"
echo "processed_count=$(ls -1 processed/*.json 2>/dev/null | wc -l | tr -d ' ')"
grep -c '"event": "processed"' worker.log || true
```

Expected signals:

- queue_count is 0
- processed_count is 3
- processed log lines exist in worker.log

## Cleanup

```bash
cd ~
rm -rf ~/work/devops-labs/mod16-events
```

## Troubleshooting

### Symptom: queue never drains

Diagnosis:

```bash
ls -la queue | head
python3 worker.py --max 1 || true
```

Fix:

- inspect the event file; remove the failure flag or handle it via a dead-letter pattern (Lab 02)

### Symptom: JSON parsing fails

Fix:

- delete the malformed event and re-enqueue it
