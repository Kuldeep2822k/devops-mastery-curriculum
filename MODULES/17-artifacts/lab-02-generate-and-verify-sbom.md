---
title: 'Generate and Verify an SBOM'
tags:
  - lab
  - sbom
  - security
module: "17"
---

# Lab 02 — Generate and Verify an SBOM (Local Manifests)

## Goal

Generate a “good enough” SBOM-style manifest locally and verify it is:

- reproducible (same inputs → same manifest)
- stored alongside artifacts
- usable during incident response (what changed, what is vulnerable)

This lab does not assume a specific SBOM tool is installed. Instead, it produces:

- an application dependency manifest (`pip freeze`)
- a base image OS package manifest (`dpkg-query`) when available

## Prereqs

- Docker installed and working: `docker version`
- `sha256sum`

## Setup

```bash
mkdir -p ~/work/devops-labs/mod17-sbom
cd ~/work/devops-labs/mod17-sbom
mkdir -p dist
```

Create a tiny dependency set:

```bash
cat > requirements.txt <<'EOF'
requests==2.31.0
EOF
```

## Steps

### 1) Generate Dependency Manifest (App SBOM)

Use a clean container to avoid host drift:

```bash
docker run --rm -t -v "$PWD":/work -w /work python:3.12-slim \
  bash -lc "python -m venv .venv && . .venv/bin/activate && pip install -r requirements.txt >/dev/null && pip freeze | sort" \
  > dist/sbom-python.txt
```

Expected signals:

- `dist/sbom-python.txt` exists and contains `requests==2.31.0`

### 2) Generate Base Image Package Manifest (OS “SBOM”)

This is a useful approximation for incident triage when SBOM tooling is not yet standardized.

```bash
docker run --rm -t python:3.12-slim bash -lc \
  "command -v dpkg-query >/dev/null 2>&1 && dpkg-query -W -f='${Package}=${Version}\n' | sort || true" \
  > dist/sbom-os.txt
```

Expected:

- `dist/sbom-os.txt` exists (may be empty if dpkg-query not present)

### 3) Create a Single “Artifact Bundle” SBOM File

```bash
printf "source_image=python:3.12-slim\n" > dist/sbom.txt
printf "requirements_sha256=%s\n" "$(sha256sum requirements.txt | awk '{print $1}')" >> dist/sbom.txt
printf "\n# python\n" >> dist/sbom.txt
cat dist/sbom-python.txt >> dist/sbom.txt
printf "\n# os\n" >> dist/sbom.txt
cat dist/sbom-os.txt >> dist/sbom.txt
```

### 4) Prove the Manifest Changes When Dependencies Change

```bash
cp dist/sbom-python.txt dist/sbom-python.before.txt
printf '\nurllib3==2.2.2\n' >> requirements.txt
docker run --rm -t -v "$PWD":/work -w /work python:3.12-slim \
  bash -lc "python -m venv .venv && . .venv/bin/activate && pip install -r requirements.txt >/dev/null && pip freeze | sort" \
  > dist/sbom-python.txt
diff -u dist/sbom-python.before.txt dist/sbom-python.txt || true
```

Expected:

- diff shows dependency set changed

## Verify

```bash
test -s dist/sbom-python.txt
grep -q '^requests==2.31.0$' dist/sbom-python.txt
test -s dist/sbom.txt
grep -q '^requirements_sha256=' dist/sbom.txt
```

Expected signals:

- SBOM files exist and are non-empty
- the bundle includes a checksum tying it to inputs

## Cleanup

```bash
cd ~
rm -rf ~/work/devops-labs/mod17-sbom
```

## Troubleshooting

### Symptom: pip install is slow or fails

Fix:

- validate network/proxy configuration
- pin versions to reduce resolver churn

### Symptom: OS manifest is empty

Explanation:

- some images may not include `dpkg-query`

Fix:

- treat OS package manifest as optional for this lab
- in production, standardize on an SBOM toolchain and base image family
