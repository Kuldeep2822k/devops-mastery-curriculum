---
title: Troubleshooting Lab (Scenarios)
tags:
  - troubleshooting-lab
  - security
  - oncall
module: "10"
---

# Troubleshooting Lab — Module 10 Security

## Instructions

- Do 12–20 scenarios.
- Time-box each: 10–20 minutes.
- Record: symptoms, constraints, hints, diagnosis commands, root cause, fix, verification, prevention.

## Scenarios

### Scenario 01 — Secret Committed to Repo

- Symptoms: secret-like value found in git history.
- Constraints: treat as compromised immediately.
- Hints: rotation first, cleanup second.
- Diagnosis:
  - identify file and commit
  - search for other exposures (logs/artifacts)
- Root cause: secret stored in file and committed.
- Fix: rotate secret; remove from code; rewrite history only if needed after rotation.
- Verification: new secret works; old secret revoked.
- Prevention: pre-commit secret scans; CI gate; training.

### Scenario 02 — Secret Printed in CI Logs

- Symptoms: CI logs show env dump includes secret.
- Constraints: do not print more secrets while investigating.
- Hints: stop pipeline and rotate.
- Diagnosis:
  - find step that printed env
- Root cause: debugging step prints env variables.
- Fix: remove env dump; rotate secret; restrict secret scope.
- Verification: CI runs without secrets printed.
- Prevention: forbid `printenv`/`env` dumps in pipelines; policy gate.

### Scenario 03 — :latest Base Image Used

- Symptoms: Dockerfile uses `:latest`.
- Constraints: must pin versions.
- Hints: reproducibility and supply chain.
- Diagnosis:
  - grep Dockerfiles
- Root cause: unpinned base image.
- Fix: pin to specific version/digest.
- Verification: policy gate passes.
- Prevention: CI gate for base image pinning.

### Scenario 04 — Dependency Drift

- Symptoms: builds pull different dependency versions over time.
- Constraints: must be deterministic.
- Hints: lockfiles.
- Diagnosis:
  - check lockfile presence
- Root cause: missing lockfile or unpinned dependency.
- Fix: add lockfile; pin versions.
- Verification: repeated builds use same versions.
- Prevention: lockfile enforcement in CI.

### Scenario 05 — Over-Permissive CI Token

- Symptoms: CI token has admin privileges.
- Constraints: least privilege required.
- Hints: scope and rotate.
- Diagnosis:
  - review what token can do (conceptually)
- Root cause: broad permissions used for convenience.
- Fix: create minimal-scope token; rotate.
- Verification: pipeline works with reduced scope.
- Prevention: standard token scopes and audits.

### Scenario 06 — Accidental Public Exposure

- Symptoms: service accessible publicly when it shouldn’t be.
- Constraints: contain quickly.
- Hints: block access first.
- Diagnosis:
  - identify which port/endpoint is exposed
- Root cause: misconfigured firewall/ingress.
- Fix: restrict access; add authentication.
- Verification: access blocked from unauthorized clients.
- Prevention: IaC policy gates and review.

### Scenario 07 — Suspicious Artifact

- Symptoms: deployed artifact hash doesn’t match expected.
- Constraints: stop rollout.
- Hints: provenance.
- Diagnosis:
  - compare expected commit SHA/digest to deployed
- Root cause: artifact rebuilt or wrong artifact promoted.
- Fix: redeploy correct artifact; investigate pipeline.
- Verification: deployed identity matches expected.
- Prevention: promote by digest; attach metadata.

### Scenario 08 — Secret in Terraform State

- Symptoms: state contains secret values.
- Constraints: treat state as sensitive.
- Hints: backend controls.
- Diagnosis:
  - identify where state stored and who has access
- Root cause: provider stored sensitive values in state.
- Fix: restrict state access; rotate secrets if exposed.
- Verification: state protected; access audited.
- Prevention: remote backend + access controls; avoid outputs of secrets.

### Scenario 09 — Secret in Shell History

- Symptoms: operator pasted token into terminal and it’s now in history.
- Constraints: assume compromise.
- Hints: rotate.
- Diagnosis:
  - identify exposure scope
- Root cause: unsafe operational behavior.
- Fix: rotate secret; clear history if appropriate (still rotate).
- Verification: old token revoked.
- Prevention: training; safer auth mechanisms; do not echo secrets.

### Scenario 10 — Policy Gate Bypassed Frequently

- Symptoms: teams disable security checks.
- Constraints: improve signal-to-noise.
- Hints: make gates actionable.
- Diagnosis:
  - identify false positives and slow checks
- Root cause: poor gate design.
- Fix: tune rules; stage checks; improve messages.
- Verification: gate enforced with low bypass rate.
- Prevention: owner for gate and SLO for gate reliability.

### Scenario 11 — CI Runs on Untrusted Code With Secrets

- Symptoms: PR from fork can access secrets.
- Constraints: must block immediately.
- Hints: environment separation.
- Diagnosis:
  - review CI secret scoping
- Root cause: secrets available to PR jobs.
- Fix: remove secrets from PR context; require approvals.
- Verification: PR jobs run without secrets.
- Prevention: CI policy templates.

### Scenario 12 — “Security Fix” Broke Production

- Symptoms: security change causes outage (e.g., firewall too strict).
- Constraints: contain and restore service.
- Hints: rollback and staged rollout.
- Diagnosis:
  - identify what security control changed and why
- Root cause: change not staged or verified.
- Fix: rollback; reapply safely with canary.
- Verification: service restored; security control redesigned.
- Prevention: ADR and runbook for security changes.

## Time-Boxed On-Call Drill (30–60 minutes)

Scenario: secret leaked in CI logs.

- deliverables:
  - containment (stop pipeline, rotate secret)
  - evidence (which step leaked)
  - prevention (policy gate + restricted secret scope)
