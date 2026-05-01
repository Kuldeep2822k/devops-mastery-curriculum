---
title: 'Retries and Dead-Letter Handling'
tags:
  - lab
  - serverless
  - retries
module: "16"
---

# Lab 02 — Retries and Dead-Letter Handling (Retry Queue + DLQ)

## Goal

Extend an event worker to handle real-world behavior:

- retries with a max attempt policy
- dead-letter queue (DLQ) for poison events
- idempotency keys to prevent duplicate side effects

## Prereqs

- `python3`
- Lab 01 directory structure familiarity (this lab is self-contained but uses the same patterns)

## Setup

```bash
mkdir -p ~/work/devops-labs/mod16-retries
cd ~/work/devops-labs/mod16-retries
mkdir -p queue processed dlq
```

Create an enqueuer (supports explicit event ids so we can simulate duplicates):

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
    ap.add_argument("--poison", action="store_true")
    args = ap.parse_args()

    event_id = args.id or str(uuid.uuid4())
    payload = {
        "id": event_id,
        "type": args.type,
        "ts": int(time.time()),
        "poison": bool(args.poison),
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

Create a retrying worker with DLQ and idempotency:

```bash
cat > worker_retry.py <<'EOF'
import argparse
import json
import os
import shutil
import time

def list_events(queue_dir):
    items = [p for p in os.listdir(queue_dir) if p.endswith(".json") and not p.startswith(".")]
    items.sort()
    return [os.path.join(queue_dir, p) for p in items]

def read_attempts(attempts_path):
    if not os.path.exists(attempts_path):
        return {}
    with open(attempts_path, "r", encoding="utf-8") as f:
        return json.load(f)

def write_attempts(attempts_path, attempts):
    tmp = attempts_path + ".tmp"
    with open(tmp, "w", encoding="utf-8") as f:
        json.dump(attempts, f, sort_keys=True)
        f.write("\n")
    os.replace(tmp, attempts_path)

def main():
    ap = argparse.ArgumentParser()
    ap.add_argument("--queue-dir", default="queue")
    ap.add_argument("--processed-dir", default="processed")
    ap.add_argument("--dlq-dir", default="dlq")
    ap.add_argument("--attempts-file", default="attempts.json")
    ap.add_argument("--processed-ids-file", default="processed_ids.txt")
    ap.add_argument("--max-attempts", type=int, default=3)
    ap.add_argument("--poll-ms", type=int, default=200)
    ap.add_argument("--max", type=int, default=50)
    args = ap.parse_args()

    os.makedirs(args.queue_dir, exist_ok=True)
    os.makedirs(args.processed_dir, exist_ok=True)
    os.makedirs(args.dlq_dir, exist_ok=True)

    attempts = read_attempts(args.attempts_file)

    processed_ids = set()
    if os.path.exists(args.processed_ids_file):
        with open(args.processed_ids_file, "r", encoding="utf-8") as f:
            processed_ids = set([line.strip() for line in f if line.strip()])

    handled = 0
    while handled < args.max:
        events = list_events(args.queue_dir)
        if not events:
            time.sleep(args.poll_ms / 1000.0)
            continue

        path = events[0]
        with open(path, "r", encoding="utf-8") as f:
            event = json.load(f)

        event_id = str(event.get("id"))
        if event_id in processed_ids:
            os.unlink(path)
            print(json.dumps({"event": "dedupe_skip", "id": event_id}, sort_keys=True), flush=True)
            handled += 1
            continue

        n = int(attempts.get(event_id, 0)) + 1
        attempts[event_id] = n
        write_attempts(args.attempts_file, attempts)

        if event.get("poison") is True:
            if n >= args.max_attempts:
                dst = os.path.join(args.dlq_dir, os.path.basename(path))
                shutil.move(path, dst)
                print(json.dumps({"event": "dead_letter", "id": event_id, "attempt": n}, sort_keys=True), flush=True)
                handled += 1
                continue
            print(json.dumps({"event": "retry", "id": event_id, "attempt": n}, sort_keys=True), flush=True)
            time.sleep(min(0.2 * (2 ** (n - 1)), 2.0))
            handled += 1
            continue

        dst = os.path.join(args.processed_dir, os.path.basename(path))
        shutil.move(path, dst)
        processed_ids.add(event_id)
        with open(args.processed_ids_file, "a", encoding="utf-8") as f:
            f.write(event_id + "\n")
        print(json.dumps({"event": "processed", "id": event_id, "attempt": n}, sort_keys=True), flush=True)
        handled += 1

if __name__ == "__main__":
    main()
EOF
```

## Steps

### 1) Poison Event → Retries → DLQ

Enqueue a poison event:

```bash
python3 enqueue.py --type poison --poison
```

Run the worker multiple times; it should retry and then dead-letter:

```bash
python3 worker_retry.py --max-attempts 3 --max 5 | tee worker.log
ls -la dlq | head
```

Expected signals:

- worker.log includes `event=retry` lines and then `event=dead_letter`
- a JSON file exists under `dlq/`
- queue eventually becomes empty

### 2) Duplicate Delivery → Idempotency

Enqueue the same id twice:

```bash
event_id="evt-dup-1"
python3 enqueue.py --id "$event_id" --type payment
python3 enqueue.py --id "$event_id" --type payment
python3 worker_retry.py --max 5 | tee -a worker.log
```

Expected signals:

- one `processed` for the id
- one `dedupe_skip` for the id

### 3) Normal Event Processing

```bash
python3 enqueue.py --type ok
python3 worker_retry.py --max 5 | tee -a worker.log
```

Expected:

- `processed` event appears in logs

## Verify

```bash
echo "queue_count=$(ls -1 queue/*.json 2>/dev/null | wc -l | tr -d ' ')"
echo "processed_count=$(ls -1 processed/*.json 2>/dev/null | wc -l | tr -d ' ')"
echo "dlq_count=$(ls -1 dlq/*.json 2>/dev/null | wc -l | tr -d ' ')"
grep -E '"event": "(retry|dead_letter|processed|dedupe_skip)"' worker.log | tail -n 20
```

Expected signals:

- dlq_count is at least 1 (the poison event)
- processed_count is at least 2 (duplicate + normal event)
- worker.log contains `retry`, `dead_letter`, `processed`, and `dedupe_skip`

## Cleanup

```bash
cd ~
rm -rf ~/work/devops-labs/mod16-retries
```

## Troubleshooting

### Symptom: retries never stop

Diagnosis:

- confirm `--max-attempts` is set and attempts file increments

```bash
cat attempts.json
```

Fix:

- ensure poison events are moved to DLQ when attempt count reaches threshold

### Symptom: duplicate still processes twice

Fix:

- ensure the idempotency store is written before the side effect in real systems
- for this lab, confirm `processed_ids.txt` is being appended
