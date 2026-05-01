---
title: "Lab 02: CI Security Gates (Dependency Manifest + Policy Checks)"
tags:
  - lab
  - security
  - ci
  - supply-chain
module: "10"
---

# Lab 02 — CI Security Gates (Dependency Manifest + Policy Checks)

## Goal

Build a simple “security gate” that can run in CI:

- generate a dependency manifest file
- run policy checks:
  - block forbidden patterns (secrets)
  - block forbidden base tags like `:latest` in Dockerfiles
  - require a lockfile/manifest file exists

This lab is tool-agnostic and does not assume external scanners are installed.

## Prereqs

- git
- bash

## Setup

Create a lab repo:

```bash
mkdir -p ~/work/devops-labs/mod10-ci-gates
cd ~/work/devops-labs/mod10-ci-gates
git init
```

Create a tiny “dependency manifest” (example: a requirements file):

```bash
cat > requirements.txt <<'EOF'
requests==2.31.0
EOF
```

Create a Dockerfile that intentionally violates policy:

```bash
cat > Dockerfile <<'EOF'
FROM python:3.12
WORKDIR /app
COPY requirements.txt /app/requirements.txt
EOF
```

Create a gate script:

```bash
mkdir -p scripts
cat > scripts/security_gate.sh <<'EOF'
set -eu

fail=0

if [ ! -f requirements.txt ]; then
  echo "gate: missing requirements.txt"
  fail=1
fi

if [ -f Dockerfile ]; then
  if grep -E -n '^FROM .+:latest\\b' Dockerfile >/dev/null 2>&1; then
    echo "gate: forbidden base image tag :latest"
    fail=1
  fi
fi

patterns='(AKIA[0-9A-Z]{16})|(-----BEGIN (RSA|EC|OPENSSH) PRIVATE KEY-----)|(password\\s*=\\s*[^\\s]+)|(token\\s*=\\s*[^\\s]+)'
if grep -R -n -E "$patterns" . >/dev/null 2>&1; then
  echo "gate: suspected secret pattern in repo"
  grep -R -n -E "$patterns" . | awk -F: '{print $1 ":" $2 ":<redacted>"}' | head -n 10
  fail=1
fi

exit "$fail"
EOF
chmod +x scripts/security_gate.sh
```

Commit baseline:

```bash
git add .
git commit -m "chore: add dependency manifest and security gate"
```

## Steps

### 1) Run Gate and Observe Failure

```bash
./scripts/security_gate.sh || true
```

Expected:

- passes lockfile/manifest check
- may fail only if you set `:latest` (optional) or if secrets exist

To exercise forbidden tag rule, change Dockerfile to use latest:

```bash
cat > Dockerfile <<'EOF'
FROM python:latest
WORKDIR /app
COPY requirements.txt /app/requirements.txt
EOF
./scripts/security_gate.sh || true
```

Expected:

- gate fails due to forbidden :latest

### 2) Fix Policy Violations

Pin base image:

```bash
cat > Dockerfile <<'EOF'
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt /app/requirements.txt
EOF
./scripts/security_gate.sh
```

Expected:

- gate passes

## Verify

- gate passes on clean repo
- gate fails on forbidden patterns

## Cleanup

```bash
cd ~
rm -rf ~/work/devops-labs/mod10-ci-gates
```

## Troubleshooting

### Symptom: gate reports a suspected secret but you can’t find it safely

Fix:

- inspect the file locally and remove the pattern without copying the value into chat/logs
- narrow the scan scope to only files you intend to ship (exclude build outputs)

### Symptom: false positives (e.g., docs or examples)

Fix:

- tighten patterns to match only high-confidence formats
- use allowlists for known-safe example strings

## Why This Matters in Production

- Basic policy gates stop obvious high-severity mistakes early.
- Even without advanced tools, you can enforce baseline hygiene and reduce incident frequency.

## Definition of Done

- you have at least two policies enforced by a script that can run in CI.
