---
title: '09-gitops-platform: Steps'
tags:
  - project
---

# 09-gitops-platform — Steps

## Prereqs

- `git`, `sha256sum`

## Setup

```bash
mkdir -p ~/work/devops-labs/09-gitops-platform
cd ~/work/devops-labs/09-gitops-platform
git init
mkdir -p releases env/dev env/prod
```

Create a release artifact and metadata:

```bash
printf 'version=1.0.0
' > app.txt
sha=$(sha256sum app.txt | awk '{print $1}')
mkdir -p "releases/$sha"
cp app.txt "releases/$sha/app.txt"
printf '{"version":"1.0.0","sha256":"%s"}
' "$sha" > "releases/$sha/release.json"
```

## Steps

### 1) Promote to dev

```bash
cp "releases/$sha/release.json" env/dev/release.json
cp "releases/$sha/app.txt" env/dev/app.txt
sha256sum env/dev/app.txt
```

### 2) Gate Promotion to prod

Create a simple gate requiring version format and checksum match:

```bash
cat > gate.sh <<'EOF'
set -eu
test -s env/dev/release.json
sha=$(python3 -c 'import json; print(json.load(open("env/dev/release.json"))["sha256"])')
calc=$(sha256sum env/dev/app.txt | awk '{print $1}')
test "$sha" = "$calc"
EOF
chmod +x gate.sh
./gate.sh
```

Promote:

```bash
cp env/dev/release.json env/prod/release.json
cp env/dev/app.txt env/prod/app.txt
```

### 3) Rollback Drill

Introduce a bad dev change and prevent prod promotion:

```bash
printf 'corrupt
' >> env/dev/app.txt
./gate.sh || true
```

Recover by re-syncing dev from the release dir:

```bash
cp "releases/$sha/app.txt" env/dev/app.txt
./gate.sh
```

## Verify

```bash
sha256sum env/dev/app.txt env/prod/app.txt
```

Expected:

- digests match when promoted correctly

## Cleanup

```bash
cd ~
rm -rf ~/work/devops-labs/09-gitops-platform
```

## Troubleshooting

- Keep promotion immutable: promote by digest/hash, not by mutable tags.
