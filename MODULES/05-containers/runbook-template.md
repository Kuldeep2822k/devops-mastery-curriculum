---
title: Runbook Template (Module 05)
tags:
  - runbook
  - template
  - containers
module: "05"
---

# Runbook Template — Module 05 Containers

## Title

Container — <Symptom (exits on start / unreachable / permission denied / OOM)>

## Impact

- User impact:
- Scope:
- Severity:

## Safety and Preconditions

- Capture evidence before removal/restart.
- Do not print secrets.
- Prefer minimal reversible actions.

## Quick Triage (5 minutes)

- Container state:
  - `docker ps -a`
- Logs:
  - `docker logs --tail 200 <container>`
- Exit code:
  - `docker inspect <container> --format '{{.State.ExitCode}}'`
- Port mapping:
  - `docker port <container> || true`
- Resources:
  - `docker stats --no-stream`

## Diagnosis

### Hypotheses

- H1:
- H2:
- H3:

### Evidence Commands

- Command:
  - Expected:
  - Interpretation:

## Containment

- rollback to prior image digest
- stop a bad rollout
- reduce traffic/load (if applicable)

## Fix

Common fixes:

- correct CMD/ENTRYPOINT
- correct port mapping
- fix volume ownership/permissions
- tune resource limits with evidence

## Verification

- health endpoint:
- logs show expected startup:
- runtime state stable for N minutes:

## Prevention / Follow-Ups

- add smoke tests (run container + curl /healthz)
- adopt digest-based deploys
- document ownership model for volumes
- add resource monitoring and limits
