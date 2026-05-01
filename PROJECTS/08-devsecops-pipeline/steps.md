---
title: '08-devsecops-pipeline: Steps'
tags:
  - project
---

# 08-devsecops-pipeline — Steps

## Prereqs

- `bash`, `git`, `python3`, `sha256sum`

## Setup

```bash
mkdir -p ~/work/devops-labs/08-devsecops-pipeline
cd ~/work/devops-labs/08-devsecops-pipeline
git init
mkdir -p src dist scripts
```

App + deps:

```bash
cat > src/app.py <<'EOF'
print("ok")
EOF
cat > requirements.txt <<'EOF'
requests==2.31.0
EOF
```

Build script (artifact + manifest):

```bash
cat > scripts/build.sh <<'EOF'
set -eu
mkdir -p dist
python3 src/app.py > dist/app.txt
sha=$(sha256sum dist/app.txt | awk '{print $1}')
printf "commit=%s
" "$(git rev-parse --short HEAD 2>/dev/null || echo unknown)" > dist/manifest.txt
printf "artifact_sha256=%s
" "$sha" >> dist/manifest.txt
EOF
chmod +x scripts/build.sh
```

Policy gate (redacts secret matches):

```bash
cat > scripts/policy_gate.sh <<'EOF'
set -eu
fail=0
patterns='(AKIA[0-9A-Z]{16})|(-----BEGIN (RSA|EC|OPENSSH) PRIVATE KEY-----)|(password\s*=\s*[^\s]+)|(token\s*=\s*[^\s]+)'
if grep -R -n -E "$patterns" . >/dev/null 2>&1; then
  echo "gate: suspected secret pattern"
  grep -R -n -E "$patterns" . | awk -F: '{print $1 ":" $2 ":<redacted>"}' | head -n 10
  fail=1
fi
./scripts/build.sh || fail=1
test -s dist/app.txt || fail=1
test -s dist/manifest.txt || fail=1
exit "$fail"
EOF
chmod +x scripts/policy_gate.sh
```

Commit:

```bash
git add .
git commit -m "chore: devsecops pipeline"
```

## Steps

### 1) Run Gate (Pass)

```bash
./scripts/policy_gate.sh && echo ok
```

### 2) Introduce a Policy Violation and Fix

```bash
printf 'password=<REDACTED>
' > bad.env
./scripts/policy_gate.sh || true
rm -f bad.env
./scripts/policy_gate.sh
```

## Verify

```bash
test -s dist/manifest.txt
grep -q '^artifact_sha256=' dist/manifest.txt && echo ok
```

## Cleanup

```bash
cd ~
rm -rf ~/work/devops-labs/08-devsecops-pipeline
```

## Troubleshooting

- If grep is too broad, narrow scope to tracked files only (`git ls-files`).
