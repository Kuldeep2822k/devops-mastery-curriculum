---
title: 'CI Pipeline Failures'
tags:
  - catalog
  - ci
  - cicd
  - supply-chain
---

# CI Pipeline Failures — Troubleshooting Scenarios

Use this when builds/tests/deploy workflows fail, are flaky, or produce the wrong artifacts.

## Scenarios

### Scenario 01 — Workflow not triggering

- Symptoms: Push happens; no run starts.
- Diagnosis commands:
  - Check workflow file path and branch filters.
  - Verify required status checks/branch protections.
- Root cause: Wrong event (`pull_request` vs `push`), path filters exclude, workflow disabled.
- Fix: Correct triggers; ensure file in default branch; re-run.
- Verify: New commit triggers run.
- Prevention: Minimal trigger test workflow; documentation.

### Scenario 02 — Secrets missing in pipeline

- Symptoms: Auth failures; env vars empty; “secret not set”.
- Constraints: Never print secrets.
- Diagnosis commands:
  - Inspect job environment separation (repo/org/environment).
  - Check permissions for PRs from forks.
- Root cause: Secrets not available in fork PRs, wrong environment, missing approvals.
- Fix: Use OIDC/workload identity; move to environment secrets; require approval gates.
- Verify: Auth succeeds without echoing secrets.
- Prevention: Explicit secret matrix and runbook.

### Scenario 03 — Cache causes stale dependencies / lockfile drift

- Symptoms: Works locally; CI uses old deps; mysterious failures.
- Diagnosis commands:
  - Inspect cache key and restore keys.
  - Confirm lockfiles are committed and unchanged.
- Root cause: Cache key too broad; lockfile not included; restore keys too permissive.
- Fix: Include lockfile hash in key; narrow restore keys; clear cache.
- Verify: Clean run reproduces deterministically.
- Prevention: Deterministic builds; periodic cache busting strategy.

### Scenario 04 — Artifacts missing (`dist/` not produced)

- Symptoms: Deploy step fails because artifact not found.
- Diagnosis commands:
  - Confirm build step produces output.
  - Check artifact upload paths and working directories.
- Root cause: Wrong path, build skipped, conditional steps, workspace mismatch.
- Fix: Normalize working directory; explicitly create artifacts; fail fast if missing.
- Verify: Artifact exists and is downloadable; deploy step consumes it.
- Prevention: Smoke-test step `test -d dist` (or equivalent).

### Scenario 05 — Flaky tests / nondeterminism

- Symptoms: Random failures; reruns pass.
- Diagnosis commands:
  - Identify failing test set and patterns.
  - Check timeouts, ordering, parallelism.
- Root cause: Race conditions, shared state, external dependencies, time sensitivity.
- Fix: Isolate tests; add retries only as quarantine; increase determinism; mock external deps.
- Verify: 20 consecutive green runs.
- Prevention: Test reliability rubric; “quarantine” policy with deadlines.

### Scenario 06 — Wrong image tag deployed / wrong revision promoted

- Symptoms: Production running older commit; rollback confusion.
- Diagnosis commands:
  - Compare image digests across environments.
  - Check tagging strategy and promotion steps.
- Root cause: Tag reuse (`latest`), rebuild-per-env, missing provenance.
- Fix: Pin digests; build once; promote by digest; include commit SHA in metadata.
- Verify: Environment references correct digest.
- Prevention: Release engineering module gates; policy checks.

### Scenario 07 — Environment-specific config leaks into build

- Symptoms: Works in staging; fails in prod; baked-in endpoints.
- Diagnosis commands:
  - Inspect build-time env variables and config files.
  - Check if configuration is runtime-injected.
- Root cause: Building with prod/stage secrets, config embedded in artifact.
- Fix: Move to runtime config injection; use templating; separate build vs deploy.
- Verify: Same artifact runs in multiple envs with different configs.
- Prevention: ADR: “build once, promote many”.
