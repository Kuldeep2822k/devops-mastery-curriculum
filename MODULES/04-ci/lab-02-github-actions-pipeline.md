---
title: "Lab 02: GitHub Actions Pipeline (Caching + Artifacts + Fail-Fast)"
tags:
  - lab
  - ci
  - github-actions
module: "04"
---

# Lab 02 — GitHub Actions Pipeline (Caching + Artifacts + Fail-Fast)

## Goal

Create a GitHub Actions pipeline that:

- runs lint/test/build/smoke using the same Make targets as local pipeline
- uses caching (dependency/tool caching) safely
- uploads artifacts (dist/) for inspection
- runs fail-fast behavior for fast feedback
- includes a smoke test gate

## Prereqs

- A GitHub repository
- Your portable pipeline repo from Lab 01 (or recreate it)
- Branch protection optional but recommended

## Setup

In your pipeline repo (e.g., `~/work/devops-labs/mod04-ci-portable`):

Create a workflow file:

```bash
mkdir -p .github/workflows
cat > .github/workflows/ci.yml <<'EOF'
name: ci

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build-test:
    runs-on: ubuntu-latest
    strategy:
      fail-fast: true
      matrix:
        python-version: ["3.12"]

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}

      - name: Cache pip
        uses: actions/cache@v4
        with:
          path: ~/.cache/pip
          key: pip-${{ runner.os }}-${{ matrix.python-version }}-${{ hashFiles('**/requirements.txt') }}
          restore-keys: |
            pip-${{ runner.os }}-${{ matrix.python-version }}-

      - name: Install build tools
        run: |
          sudo apt-get update
          sudo apt-get install -y --no-install-recommends make

      - name: Lint
        run: make lint

      - name: Test
        run: make test

      - name: Build + Smoke
        run: make smoke

      - name: Upload dist artifacts
        uses: actions/upload-artifact@v4
        with:
          name: dist
          path: dist/
EOF
```

If you don’t have a `requirements.txt`, create an empty one to stabilize cache key shape:

```bash
test -f requirements.txt || : > requirements.txt
```

Commit and push:

```bash
git add .github/workflows/ci.yml requirements.txt
git commit -m "ci: add GitHub Actions pipeline"
```

Push to GitHub and open a PR (or push to main).

## Steps

### 1) Confirm Pipeline Trigger and Status

Expected signals:

- workflow triggers on PR and/or main push
- lint/test/smoke steps run
- artifact named `dist` uploaded

### 2) Validate Artifact Contract

From the Actions UI:

- download the `dist` artifact
- confirm it includes:
  - `app.txt`
  - `manifest.txt`

### 3) Exercise Fail-Fast and Smoke Gate

Introduce a deliberate failure:

- break `test.sh` so it fails
- push commit to PR

Expected:

- pipeline fails quickly at test step
- later steps do not run

Fix and push again:

- pipeline returns to green

## Verify

Verification signals to capture in your evidence:

- the workflow run link (no secrets)
- artifact contents (file listing, not secret values)
- a failing run and the step where it failed

## Cleanup

- remove the test repo if not needed
- do not leave tokens or credentials lying around

## Troubleshooting

### Symptom: workflow doesn’t trigger

Diagnosis:

- check file path is `.github/workflows/ci.yml`
- check `on:` configuration
- check branch name matches

Fix:

- correct triggers and push again

### Symptom: cache is ineffective or causes breakage

Diagnosis:

- cache key does not include lockfile/tool version
- cache reused across branches incorrectly

Fix:

- tighten cache key (include lockfile hash)
- restrict cache scope (OS, language version)

### Symptom: missing `dist/` artifacts

Diagnosis:

- build step did not produce outputs
- artifact path incorrect

Fix:

- ensure `make smoke` produces dist/
- ensure upload step points to `dist/`

## Why This Matters in Production

- CI must be fast, reliable, and auditable to support incident response.
- Artifacts and caching reduce build times but require careful correctness controls.

## What to Write in a Runbook

- how to reproduce CI locally (`make ci`)
- how to inspect artifacts from CI runs
- how to debug missing dist/ and lockfile drift

## Definition of Done

- pipeline triggers on PR and push
- caches are keyed safely
- artifacts uploaded and inspectable
- fail-fast behavior observed on intentional failure
