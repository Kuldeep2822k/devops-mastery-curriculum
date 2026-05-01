---
title: Troubleshooting Lab (Scenarios)
tags:
  - troubleshooting-lab
  - delivery
  - oncall
module: "07"
---

# Troubleshooting Lab — Module 07 Delivery

## Instructions

- Do 12–20 scenarios.
- Time-box each: 10–20 minutes.
- For each: symptoms, constraints, hints, diagnosis commands, root cause, fix, verification, prevention.

## Scenarios

### Scenario 01 — Wrong Version Deployed

- Symptoms: responses show unexpected version string.
- Constraints: no rebuilds; use existing artifacts.
- Hints: inspect image tag/digest and rollout history.
- Diagnosis commands:
  - `kubectl -n <ns> rollout history deploy/<name>`
  - `kubectl -n <ns> get deploy/<name> -o jsonpath='{.spec.template.spec.containers[*].image}{"\n"}'`
- Root cause: deploy referenced wrong tag/commit.
- Fix: deploy the correct artifact identity; record it.
- Verification: responses show correct version; rollout stable.
- Prevention: promote by digest; CI attaches commit SHA to artifact.

### Scenario 02 — Rollout Stuck (Readiness Failures)

- Symptoms: rollout never completes; pods running but not Ready.
- Constraints: do not delete pods without evidence.
- Hints: readiness probe mismatch.
- Diagnosis commands:
  - `kubectl -n <ns> rollout status deploy/<name>`
  - `kubectl -n <ns> describe pod <pod>`
  - `kubectl -n <ns> get ep <svc>`
- Root cause: readiness probe wrong path/port or dependency failing.
- Fix: correct probe or config; rollout; consider rollback if impact high.
- Verification: pods Ready; endpoints populated; user checks pass.
- Prevention: test probes in staging; add smoke checks.

### Scenario 03 — Canary Unhealthy but Stable OK

- Symptoms: canary pods not Ready; stable still serving.
- Constraints: protect user impact; do not promote.
- Hints: rollback canary only.
- Diagnosis commands:
  - `kubectl -n <ns> get pods --show-labels`
  - `kubectl -n <ns> get ep <svc>`
  - events and describe for canary pods
- Root cause: canary misconfig or bad build.
- Fix: rollback canary; keep stable serving.
- Verification: no canary endpoints; stable service healthy.
- Prevention: canary gates with explicit stop conditions.

### Scenario 04 — Service 503 After Deploy (Endpoints Empty)

- Symptoms: 503; Service endpoints empty.
- Constraints: do not restart ingress/controller as first move.
- Hints: label mismatch or readiness.
- Diagnosis commands:
  - `kubectl -n <ns> get svc,ep`
  - `kubectl -n <ns> get pods --show-labels`
  - `kubectl -n <ns> describe svc <svc>`
- Root cause: selector/label mismatch or pods not Ready.
- Fix: align labels/selectors; fix readiness.
- Verification: endpoints populated; 200 responses.
- Prevention: manifest linting and policy checks.

### Scenario 05 — Rollback Didn’t Fix Incident

- Symptoms: rollback executed but errors persist.
- Constraints: must prove whether rollback actually took effect.
- Hints: verify deployed artifact identity and config.
- Diagnosis commands:
  - check image after rollback: jsonpath image
  - verify config versions (ConfigMap/flags)
  - check dependency health
- Root cause: rollback unsafe (schema/config) or non-deploy root cause.
- Fix: contain with feature flag/traffic reduction; fix root cause; plan safe migration.
- Verification: user signals recover.
- Prevention: expand/contract migrations; rollback rehearsals.

### Scenario 06 — Migration Broke Old Version

- Symptoms: after rollback, old version crashes on startup.
- Constraints: cannot re-run destructive migration backwards.
- Hints: backward compatibility failure.
- Diagnosis commands:
  - app logs show schema/config mismatch
- Root cause: migration not backward compatible.
- Fix: forward-fix with compatibility shim; restore old schema fields if possible.
- Verification: old and new versions can run concurrently.
- Prevention: expand/contract; feature flags; dual-write where needed.

### Scenario 07 — Deployment Rolled Out but Latency Spikes

- Symptoms: 200 OK but p95 latency increased.
- Constraints: avoid multiple deploys; pick one containment action.
- Hints: rollback trigger based on SLO.
- Diagnosis commands:
  - measure latency with curl loop
  - inspect resource throttling and CPU limits
- Root cause: regression or CPU throttling.
- Fix: rollback; or adjust resources if evidence supports.
- Verification: latency returns to baseline range.
- Prevention: performance smoke tests; avoid CPU limits for latency critical path (context-dependent).

### Scenario 08 — Wrong Config Applied to Production

- Symptoms: behavior indicates staging config in prod.
- Constraints: secrets must not be printed; avoid broad access.
- Hints: environment separation failure.
- Diagnosis commands:
  - inspect ConfigMap/Env references
  - confirm namespace and context
- Root cause: wrong config map referenced or wrong environment variables.
- Fix: deploy correct config; rollback if needed.
- Verification: behavior matches production expectations.
- Prevention: environment-specific promotion rules; approvals.

### Scenario 09 — Approval Gate Bypassed

- Symptoms: production deploy happened without approval.
- Constraints: preserve audit trail.
- Hints: CI/workflow misconfiguration.
- Diagnosis commands:
  - inspect pipeline configuration and environment restrictions
- Root cause: missing protected environment gate or conditions.
- Fix: restore approvals; restrict deploy job to tags/main.
- Verification: unauthorized deploy attempts blocked.
- Prevention: policy as code; periodic audit of pipeline settings.

### Scenario 10 — Artifact Rebuilt in CD (Provenance Break)

- Symptoms: deployed artifact differs from what was tested.
- Constraints: must stop rebuild; build once.
- Hints: separate build and deploy stages.
- Diagnosis commands:
  - compare manifest/commit SHA embedded in artifact
- Root cause: CD rebuilds from different inputs.
- Fix: deploy built artifact; promote by digest.
- Verification: deployed artifact identity matches tested identity.
- Prevention: pipeline contract: build produces immutable artifact, deploy consumes it.

### Scenario 11 — Canary Never Gets Traffic (Coarse Split)

- Symptoms: canary exists but responses never show canary.
- Constraints: only use basic primitives.
- Hints: Service selector and replica ratios.
- Diagnosis commands:
  - check Service selector
  - check endpoints and pod labels
- Root cause: Service not selecting canary pods.
- Fix: correct selector or labels so both tracks are selected.
- Verification: responses include canary (ratio as expected).
- Prevention: standardized labels for tracks; preflight checks.

### Scenario 12 — Rollout Undo Picks Wrong Revision

- Symptoms: rollback goes to unexpected state.
- Constraints: must choose explicit revision.
- Hints: rollout history and revisions.
- Diagnosis commands:
  - `kubectl -n <ns> rollout history deploy/<name>`
- Root cause: multiple revisions; last revision not the known-good one.
- Fix: undo to a specific revision if needed.
- Verification: image identity and behavior match known good.
- Prevention: record known-good revision and artifact identity.

### Scenario 13 — Config Drift Between Clusters/Namespaces

- Symptoms: staging works; prod fails.
- Constraints: no manual edits in prod as fix.
- Hints: drift in config and policies.
- Diagnosis commands:
  - diff manifests/config between envs (from git)
  - inspect live resources vs declared
- Root cause: drift or missing config in prod.
- Fix: reconcile declared state; avoid ad hoc prod changes.
- Verification: prod matches declared state and works.
- Prevention: GitOps reconciliation (later), drift detection.

### Scenario 14 — Stuck Because of PDB / MaxUnavailable Constraints

- Symptoms: rollout hangs; old pods can’t be terminated.
- Constraints: do not increase disruption beyond safe limits.
- Hints: availability constraints blocking rollout.
- Diagnosis commands:
  - describe deployment and check conditions
  - inspect PDBs if present
- Root cause: disruption budgets or surge/unavailable settings too strict.
- Fix: tune rollout strategy parameters safely.
- Verification: rollout completes while maintaining availability.
- Prevention: test rollout configs; document availability constraints.

## Time-Boxed On-Call Drill (30–60 minutes)

Scenario:

- deploy causes Service 503 and pods restarting
- constraints: no deleting pods without evidence; rollback allowed

Deliverables:

- incident timeline with commands and reasons
- containment decision (rollback vs forward-fix)
- verification window
- prevention tasks (probe policy, label linting, rollout gates)
