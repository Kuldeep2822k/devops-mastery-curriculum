---
title: Troubleshooting Guide
tags:
  - troubleshooting
  - ci
module: "04"
---

# Troubleshooting — Module 04 CI

## First Principle

CI failures are usually one of:

- input drift (lockfiles, dependencies, tool versions)
- missing artifacts (dist/ not generated or not persisted)
- environment mismatch (local vs runner differences)
- secrets/environment separation issues
- nondeterminism (flaky tests, timing)

## Fast Diagnosis Checklist

- Identify first failing step, not the last summary.
- Determine if failure is deterministic by re-running locally in a clean runner container.
- Confirm artifact expectations:
  - which step produces `dist/`
  - which step consumes it
- Confirm cache key correctness:
  - includes lockfile hash and tool version
- Confirm secrets are available only where intended and not required in PR validation.

## Core Commands (Local Repro)

```bash
make ci
docker run --rm -t -v "$PWD":/work -w /work python:3.12-slim bash -lc "apt-get update >/dev/null && apt-get install -y --no-install-recommends make git >/dev/null && make ci"
```

## Artifact Debugging

If `dist/` is missing:

- confirm build step actually ran
- confirm paths match
- confirm cleanup didn’t remove outputs

Local inspection:

```bash
ls -la dist || true
find . -maxdepth 3 -type f | head -n 50
```

## Cache Debugging

Cache failure patterns:

- stale cache: missing modules or unexpected versions
- cache miss: pipeline slow but correct

Fix strategy:

- tighten key (include lockfile and tool versions)
- add restore keys carefully (avoid cross-branch contamination)

## Secrets Debugging (Safe)

Do not print secrets.

Instead verify presence by:

- checking step behavior (e.g., “auth step succeeded”)
- checking whether step is allowed to access the secret in that environment

## Flaky Tests Debugging

- run the test loop locally:

```bash
for i in $(seq 1 20); do ./test.sh || exit 1; done
echo stable
```

- eliminate time and randomness dependencies
- isolate shared state
