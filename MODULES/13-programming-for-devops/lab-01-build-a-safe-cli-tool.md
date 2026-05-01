---
title: "Lab 01: Build a Safe CLI Tool (Dry-Run + Limits)"
tags:
  - lab
  - programming
  - automation
module: "13"
---

# Lab 01 — Build a Safe CLI Tool (Dry-Run + Limits)

## Goal

Build a small CLI tool that:

- reads a directory of files
- reports how many files match a predicate
- optionally deletes them
- supports dry-run and max-delete limits

This is a model for safe operational automation.

## Prereqs

- `python3`

## Setup

```bash
mkdir -p ~/work/devops-labs/mod13-cli
cd ~/work/devops-labs/mod13-cli
```

Create sample data:

```bash
mkdir -p data
touch data/a.tmp data/b.tmp data/keep.txt
```

Create `cleanup.py`:

```bash
cat > cleanup.py <<'EOF'
import argparse
import os
import sys

def list_targets(root, suffix):
    targets = []
    for name in os.listdir(root):
        p = os.path.join(root, name)
        if os.path.isfile(p) and name.endswith(suffix):
            targets.append(p)
    targets.sort()
    return targets

def main():
    ap = argparse.ArgumentParser()
    ap.add_argument("--root", required=True)
    ap.add_argument("--suffix", default=".tmp")
    ap.add_argument("--max-delete", type=int, default=10)
    ap.add_argument("--apply", action="store_true")
    args = ap.parse_args()

    targets = list_targets(args.root, args.suffix)
    print(f"found={len(targets)} root={args.root} suffix={args.suffix}")

    if not args.apply:
        for p in targets[:args.max_delete]:
            print(f"dry_run_delete={p}")
        return 0

    n = 0
    for p in targets:
        if n >= args.max_delete:
            print("limit_reached", file=sys.stderr)
            return 2
        os.unlink(p)
        print(f"deleted={p}")
        n += 1
    return 0

if __name__ == "__main__":
    raise SystemExit(main())
EOF
```

## Steps

### 1) Dry-Run

```bash
python3 cleanup.py --root data --suffix .tmp
ls -la data
```

Expected:

- tool prints `dry_run_delete=...`
- files still exist

### 2) Apply With Limits

```bash
python3 cleanup.py --root data --suffix .tmp --max-delete 1 --apply || true
ls -la data
```

Expected:

- one file deleted
- tool exits non-zero if limit reached before deleting all

### 3) Apply Remaining

```bash
python3 cleanup.py --root data --suffix .tmp --max-delete 10 --apply
ls -la data
```

Expected:

- tmp files removed
- keep.txt remains

## Verify

- dry-run is safe
- apply is bounded
- exit codes communicate partial completion

## Cleanup

```bash
cd ~
rm -rf ~/work/devops-labs/mod13-cli
```

## Troubleshooting

### Symptom: tool deletes too much

Fix:

- keep `--apply` required for destruction
- lower `--max-delete` and validate dry-run output before apply

### Symptom: script exits 2 unexpectedly

Fix:

- this indicates limit was reached; increase `--max-delete` or run multiple bounded passes

## Why This Matters in Production

- “bounded destructive actions” prevents catastrophic mistakes.
- dry-run builds trust in automation.

## Definition of Done

- your tool is safe by default and destructive only with explicit flag.
