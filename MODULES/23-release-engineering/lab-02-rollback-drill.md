---
title: 'Rollback Drill'
tags:
  - lab
  - release-engineering
  - rollback
module: "23"
---

# Lab 02 — Rollback Drill (Symlink Deploy + Verification Window)

## Goal

Practice a safe rollback workflow using explicit signals:

- deploy v1 (known good)
- deploy v2 (known bad)
- detect failure quickly
- rollback by switching to previous artifact
- verify recovery for a defined window

## Prereqs

- `bash`
- `sha256sum`

## Setup

```bash
mkdir -p ~/work/devops-labs/mod23-rollback
cd ~/work/devops-labs/mod23-rollback
mkdir -p releases prod
```

Create a “health check” runner:

```bash
cat > run.sh <<'EOF'
set -eu
dir="${1:?release dir required}"
test -x "$dir/app.sh"
"$dir/app.sh"
EOF
chmod +x run.sh
```

Create v1 and v2 releases:

```bash
cat > releases/v1/app.sh <<'EOF'
set -eu
echo "ok version=v1"
EOF
chmod +x releases/v1/app.sh

cat > releases/v2/app.sh <<'EOF'
set -eu
echo "fail version=v2"
exit 2
EOF
chmod +x releases/v2/app.sh
```

Set prod “current” pointer:

```bash
ln -sfn "$PWD/releases/v1" prod/current
```

## Steps

### 1) Deploy v1 and Verify

```bash
./run.sh prod/current
```

Expected:

- prints `ok version=v1`

Record digest for the release script:

```bash
sha256sum prod/current/app.sh | tee prod/current.sha256
```

### 2) Deploy v2 (Bad) and Detect Failure

```bash
ln -sfn "$PWD/releases/v2" prod/current
./run.sh prod/current || true
```

Expected:

- non-zero exit
- output indicates failure

### 3) Roll Back to v1

```bash
ln -sfn "$PWD/releases/v1" prod/current
./run.sh prod/current
```

Expected:

- prints `ok version=v1`

### 4) Verification Window (Stability Check)

```bash
for i in $(seq 1 5); do ./run.sh prod/current; sleep 1; done
```

Expected:

- all runs succeed

## Verify

```bash
./run.sh prod/current | grep -q 'ok version=v1' && echo "ok: rolled back"
sha256sum -c prod/current.sha256
```

Expected signals:

- final line prints `ok: rolled back`
- checksum validation prints `OK`

## Cleanup

```bash
cd ~
rm -rf ~/work/devops-labs/mod23-rollback
```

## Troubleshooting

### Symptom: rollback didn’t change behavior

Fix:

- confirm `prod/current` points to the intended release:

```bash
readlink -f prod/current
```
