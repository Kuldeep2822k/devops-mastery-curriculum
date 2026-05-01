---
title: "Lab 01: Portable Pipeline (Makefile + Local Runner Simulation)"
tags:
  - lab
  - ci
  - local-first
module: "04"
---

# Lab 01 — Portable Pipeline (Makefile + Local Runner Simulation)

## Goal

Build a local-first pipeline that:

- runs via Make targets locally
- can be executed in a clean “CI-like” environment
- produces an artifact (`dist/app.txt`) and a manifest file
- includes a smoke test that fails-fast on broken artifacts

## Prereqs

- `make`
- `python3`
- Docker (recommended for local runner simulation)

## Setup

Create a lab repo:

```bash
mkdir -p ~/work/devops-labs/mod04-ci-portable
cd ~/work/devops-labs/mod04-ci-portable
git init
```

Create a tiny app and tests:

```bash
mkdir -p src dist

cat > src/app.py <<'EOF'
import os

def main():
    version = os.getenv("APP_VERSION", "dev")
    out = f"hello version={version}\n"
    return out

if __name__ == "__main__":
    print(main(), end="")
EOF

cat > test.sh <<'EOF'
set -eu
out="$(python3 src/app.py)"
printf "%s" "$out" | grep -q 'hello version='
EOF
chmod +x test.sh
```

Create a Makefile pipeline contract:

```bash
cat > Makefile <<'EOF'
SHELL := /bin/bash
.PHONY: lint test build package smoke clean ci

lint:
	python3 -m py_compile src/app.py

test:
	./test.sh

build:
	mkdir -p dist
	APP_VERSION=$$(git rev-parse --short HEAD 2>/dev/null || echo dev) python3 src/app.py > dist/app.txt

package: build
	printf "commit=%s\n" "$$(git rev-parse HEAD 2>/dev/null || echo unknown)" > dist/manifest.txt

smoke: package
	test -s dist/app.txt
	grep -q '^commit=' dist/manifest.txt

clean:
	rm -rf dist

ci: lint test smoke
EOF
```

Initial commit:

```bash
git add .
git commit -m "chore: add portable pipeline contract"
```

## Steps

### 1) Run Locally

```bash
make ci
ls -la dist
```

Expected signals:

- `make ci` succeeds
- `dist/app.txt` and `dist/manifest.txt` exist and are non-empty

### 2) Simulate a Clean Runner (Container)

This catches missing tools and hidden host dependencies.

```bash
docker run --rm -t \
  -v "$PWD":/work -w /work \
  python:3.12-slim \
  bash -lc "apt-get update >/dev/null && apt-get install -y --no-install-recommends make git >/dev/null && make ci && ls -la dist"
```

Expected signals:

- pipeline succeeds in a clean container after installing make/git

### 3) Break the Artifact and Observe Fail-Fast

Break build output intentionally:

```bash
rm -f dist/app.txt
make smoke || true
```

Expected signal:

- smoke step fails due to missing artifact

Fix by regenerating:

```bash
make package
make smoke
```

## Verify

```bash
test -s dist/app.txt
grep -q '^commit=' dist/manifest.txt
cat dist/app.txt
cat dist/manifest.txt
```

Expected signals:

- artifact contains version string
- manifest contains commit hash

## Cleanup

```bash
make clean
git status
```

Expected signals:

- `dist/` removed
- working tree clean (or only expected changes)

## Troubleshooting

### Symptom: “works locally, fails in container”

Diagnosis:

- you relied on host tools or environment variables not present in CI

Fix:

- add explicit tool installation in runner
- keep build contract minimal and explicit

### Symptom: missing dist/ in later steps

Diagnosis:

- build step not producing expected outputs
- cleanup step ran earlier than expected

Fix:

- ensure `package` depends on `build`
- ensure smoke depends on `package`

## Why This Matters in Production

- Portable pipelines reduce CI migration risk and reduce pipeline MTTR.
- Local runner simulation is the fastest way to reproduce CI failures without waiting for remote runs.

## What to Write in a Runbook

- how to run `make ci` locally
- how to reproduce in a clean runner container
- what artifacts are expected and where they live

## Definition of Done

- `make ci` works locally and in a clean container.
- smoke step fails when artifacts are missing and succeeds when restored.
