---
title: Runbook Template (Module 10)
tags:
  - runbook
  - template
  - security
module: "10"
---

# Runbook Template — Module 10 Security

## Title

Security — <Incident (secret leaked / suspicious CI / dependency compromise)>

## Impact

- What is compromised or at risk?
- Scope (which systems/environments)?
- Severity:

## Safety and Preconditions

- Do not print secrets.
- Preserve evidence where safe.
- Prefer containment first when blast radius is high.

## Secret Leak Response

1) Assume compromise  
2) Revoke/rotate secret  
3) Remove secret from code/config  
4) Identify exposure locations (logs, artifacts, forks)  
5) Add guardrails (pre-commit/CI gates)  

## CI Suspicion Response

- stop releases/deploys
- rotate CI tokens if exposure possible
- review recent PRs and workflow changes
- verify artifact identity (digest/SHA)

## Dependency/Supply Chain Response

- identify affected versions (manifest/lockfile)
- rollback to known-good version if needed
- pin fixed versions and redeploy

## Verification

- old secrets revoked, new secrets work
- pipeline no longer leaks secrets
- policies enforced and auditable

## Prevention / Follow-Ups

- add/strengthen policy gates
- least privilege audits
- on-call drills for secret leaks
