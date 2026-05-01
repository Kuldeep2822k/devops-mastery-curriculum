---
title: 'Add Policy Gates'
tags:
  - lab
  - platform-engineering
  - policy
module: "24"
---

# Lab 02 — Add Policy Gates (Repo Guardrails)

## Goal

Add baseline guardrails that prevent common operational mistakes:

- require ownership metadata
- require an SLO doc and a runbook
- forbid mutable base image tags like `:latest`
- enforce a build contract (`make ci`)

## Prereqs

- `bash`
- `git`
- `make`
- `python3`

## Setup

```bash
mkdir -p ~/work/devops-labs/mod24-policy
cd ~/work/devops-labs/mod24-policy
git init
mkdir -p docs scripts src dist
```

Create a minimal build contract:

```bash
cat > src/app.py <<'EOF'
print("ok")
EOF
```

```bash
cat > Makefile <<'EOF'
SHELL := /bin/bash
.PHONY: ci
ci:
	python3 -m py_compile src/app.py
	python3 src/app.py | grep -q '^ok$'
	mkdir -p dist
	printf "ok\n" > dist/app.txt
EOF
```

Create a Dockerfile with a policy violation:

```bash
cat > Dockerfile <<'EOF'
FROM python:latest
WORKDIR /app
COPY src/app.py /app/app.py
CMD ["python3","/app/app.py"]
EOF
```

Create the policy gate:

```bash
cat > scripts/policy_gate.sh <<'EOF'
set -eu

fail=0

if [ ! -f OWNER ]; then
  echo "policy: missing OWNER"
  fail=1
fi

if [ ! -f docs/SLO.md ]; then
  echo "policy: missing docs/SLO.md"
  fail=1
fi

if [ ! -f docs/runbook.md ]; then
  echo "policy: missing docs/runbook.md"
  fail=1
fi

if [ -f Dockerfile ] && grep -E -n '^FROM .+:latest\\b' Dockerfile >/dev/null 2>&1; then
  echo "policy: forbidden base image tag :latest"
  fail=1
fi

make ci >/dev/null 2>&1 || fail=1

test -s dist/app.txt || fail=1

exit "$fail"
EOF
chmod +x scripts/policy_gate.sh
```

Commit baseline:

```bash
git add .
git commit -m "chore: add policy gate"
```

## Steps

### 1) Observe Policy Gate Failure

```bash
./scripts/policy_gate.sh || true
```

Expected:

- reports missing OWNER and docs
- reports forbidden `:latest`
- exits non-zero

### 2) Fix Violations

Add required docs:

```bash
printf "team=platform\n" > OWNER
cat > docs/SLO.md <<'EOF'
# SLO

- SLI:
- Target:
- Window:
EOF
cat > docs/runbook.md <<'EOF'
# Runbook

## Triage

## Mitigations
EOF
```

Pin base image:

```bash
cat > Dockerfile <<'EOF'
FROM python:3.12-slim
WORKDIR /app
COPY src/app.py /app/app.py
CMD ["python3","/app/app.py"]
EOF
```

Re-run gate:

```bash
./scripts/policy_gate.sh
```

Expected:

- exit code 0

## Verify

```bash
./scripts/policy_gate.sh && echo "ok: policy gate passes"
```

Expected signals:

- prints `ok: policy gate passes`

## Cleanup

```bash
cd ~
rm -rf ~/work/devops-labs/mod24-policy
```

## Troubleshooting

### Symptom: policy gate blocks legitimate changes

Fix:

- start with a small baseline set of rules and iterate
- add explicit allowlists for known-safe cases (documented)
