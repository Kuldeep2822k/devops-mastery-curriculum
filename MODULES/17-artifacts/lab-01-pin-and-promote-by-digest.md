---
title: 'Pin and Promote by Digest'
tags:
  - lab
  - artifacts
  - delivery
module: "17"
---

# Lab 01 — Pin and Promote by Digest (Local Promotion)

## Goal

Practice “build once, promote many” with concrete artifact identity:

- build a container image (candidate)
- record immutable identity (image ID + repo digest)
- “promote” the exact same image to a new tag without rebuilding
- demonstrate why tags are not identity

## Prereqs

- Docker installed and working: `docker version`
- `sha256sum`

## Setup

```bash
mkdir -p ~/work/devops-labs/mod17-digest
cd ~/work/devops-labs/mod17-digest
```

Create a tiny app:

```bash
cat > app.sh <<'EOF'
set -eu
echo "hello commit=${COMMIT_SHA:-dev}"
EOF
chmod +x app.sh
```

Create a Dockerfile (intentionally tag-based to illustrate why digest pinning matters):

```bash
cat > Dockerfile <<'EOF'
FROM alpine:3.20
WORKDIR /app
COPY app.sh /app/app.sh
ENV COMMIT_SHA=dev
CMD ["/bin/sh","/app/app.sh"]
EOF
```

## Steps

### 1) Build Candidate Image and Record Identity

```bash
docker build -t mod17-app:candidate .
docker image inspect mod17-app:candidate --format '{{.Id}}'
```

Capture a simple “release metadata” file:

```bash
image_id="$(docker image inspect mod17-app:candidate --format '{{.Id}}')"
printf "image_id=%s\n" "$image_id" > release.txt
cat release.txt
```

Expected signals:

- `release.txt` exists
- it contains an immutable image id

### 2) Demonstrate Why Tags Are Not Identity

Rebuild the same tag after changing the build input:

```bash
printf '\necho "build_ts=%s"\n' "$(date +%s)" >> app.sh
docker build -t mod17-app:candidate .
docker image inspect mod17-app:candidate --format '{{.Id}}'
```

Expected signals:

- the image ID changes after rebuild even though the tag stayed the same

### 3) Promote Without Rebuilding

Use the previously recorded image id to tag the exact same artifact:

```bash
original_id="$(awk -F= '/^image_id=/{print $2}' release.txt)"
docker tag "$original_id" mod17-app:prod
docker image inspect mod17-app:prod --format '{{.Id}}'
```

Expected signals:

- `mod17-app:prod` points at the original image id from `release.txt`

### 4) Run and Verify Behavior

```bash
docker run --rm mod17-app:prod
docker run --rm -e COMMIT_SHA=abc123 mod17-app:prod
```

Expected:

- output includes `hello commit=...`

### 5) Controlled Failure Drill: Wrong Artifact Promoted

Promote the mutable tag by mistake:

```bash
docker tag mod17-app:candidate mod17-app:prod-bad
docker image inspect mod17-app:prod-bad --format '{{.Id}}'
```

Root cause:

- you promoted by tag, not by immutable identity.

Recovery:

```bash
docker tag "$original_id" mod17-app:prod
docker image inspect mod17-app:prod --format '{{.Id}}'
```

## Verify

```bash
original_id="$(awk -F= '/^image_id=/{print $2}' release.txt)"
prod_id="$(docker image inspect mod17-app:prod --format '{{.Id}}')"
test "$original_id" = "$prod_id" && echo "ok: prod pinned to original id"
```

Expected signals:

- script prints `ok: prod pinned to original id`

## Cleanup

```bash
docker image rm -f mod17-app:candidate mod17-app:prod mod17-app:prod-bad 2>/dev/null || true
cd ~
rm -rf ~/work/devops-labs/mod17-digest
```

## Troubleshooting

### Symptom: you can’t tag by image id

Fix:

- ensure the id includes the `sha256:` prefix (it should from `docker image inspect`)

### Symptom: image build is not reproducible

Fix:

- pin base images by digest in real systems
- avoid injecting timestamps unless you intend to change the artifact
