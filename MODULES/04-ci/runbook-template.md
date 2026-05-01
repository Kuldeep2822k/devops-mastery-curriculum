---
title: Runbook Template (Module 04)
tags:
  - runbook
  - template
  - ci
module: "04"
---

# Runbook Template — Module 04 CI

## Title

CI Pipeline — <Symptom (e.g., failing on main / missing artifacts / slow runs)>

## Impact

- Who is blocked (merges, releases, hotfixes)?
- Scope (all repos, one repo, one job)?
- Severity:

## Safety

- No secrets printed in logs.
- No disabling protections without a documented break-glass process.
- Preserve evidence: failing run links, logs, artifact listings.

## Quick Triage (10 minutes)

- Identify first failing step.
- Classify failure type:
  - input drift
  - artifact missing
  - cache issue
  - secret/env permissions
  - flaky/nondeterministic
  - timeout/performance

## Diagnosis Commands (Local Repro)

```bash
make ci
docker run --rm -t -v "$PWD":/work -w /work python:3.12-slim bash -lc "apt-get update >/dev/null && apt-get install -y --no-install-recommends make git >/dev/null && make ci"
```

## Artifact Contract Checks

- expected outputs:
- where produced:
- where consumed:

Verify locally:

```bash
ls -la dist || true
test -s dist/app.txt
test -s dist/manifest.txt
```

## Cache Checks

- key components:
- lockfile hash included:
- tool version included:

## Secrets and Environment Checks

- which jobs require secrets:
- confirm secrets only present in trusted contexts:

## Fix and Verification

- minimal fix:
- verification signals:
  - pipeline green
  - artifact uploaded
  - duration acceptable

## Prevention / Follow-Ups

- guardrails:
- flake tracking:
- cache key policy:
- artifact contract enforcement:
