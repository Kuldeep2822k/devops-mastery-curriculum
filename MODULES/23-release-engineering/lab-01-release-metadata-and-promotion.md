---
title: 'Release Metadata and Promotion'
tags:
  - lab
  - release-engineering
module: "23"
---

# Lab 01 — Release Metadata and Promotion (Build Once, Promote Many)

## Goal

Practice release hygiene locally:

- build a single artifact
- create release metadata (version, commit, artifact digest)
- promote the same artifact into multiple environments without rebuilding
- detect and prevent “wrong revision promoted”

## Prereqs

- `bash`
- `git`
- `sha256sum`

## Setup

```bash
mkdir -p ~/work/devops-labs/mod23-release
cd ~/work/devops-labs/mod23-release
git init
mkdir -p src dist env/staging env/prod releases
```

Create a tiny “app”:

```bash
cat > src/app.sh <<'EOF'
set -eu
printf "hello version=%s commit=%s\n" "${APP_VERSION:-dev}" "${COMMIT_SHA:-unknown}"
EOF
chmod +x src/app.sh
```

Create a build script:

```bash
cat > build.sh <<'EOF'
set -eu
version="${1:?version required}"
commit="$(git rev-parse --short HEAD 2>/dev/null || echo unknown)"
mkdir -p dist
APP_VERSION="$version" COMMIT_SHA="$commit" ./src/app.sh > dist/app.txt
sha="$(sha256sum dist/app.txt | awk '{print $1}')"
printf '{"version":"%s","commit":"%s","artifact_sha256":"%s"}\n' "$version" "$commit" "$sha" > dist/release.json
EOF
chmod +x build.sh
```

Commit baseline:

```bash
git add .
git commit -m "chore: initial release skeleton"
```

## Steps

### 1) Build a Release Artifact

```bash
./build.sh 1.0.0
cat dist/app.txt
cat dist/release.json
```

Expected signals:

- `dist/app.txt` exists and is non-empty
- `dist/release.json` contains version, commit, and sha256

### 2) “Publish” the Release (Immutable Folder by Digest)

```bash
sha="$(python3 -c 'import json; print(json.load(open(\"dist/release.json\"))[\"artifact_sha256\"])')"
release_dir="releases/${sha}"
mkdir -p "$release_dir"
cp dist/app.txt dist/release.json "$release_dir"/
ls -la "$release_dir"
```

Expected:

- a release directory exists named by sha256

### 3) Promote to Staging and Prod Without Rebuilding

Promote means: copy the same artifact and metadata:

```bash
cp "$release_dir/app.txt" "$release_dir/release.json" env/staging/
cp "$release_dir/app.txt" "$release_dir/release.json" env/prod/
```

Verify both environments have the same artifact digest:

```bash
sha256sum env/staging/app.txt env/prod/app.txt
```

Expected:

- both digests match

### 4) Controlled Failure: Wrong Artifact Promoted

Change the app and rebuild “same version” (simulating a bad practice):

```bash
printf '\nprintf "build_variant=%s\n" "${BUILD_VARIANT:-a}"\n' >> src/app.sh
git add src/app.sh
git commit -m "feat: change output"
./build.sh 1.0.0
bad_sha="$(python3 -c 'import json; print(json.load(open(\"dist/release.json\"))[\"artifact_sha256\"])')"
mkdir -p "releases/${bad_sha}"
cp dist/app.txt dist/release.json "releases/${bad_sha}/"
cp "releases/${bad_sha}/app.txt" env/prod/app.txt
```

Detect mismatch:

```bash
sha256sum env/staging/app.txt env/prod/app.txt || true
```

Recovery: promote the original digest again:

```bash
cp "$release_dir/app.txt" env/prod/app.txt
sha256sum env/staging/app.txt env/prod/app.txt
```

## Verify

```bash
sha256sum env/staging/app.txt env/prod/app.txt
python3 - <<'PY'
import json
st=json.load(open("env/staging/release.json"))
pd=json.load(open("env/prod/release.json"))
print("ok" if st["artifact_sha256"]==pd["artifact_sha256"] else "mismatch")
PY
```

Expected signals:

- staging and prod digests match
- script prints `ok`

## Cleanup

```bash
cd ~
rm -rf ~/work/devops-labs/mod23-release
```

## Troubleshooting

### Symptom: “same version” has different digests

Fix:

- treat versions as immutable; never rebuild and re-publish the same version
- promote by digest (or release directory keyed by digest) instead of tag/version alone
