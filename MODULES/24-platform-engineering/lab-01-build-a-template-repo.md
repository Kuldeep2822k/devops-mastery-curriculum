---
title: 'Build a Template Repo'
tags:
  - lab
  - platform-engineering
  - templates
module: "24"
---

# Lab 01 — Build a Template Repo (Golden Path Skeleton)

## Goal

Create a minimal “golden path” template repository that standardizes:

- a build contract (`make ci`)
- artifact output (`dist/`)
- basic service health check
- operational docs: runbook + ADR placeholders

## Prereqs

- `git`
- `make`
- `python3`

## Setup

```bash
mkdir -p ~/work/devops-labs/mod24-template
cd ~/work/devops-labs/mod24-template
git init
mkdir -p src dist docs
```

Create a tiny service:

```bash
cat > src/app.py <<'EOF'
import os

def main():
    version = os.getenv("APP_VERSION", "dev")
    return f"ok version={version}\n"

if __name__ == "__main__":
    print(main(), end="")
EOF
```

Create tests:

```bash
cat > test.sh <<'EOF'
set -eu
out="$(python3 src/app.py)"
printf "%s" "$out" | grep -q '^ok version='
EOF
chmod +x test.sh
```

Create a Makefile contract:

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

Create docs:

```bash
cat > docs/runbook.md <<'EOF'
# Runbook

## Service Summary

## Triage

## Mitigations

## Rollback
EOF
```

```bash
cat > docs/adr-0001-template.md <<'EOF'
# ADR 0001

## Decision

## Context

## Alternatives

## Consequences
EOF
```

Commit baseline:

```bash
git add .
git commit -m "chore: template repo skeleton"
```

## Steps

### 1) Run the Golden Path Contract

```bash
make ci
ls -la dist
```

Expected:

- dist/app.txt and dist/manifest.txt exist and are non-empty

### 2) Break/Fix: Missing Artifact

```bash
rm -f dist/app.txt
make smoke || true
make package
make smoke
```

Expected:

- smoke fails when artifact missing, succeeds after package rebuild

## Verify

```bash
make ci
test -s dist/app.txt
test -s dist/manifest.txt
test -s docs/runbook.md
test -s docs/adr-0001-template.md
```

Expected signals:

- all checks pass

## Cleanup

```bash
cd ~
rm -rf ~/work/devops-labs/mod24-template
```

## Troubleshooting

### Symptom: make ci fails in a clean environment

Fix:

- keep tooling requirements minimal and explicit
- prefer portable Make targets over hidden local scripts
