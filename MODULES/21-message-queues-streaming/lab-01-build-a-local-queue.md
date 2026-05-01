---
title: 'Build a Local Queue'
tags:
  - lab
  - messaging
module: "21"
---

# Lab 01 — Build a Local Queue (File Queue + Visibility Timeout)

## Goal

Build a minimal local-first queue and practice operational triage signals:

- enqueue events (producer)
- process events (worker)
- at-least-once semantics via a visibility timeout
- measure lag (oldest message age) and backlog (queue depth)

## Prereqs

- `python3`

## Setup

```bash
mkdir -p ~/work/devops-labs/mod21-queue
cd ~/work/devops-labs/mod21-queue
mkdir -p queue inflight done
```

Create a producer:

```bash
cat > enqueue.py <<'EOF'
import argparse
import json
import os
import time
import uuid

def main():
    ap = argparse.ArgumentParser()
    ap.add_argument("--queue", default="queue")
    ap.add_argument("--type", default="demo")
    ap.add_argument("--n", type=int, default=1)
    args = ap.parse_args()

    os.makedirs(args.queue, exist_ok=True)
    for _ in range(args.n):
        event_id = str(uuid.uuid4())
        msg = {"id": event_id, "type": args.type, "ts": int(time.time())}
        tmp = os.path.join(args.queue, f".{event_id}.json")
        out = os.path.join(args.queue, f"{int(time.time())}-{event_id}.json")
        with open(tmp, "w", encoding="utf-8") as f:
            json.dump(msg, f, sort_keys=True)
            f.write("\n")
        os.replace(tmp, out)
        print(out)

if __name__ == "__main__":
    main()
EOF
```

Create a worker with a visibility timeout:

```bash
cat > worker.py <<'EOF'
import argparse
import json
import os
import shutil
import time

def now():
    return int(time.time())

def list_msgs(path):
    items = [p for p in os.listdir(path) if p.endswith(".json") and not p.startswith(".")]
    items.sort()
    return [os.path.join(path, p) for p in items]

def claim(queue_dir, inflight_dir, visibility_s):
    for p in list_msgs(queue_dir):
        name = os.path.basename(p)
        claim_path = os.path.join(inflight_dir, name)
        try:
            os.replace(p, claim_path)
            deadline = now() + visibility_s
            return claim_path, deadline
        except FileNotFoundError:
            continue
    return None, None

def release_if_expired(inflight_dir, queue_dir):
    for p in list_msgs(inflight_dir):
        try:
            st = os.stat(p)
        except FileNotFoundError:
            continue
        age = now() - int(st.st_mtime)
        if age >= 2:
            dst = os.path.join(queue_dir, os.path.basename(p))
            try:
                os.replace(p, dst)
            except FileNotFoundError:
                continue

def main():
    ap = argparse.ArgumentParser()
    ap.add_argument("--queue", default="queue")
    ap.add_argument("--inflight", default="inflight")
    ap.add_argument("--done", default="done")
    ap.add_argument("--visibility-s", type=int, default=2)
    ap.add_argument("--max", type=int, default=10)
    ap.add_argument("--fail-on-type", default="")
    args = ap.parse_args()

    os.makedirs(args.queue, exist_ok=True)
    os.makedirs(args.inflight, exist_ok=True)
    os.makedirs(args.done, exist_ok=True)

    processed = 0
    while processed < args.max:
        release_if_expired(args.inflight, args.queue)
        msg_path, _ = claim(args.queue, args.inflight, args.visibility_s)
        if not msg_path:
            time.sleep(0.2)
            continue

        with open(msg_path, "r", encoding="utf-8") as f:
            msg = json.load(f)

        if args.fail_on_type and msg.get("type") == args.fail_on_type:
            raise SystemExit(f"processing_failed id={msg.get('id')}")

        print(json.dumps({"event": "processed", "id": msg.get("id"), "type": msg.get("type")}, sort_keys=True), flush=True)
        dst = os.path.join(args.done, os.path.basename(msg_path))
        shutil.move(msg_path, dst)
        processed += 1

if __name__ == "__main__":
    main()
EOF
```

Create a triage tool:

```bash
cat > triage.py <<'EOF'
import argparse
import os
import time

def list_msgs(path):
    if not os.path.isdir(path):
        return []
    items = [p for p in os.listdir(path) if p.endswith(".json") and not p.startswith(".")]
    items.sort()
    return [os.path.join(path, p) for p in items]

def oldest_age_s(paths):
    if not paths:
        return 0
    st = os.stat(paths[0])
    return max(0, int(time.time()) - int(st.st_mtime))

def main():
    ap = argparse.ArgumentParser()
    ap.add_argument("--queue", default="queue")
    ap.add_argument("--inflight", default="inflight")
    ap.add_argument("--done", default="done")
    args = ap.parse_args()

    q = list_msgs(args.queue)
    inf = list_msgs(args.inflight)
    done = list_msgs(args.done)
    print(f"queue_depth={len(q)} inflight={len(inf)} done={len(done)} oldest_queue_age_s={oldest_age_s(q)}")

if __name__ == "__main__":
    main()
EOF
```

## Steps

### 1) Enqueue Messages

```bash
python3 enqueue.py --type ok --n 5
python3 triage.py
```

Expected signals:

- queue_depth is 5

### 2) Consume Messages

```bash
python3 worker.py --max 5 | tee worker.log
python3 triage.py
```

Expected:

- worker.log includes `processed` lines
- queue_depth becomes 0, done becomes 5

### 3) Failure + Visibility Timeout (At-Least-Once)

Enqueue a poison message type:

```bash
python3 enqueue.py --type poison --n 1
python3 triage.py
```

Run worker configured to fail on that type:

```bash
python3 worker.py --max 1 --fail-on-type poison || true
python3 triage.py
sleep 3
python3 triage.py
```

Expected:

- worker exits with `processing_failed`
- message returns from inflight back to queue after visibility timeout (it is re-delivered)

### 4) Recovery

Process the message successfully by running without the failure rule:

```bash
python3 worker.py --max 1 | tee -a worker.log
python3 triage.py
```

Expected:

- done increases by 1

## Verify

```bash
python3 triage.py
grep -c '"event": "processed"' worker.log || true
```

Expected signals:

- queue_depth is 0
- processed log lines exist

## Cleanup

```bash
cd ~
rm -rf ~/work/devops-labs/mod21-queue
```

## Troubleshooting

### Symptom: queue depth stays non-zero

Diagnosis:

```bash
python3 triage.py
ls -la queue inflight done | head
```

Fix:

- run worker again and inspect for repeated failure on a specific message type

### Symptom: message never reappears after failure

Fix:

- ensure the visibility timeout logic runs (wait > 2 seconds) and re-run triage
