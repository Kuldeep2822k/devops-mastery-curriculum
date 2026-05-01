---
title: "Lab 01: Secret Leak Prevention (Git Hook + Scanner Script)"
tags:
  - lab
  - security
  - secrets
  - git
module: "10"
---

# Lab 01 — Secret Leak Prevention (Git Hook + Scanner Script)

## Goal

Create a simple local guardrail that blocks commits containing obvious secret patterns.

This is not a replacement for professional scanners, but it builds the operational habit:

- scan before you commit
- rotate secrets when leaks happen

## Prereqs

- git installed

## Setup

Create a lab repo:

```bash
mkdir -p ~/work/devops-labs/mod10-secret-guard
cd ~/work/devops-labs/mod10-secret-guard
git init
```

Create a simple scanner script:

```bash
mkdir -p scripts
cat > scripts/secret_scan.sh <<'EOF'
set -eu

fail=0

patterns='(AKIA[0-9A-Z]{16})|(-----BEGIN (RSA|EC|OPENSSH) PRIVATE KEY-----)|(password\\s*=\\s*[^\\s]+)|(token\\s*=\\s*[^\\s]+)'

files="$(git diff --cached --name-only)"
for f in $files; do
  test -f "$f" || continue
  if grep -E -n "$patterns" "$f" >/dev/null 2>&1; then
    echo "secret_scan: suspected secret pattern in $f"
    grep -E -n "$patterns" "$f" | head -n 5
    fail=1
  fi
done

exit "$fail"
EOF
chmod +x scripts/secret_scan.sh
```

Install a pre-commit hook that runs the scanner:

```bash
cat > .git/hooks/pre-commit <<'EOF'
#!/bin/sh
set -eu
./scripts/secret_scan.sh
EOF
chmod +x .git/hooks/pre-commit
```

Commit a safe file:

```bash
echo "ok" > README.md
git add README.md
git commit -m "chore: init"
```

## Steps

### 1) Prove the Hook Blocks a Secret-Like Commit

Create a file with a fake secret pattern:

```bash
cat > bad.env <<'EOF'
password=<REDACTED>
EOF
git add bad.env
git commit -m "chore: add env" || true
```

Expected signal:

- commit blocked by hook

### 2) Fix by Removing the Secret

```bash
cat > bad.env <<'EOF'
password=REDACTED
EOF
git add bad.env
git commit -m "chore: add env (redacted)"
```

## Verify

```bash
git log --oneline -n 5
```

Expected:

- only redacted version is committed

## Cleanup

```bash
cd ~
rm -rf ~/work/devops-labs/mod10-secret-guard
```

## Troubleshooting

### Symptom: hook doesn’t run

Fix:

- ensure `.git/hooks/pre-commit` is executable

### Symptom: false positives

Fix:

- tighten patterns and focus on high-signal matches
- prefer professional scanners in real repos

## Why This Matters in Production

- The cheapest security incident is the one you prevent before a push.
- Secret rotation is expensive; preventing commits reduces incident frequency.

## Definition of Done

- hook blocks a secret-like pattern and allows clean commits.
