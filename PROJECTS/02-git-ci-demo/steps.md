---
title: '02-git-ci-demo: Steps'
tags:
  - project
---

# 02-git-ci-demo — Steps

## Prereqs

- `git`, `make`, `python3`

## Setup

```bash
mkdir -p ~/work/devops-labs/02-git-ci-demo
cd ~/work/devops-labs/02-git-ci-demo
git init
mkdir -p src dist
```

Create a small program + test:

```bash
cat > src/app.py <<'EOF'
print("ok")
EOF
cat > test.sh <<'EOF'
set -eu
python3 src/app.py | grep -q '^ok$'
EOF
chmod +x test.sh
```

Create a Makefile pipeline:

```bash
cat > Makefile <<'EOF'
SHELL := /bin/bash
.PHONY: lint test build package ci

lint:
	python3 -m py_compile src/app.py

test:
	./test.sh

build:
	mkdir -p dist
	python3 src/app.py > dist/app.txt

package: build
	printf "commit=%s
" "$$(git rev-parse --short HEAD 2>/dev/null || echo unknown)" > dist/manifest.txt

ci: lint test package
EOF
```

Commit baseline:

```bash
git add .
git commit -m "chore: ci demo"
```

## Steps

### 1) Run Pipeline

```bash
make ci
ls -la dist
cat dist/manifest.txt
```

### 2) Break/Fix: Make CI Fail and Recover with Revert

Break test:

```bash
printf 'print("broken")
' > src/app.py
make ci || true
```

Fix via git revert:

```bash
git add src/app.py
git commit -m "break: output"
git revert HEAD --no-edit
make ci
```

## Verify

```bash
make ci
grep -q '^commit=' dist/manifest.txt && echo ok
```

## Cleanup

```bash
cd ~
rm -rf ~/work/devops-labs/02-git-ci-demo
```

## Troubleshooting

- If `git commit` is blocked by config, set user.name/user.email locally for this repo.
