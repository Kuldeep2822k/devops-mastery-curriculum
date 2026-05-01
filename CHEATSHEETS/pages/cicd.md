---
title: 'CI/CD'
tags:
  - cheatsheet
  - ci
  - cicd
  - delivery
---

# CI/CD Cheatsheet (Operator + Reliability)

## Core Concepts

- Build once, promote many (promote artifacts by digest, not by rebuilding).
- Separate environments via config + approvals, not branches with divergent code.

## Common Commands / Checks

- Confirm what triggered the run:
  - event type, branch filters, path filters
- Confirm what artifact is produced:
  - explicit artifact path + checksum/digest
- Confirm cache keys include lockfiles:
  - dependency lockfile hash in key

## Failure Patterns

- Trigger not firing: wrong event, workflow disabled, filters exclude.
- Secrets missing: fork PR restrictions, wrong environment, missing approvals.
- Missing artifact: wrong working dir, conditional step, build skipped.
- Flaky tests: nondeterminism, ordering, timeouts, shared state.
- Wrong version deployed: mutable tags, no provenance, rebuild-per-env.

## Safe Practices

- Never print secrets; avoid `set -x` in jobs that touch credentials.
- Pin dependencies (lockfiles) and artifacts (digests).
- Add smoke tests that validate artifacts exist and start.

