---
title: "Lab 02: Regression Hunt (Bisect + Revert)"
tags:
  - lab
  - git
  - bisect
  - incident
module: "03"
---

# Lab 02 — Regression Hunt (Bisect + Revert)

## Goal

Practice finding and undoing a regression under incident-like constraints:

- create a small history with a hidden break
- use `git bisect` with a deterministic test to identify the first bad commit
- rollback safely using `git revert` (shared-history-safe)
- verify recovery and write prevention steps

## Prereqs

- Git installed
- `bash` and `python3` available

## Setup

Create a new repo:

```bash
mkdir -p ~/work/devops-labs/mod03-bisect
cd ~/work/devops-labs/mod03-bisect
git init
```

Create a simple script with a test:

```bash
cat > calc.py <<'EOF'
import sys

def add(a, b):
    return a + b

if __name__ == "__main__":
    a = int(sys.argv[1])
    b = int(sys.argv[2])
    print(add(a, b))
EOF

cat > test.sh <<'EOF'
set -eu
out="$(python3 calc.py 2 2)"
test "$out" = "4"
EOF
chmod +x test.sh
```

Commit baseline:

```bash
git add calc.py test.sh
git commit -m "chore: add calc and deterministic test"
```

Create a few commits:

```bash
perl -0777 -pe 's/return a \\+ b/return a + b + 0/' -i calc.py
git add calc.py
git commit -m "refactor: no-op change"

perl -0777 -pe 's/return a \\+ b \\+ 0/return a + b + 1/' -i calc.py
git add calc.py
git commit -m "feat: add bias (regression)"

perl -0777 -pe 's/print\\(add\\(a, b\\)\\)/print(add(a, b))/' -i calc.py
git add calc.py
git commit -m "chore: formatting"
```

Confirm the repo is currently broken:

```bash
./test.sh || true
```

Expected signal:

- test fails

## Steps

### 1) Identify a Known Good Commit

List history:

```bash
git log --oneline --reverse
```

Pick the first commit as known good (the baseline).

### 2) Run Bisect

```bash
git bisect start
git bisect bad
git bisect good HEAD~3
```

Now repeatedly run the test and mark good/bad:

```bash
./test.sh && git bisect good || git bisect bad
```

Repeat until bisect identifies the first bad commit.

Capture result:

```bash
git bisect log
git show --stat
```

### 3) Return to Normal State

```bash
git bisect reset
```

### 4) Roll Back Safely Using Revert

Revert the identified bad commit (use the hash from bisect result):

```bash
bad_commit="<PASTE_HASH_HERE>"
git revert "$bad_commit"
```

If your environment does not allow interactive commit editing, accept default message.

## Verify

```bash
./test.sh
python3 calc.py 2 2
git log --oneline -n 10
```

Expected signals:

- test passes
- output is 4
- a revert commit exists

## Cleanup

Optional:

```bash
cd ~
rm -rf ~/work/devops-labs/mod03-bisect
```

## Troubleshooting

### Symptom: bisect gives wrong commit

Diagnosis:

- your test is not deterministic
- your good/bad boundaries are wrong

Fix:

- tighten the test to a stable signal
- ensure the good commit actually passes and the bad commit fails

### Symptom: revert causes conflicts

Diagnosis:

- later commits touched the same lines

Fix:

- resolve conflicts by restoring intended behavior
- verify with the test after resolving

## Why This Matters in Production

- Many incidents are regressions introduced by changes that “look small”.
- Bisect is one of the fastest ways to find the causal commit when you have a stable test/signal.
- Revert preserves audit trail and works safely on shared branches.

## What to Write in a Runbook

- how to define a “good/bad” signal for bisect (tests, smoke check, log pattern)
- bisect procedure and evidence to capture
- revert procedure and verification signals

## Definition of Done

- You can bisect to a first-bad commit using a deterministic signal.
- You can revert that commit and verify recovery.
