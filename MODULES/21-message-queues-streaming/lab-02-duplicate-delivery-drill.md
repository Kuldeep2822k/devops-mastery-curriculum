---
title: 'Duplicate Delivery Drill'
tags:
  - lab
  - messaging
  - idempotency
module: "21"
---

# Lab 02 — Duplicate Delivery Drill (Idempotency Keys)

## Goal

Prove why at-least-once delivery causes duplicates and how idempotency prevents bad side effects:

- simulate duplicate deliveries of the same event
- demonstrate double side effects without idempotency
- add an idempotency key store and prove duplicates are safe

## Prereqs

- `python3`

## Setup

```bash
mkdir -p ~/work/devops-labs/mod21-dup
cd ~/work/devops-labs/mod21-dup
mkdir -p out
```

Create a “charge” simulator:

```bash
cat > charge.py <<'EOF'
import argparse
import json
import os
import sqlite3
import time

def ensure_db(path):
    conn = sqlite3.connect(path)
    conn.execute("create table if not exists processed (id text primary key, ts integer)")
    conn.commit()
    return conn

def main():
    ap = argparse.ArgumentParser()
    ap.add_argument("--event-id", required=True)
    ap.add_argument("--amount", type=int, default=100)
    ap.add_argument("--ledger", default="out/ledger.txt")
    ap.add_argument("--idempotent", action="store_true")
    ap.add_argument("--db", default="out/processed.db")
    args = ap.parse_args()

    os.makedirs(os.path.dirname(args.ledger), exist_ok=True)

    if args.idempotent:
        conn = ensure_db(args.db)
        try:
            conn.execute("insert into processed(id, ts) values (?,?)", (args.event_id, int(time.time())))
            conn.commit()
        except sqlite3.IntegrityError:
            print(json.dumps({"event": "dedupe_skip", "id": args.event_id}, sort_keys=True))
            return 0

    with open(args.ledger, "a", encoding="utf-8") as f:
        f.write(f"id={args.event_id} amount={args.amount}\n")
    print(json.dumps({"event": "charged", "id": args.event_id, "amount": args.amount}, sort_keys=True))
    return 0

if __name__ == "__main__":
    raise SystemExit(main())
EOF
```

## Steps

### 1) Demonstrate Duplicate Side Effects (No Idempotency)

```bash
event_id="evt-123"
python3 charge.py --event-id "$event_id" --amount 100
python3 charge.py --event-id "$event_id" --amount 100
tail -n 5 out/ledger.txt
```

Expected:

- ledger shows two lines for the same id (double charge)

### 2) Add Idempotency and Prove Safety

Reset evidence:

```bash
rm -f out/ledger.txt out/processed.db
```

Process the same event twice with idempotency:

```bash
python3 charge.py --event-id "$event_id" --amount 100 --idempotent
python3 charge.py --event-id "$event_id" --amount 100 --idempotent
cat out/ledger.txt
```

Expected:

- first run emits `charged`
- second run emits `dedupe_skip`
- ledger contains exactly one line for the id

### 3) Controlled Failure Drill: “Lost ACK” Duplicate

Simulate “processed but ack lost” by reprocessing the same id:

```bash
python3 charge.py --event-id "evt-456" --amount 50 --idempotent
python3 charge.py --event-id "evt-456" --amount 50 --idempotent
grep -c 'id=evt-456' out/ledger.txt
```

Expected:

- count is 1

## Verify

```bash
test "$(grep -c '^id=evt-123 ' out/ledger.txt)" = "1"
test "$(grep -c '^id=evt-456 ' out/ledger.txt)" = "1"
```

Expected signals:

- duplicates do not produce duplicate side effects when idempotency is enabled

## Cleanup

```bash
cd ~
rm -rf ~/work/devops-labs/mod21-dup
```

## Troubleshooting

### Symptom: dedupe does not work

Fix:

- ensure you keep the idempotency store on durable storage in real systems
- ensure you write the idempotency record before the side effect when possible
