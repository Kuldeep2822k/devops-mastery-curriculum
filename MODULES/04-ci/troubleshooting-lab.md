---
title: Troubleshooting Lab (Scenarios)
tags:
  - troubleshooting-lab
  - ci
  - oncall
module: "04"
---

# Troubleshooting Lab — Module 04 CI

## Instructions

- Do 12–20 scenarios.
- Time-box each: 10–20 minutes.
- For each scenario: symptoms, constraints, hints, diagnosis commands, root cause, fix, verification, prevention.

## Scenarios

### Scenario 01 — Workflow Trigger Not Firing

- Symptoms: push/PR created but no CI run.
- Constraints: no admin permissions assumed.
- Hints: workflow path and trigger config.
- Diagnosis commands:
  - confirm file path `.github/workflows/ci.yml`
  - check `on:` includes correct branches/events
- Root cause: workflow not in default branch or triggers misconfigured.
- Fix: correct triggers and push.
- Verification: new push triggers workflow.
- Prevention: add a minimal “CI sanity” workflow and require it in branch protections.

### Scenario 02 — Lockfile Drift Breaks Build

- Symptoms: CI fails installing deps; local works.
- Constraints: do not “just update” dependencies.
- Hints: lockfiles and deterministic installs.
- Diagnosis commands:
  - confirm lockfile committed and unchanged
  - compare dependency versions between local and CI (without printing secrets)
- Root cause: CI uses different dependency set due to missing lockfile or unpinned versions.
- Fix: commit lockfile; pin versions; ensure install uses lockfile.
- Verification: CI install step becomes deterministic.
- Prevention: CI check that fails if lockfile changes are uncommitted.

### Scenario 03 — Missing dist/ Directory

- Symptoms: upload-artifact step fails because dist/ missing.
- Constraints: must not add “mkdir dist” without fixing the real issue.
- Hints: artifact contract broken.
- Diagnosis commands:
  - run `make smoke` locally
  - inspect Make target dependencies
- Root cause: build/package not executed or output path mismatch.
- Fix: correct target dependencies and output paths.
- Verification: dist/ exists and is uploaded.
- Prevention: smoke test gate that asserts dist/ shape.

### Scenario 04 — Secrets Missing in Environment

- Symptoms: deploy/auth step fails; secret not found.
- Constraints: do not print secrets; do not broaden secret scope.
- Hints: environment separation and protected contexts.
- Diagnosis commands:
  - confirm workflow environment context (PR vs main vs release)
  - confirm secret configured in that environment scope
- Root cause: secret not configured for that environment or not accessible in PR context.
- Fix: store secret in correct environment; restrict deploy steps to trusted contexts.
- Verification: step succeeds in allowed context; still blocked in PR contexts.
- Prevention: policy: PR jobs never require prod secrets.

### Scenario 05 — Slow Pipelines (Cache Ineffective)

- Symptoms: pipeline takes too long; repeated runs not faster.
- Constraints: do not sacrifice correctness.
- Hints: cache key design.
- Diagnosis commands:
  - review cache keys: include lockfile hash, OS, tool version
  - observe whether cache hits occur
- Root cause: cache keys too specific (always miss) or caching wrong path.
- Fix: correct cache path and restore keys; include stable but correct key components.
- Verification: subsequent run shows faster install/build.
- Prevention: track cache hit rate metric.

### Scenario 06 — Flaky Tests

- Symptoms: same commit alternates pass/fail.
- Constraints: cannot “rerun until pass” as the fix.
- Hints: nondeterminism.
- Diagnosis commands:
  - run test loop locally 20 times
  - isolate time/randomness/shared state
- Root cause: flaky test due to race/time dependency.
- Fix: make test deterministic; remove sleeps; isolate state.
- Verification: 20/20 passes on same commit.
- Prevention: measure flake rate; quarantine only with an SLA to fix.

### Scenario 07 — Wrong Tag Deployed

- Symptoms: release pipeline deployed an unexpected version.
- Constraints: preserve audit trail.
- Hints: tag/commit mapping and artifact promotion.
- Diagnosis commands:
  - verify tag points to correct commit: `git show <tag>`
  - verify artifact built from commit SHA recorded in manifest
- Root cause: tag created at wrong commit or artifact rebuilt from different SHA.
- Fix: stop rebuilding; build once; promote by digest; create corrected tag.
- Verification: deployed artifact matches expected commit SHA.
- Prevention: enforce provenance metadata and immutable artifact promotion.

### Scenario 08 — Branch Protections Blocking Hotfix

- Symptoms: urgent fix blocked by required checks.
- Constraints: do not disable protections permanently.
- Hints: emergency path should be documented and auditable.
- Diagnosis commands:
  - identify which checks are required and why they are failing/slow
- Root cause: missing emergency process or CI outage.
- Fix: use documented emergency path (temporary override with logging) or fix CI quickly.
- Verification: hotfix merged and released with traceability.
- Prevention: define break-glass procedure with guardrails.

### Scenario 09 — Environment-Specific Secrets Used in PR

- Symptoms: PR job fails because it expects prod secret.
- Constraints: no prod secrets for PRs.
- Hints: separate jobs and conditionals.
- Diagnosis commands:
  - inspect workflow conditionals and job dependencies
- Root cause: deploy/auth step running in PR context.
- Fix: gate deploy steps by branch/tag; move secrets to protected env.
- Verification: PR pipeline passes without secrets; release pipeline uses secrets safely.
- Prevention: design PR pipelines to be hermetic.

### Scenario 10 — Caching Causes Stale Dependencies

- Symptoms: CI uses old dependency version; local uses new.
- Constraints: must not disable caching entirely.
- Hints: cache key missing lockfile hash.
- Diagnosis commands:
  - inspect cache key components
  - confirm lockfile change triggers cache bust
- Root cause: cache key too broad; stale cache reused.
- Fix: include lockfile hash in cache key; reduce restore-key breadth.
- Verification: CI installs correct deps after lockfile update.
- Prevention: include tool and lockfile fingerprinting everywhere.

### Scenario 11 — Artifact Not Available in Downstream Job

- Symptoms: job B cannot find output from job A.
- Constraints: do not rebuild; must use artifact.
- Hints: use artifact upload/download and explicit dependencies.
- Diagnosis commands:
  - confirm upload step ran and path correct
  - confirm download step exists and runs before use
- Root cause: artifact not uploaded, wrong name, wrong path, or missing `needs:`.
- Fix: fix upload/download and job dependency graph.
- Verification: downstream job can access artifact.
- Prevention: document artifact contract and enforce via smoke checks.

### Scenario 12 — Pipeline Fails Only on main

- Symptoms: PR passes; main fails.
- Constraints: cannot weaken main checks.
- Hints: environment differences and conditional steps.
- Diagnosis commands:
  - compare job steps and env for PR vs main
  - identify steps only on main (publish/release)
- Root cause: main-only steps require additional permissions or secrets.
- Fix: scope permissions; add safe validation for main-only steps; ensure secrets configured.
- Verification: main pipeline green; PR remains hermetic.
- Prevention: keep PR/main differences explicit and minimal.

### Scenario 13 — “Works on Runner” but Fails Locally

- Symptoms: CI green; local `make ci` fails.
- Constraints: pipeline must be portable.
- Hints: local toolchain mismatch.
- Diagnosis commands:
  - run clean runner container locally
  - compare tool versions
- Root cause: local environment missing tool or version mismatch.
- Fix: document and pin tool versions; provide devcontainer or bootstrap scripts.
- Verification: local and CI both pass.
- Prevention: makefile contract and local runner simulation as standard.

### Scenario 14 — Timeout Failures

- Symptoms: jobs fail due to timeouts.
- Constraints: do not just increase timeout blindly.
- Hints: identify slow step.
- Diagnosis commands:
  - measure duration per step
  - identify network waits, caching misses, heavy tests
- Root cause: slow dependencies, no caching, or heavy tests.
- Fix: optimize caching, split tests, reduce unnecessary work; then tune timeout.
- Verification: job completes within timeout with stable duration.
- Prevention: CI performance metrics and budgets.

## Time-Boxed On-Call Drill (30–60 minutes)

Simulate a CI outage:

- Scenario: main pipeline failing; hotfix needed; constraints: no force-push, no disabling protections permanently.
- Deliverables:
  - incident timeline
  - minimal fix to restore CI
  - documented break-glass path
  - prevention tasks (flake tracking, cache key policy, artifact contracts)
