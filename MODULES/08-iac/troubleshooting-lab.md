---
title: Troubleshooting Lab (Scenarios)
tags:
  - troubleshooting-lab
  - terraform
  - oncall
module: "08"
---

# Troubleshooting Lab — Module 08 IaC (Terraform)

## Instructions

- Do 12–20 scenarios.
- Time-box each: 10–20 minutes.
- Produce: symptoms, constraints, hints, diagnosis commands, root cause, fix, verification, prevention.

## Scenarios

### Scenario 01 — Wrong Directory / Wrong Environment

- Symptoms: plan shows unexpected changes.
- Constraints: do not apply until you prove environment.
- Hints: you’re in the wrong env directory or workspace.
- Diagnosis commands:
  - `pwd`
  - `terraform workspace show || true`
  - inspect backend config
- Root cause: wrong environment targeted.
- Fix: switch to correct directory/state.
- Verification: plan now shows expected diff.
- Prevention: separate env directories and use clear prompts.

### Scenario 02 — Drift Detected

- Symptoms: plan shows changes you didn’t make in code.
- Constraints: do not ignore drift.
- Hints: someone changed resource manually.
- Diagnosis commands:
  - `terraform plan`
  - `terraform show`
- Root cause: manual change/drift.
- Fix: reconcile via apply or revert manual change; update process.
- Verification: plan becomes clean after reconciliation.
- Prevention: drift policy and audits.

### Scenario 03 — Provider Download Fails

- Symptoms: init fails downloading provider.
- Constraints: no internet changes allowed.
- Hints: proxy/DNS/TLS.
- Diagnosis commands:
  - check proxy env vars
  - retry init after clearing `.terraform/`
- Root cause: network/proxy issue.
- Fix: configure proxy/CA correctly; retry.
- Verification: init succeeds.
- Prevention: stable tooling network path; mirrored registries if needed.

### Scenario 04 — Unexpected Replacement

- Symptoms: plan wants to replace a resource.
- Constraints: replacement is risky; must understand why.
- Hints: ForceNew attribute changed.
- Diagnosis commands:
  - `terraform plan` and inspect diff
- Root cause: changed attribute forces replacement.
- Fix: split change; use create_before_destroy if safe; or accept replacement with downtime plan.
- Verification: plan reflects chosen approach.
- Prevention: ADR documenting risky attributes and safe patterns.

### Scenario 05 — State Refactor Needed After Rename

- Symptoms: Terraform wants to destroy and recreate after resource rename.
- Constraints: must avoid replacement.
- Hints: state mv.
- Diagnosis commands:
  - `terraform state list`
- Root cause: state still points to old address.
- Fix: `terraform state mv old new`.
- Verification: plan shows no replacement.
- Prevention: include state mv steps in refactor PRs.

### Scenario 06 — apply failed, state now partial

- Symptoms: apply errors and stops mid-run.
- Constraints: do not manually edit resources.
- Hints: plan again to see residual diff.
- Diagnosis commands:
  - `terraform plan`
  - `terraform show`
- Root cause: transient provider/API failure or permission.
- Fix: address cause; re-run plan/apply.
- Verification: plan clean and resources consistent.
- Prevention: retries/timeouts policy; least privilege credentials.

### Scenario 07 — State File Accidentally Deleted (Local)

- Symptoms: Terraform thinks nothing exists; wants to create again.
- Constraints: do not double-create in real systems.
- Hints: state is the record.
- Diagnosis commands:
  - check backup file `terraform.tfstate.backup`
- Root cause: state deleted.
- Fix: restore from backup; in remote backend, restore version.
- Verification: plan reflects real world again.
- Prevention: remote backend with versioning; backups.

### Scenario 08 — Sensitive Output Leaked

- Symptoms: outputs show secret-like values.
- Constraints: must stop printing secrets.
- Hints: outputs should not expose secrets.
- Diagnosis commands:
  - inspect outputs and state
- Root cause: secret exported as output.
- Fix: remove output or mark sensitive; rotate secret if exposed.
- Verification: outputs no longer reveal secret values.
- Prevention: policy checks and review checklist.

### Scenario 09 — Workspace Confusion

- Symptoms: apply targets wrong workspace.
- Constraints: must prove workspace before any apply.
- Hints: workspace is hidden state selector.
- Diagnosis commands:
  - `terraform workspace show`
- Root cause: workspace mismatch.
- Fix: select correct workspace or avoid workspaces.
- Verification: plan matches expected.
- Prevention: env directory separation; minimal workspace use.

### Scenario 10 — Destroy Accident Risk

- Symptoms: plan includes destroy unexpectedly.
- Constraints: destroy not allowed.
- Hints: prevent_destroy.
- Diagnosis commands:
  - inspect plan for destroys
- Root cause: config removed a resource unintentionally.
- Fix: restore config; add prevent_destroy to critical resources.
- Verification: plan no longer destroys.
- Prevention: review gates; policy as code.

### Scenario 11 — Lock Contention (Remote Backend)

- Symptoms: backend reports state locked.
- Constraints: must not force unlock unless safe.
- Hints: another apply running or crashed.
- Diagnosis:
  - check recent runs and who holds lock (backend dependent)
- Root cause: concurrent apply or stale lock.
- Fix: wait or coordinate; force unlock only after confirmation.
- Verification: apply runs with lock acquired.
- Prevention: pipeline serialization; clear ownership.

### Scenario 12 — “It applied but service is broken”

- Symptoms: IaC apply succeeded, but workload unhealthy.
- Constraints: must use post-apply verification.
- Hints: Terraform success ≠ service success.
- Diagnosis:
  - run service health checks
  - inspect dependent systems
- Root cause: missing operational verification.
- Fix: rollback change if needed; add smoke checks.
- Verification: service healthy.
- Prevention: delivery gates and runbooks.

## Time-Boxed On-Call Drill (30–60 minutes)

Scenario: plan shows unexpected replacement in prod.

- constraints: no apply until risk understood; must produce rollback plan
- deliverables:
  - evidence of why replacement happens
  - containment decision (delay vs staged change)
  - updated ADR/runbook steps
