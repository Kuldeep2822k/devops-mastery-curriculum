---
title: 'Terraform + Ansible Failures'
tags:
  - catalog
  - terraform
  - ansible
  - iac
  - incident
---

# Terraform + Ansible Failures — Troubleshooting Scenarios

Use this when provisioning or configuration changes fail, drift appears, or automation is unsafe/unreliable.

## Fast Triage Checklist

- Identify phase: plan vs apply vs post-apply config.
- Confirm environment/workspace selection (wrong dir/workspace is a common failure mode).
- Collect evidence: plan output, state snapshot (without exposing secrets), run logs.
- Prefer rollback-by-revert over “hot edits” to state unless you understand consequences.

## Terraform Baseline Commands

- `terraform version`
- `terraform fmt -check`
- `terraform validate`
- `terraform plan -out tfplan`
- `terraform show -no-color tfplan | sed -n '1,200p'`
- `terraform state list | head`

## Ansible Baseline Commands

- `ansible --version`
- `ansible-inventory -i inventory --graph || true`
- `ansible-playbook -i inventory playbook.yml --check --diff`
- `ansible-playbook -i inventory playbook.yml -vvv --limit <host/group>`

## Scenarios

### Scenario 01 — Terraform plan shows unexpected replacement

- Symptoms: Plan wants to destroy/recreate critical resources.
- Constraints: No destructive change without explicit approval.
- Diagnosis commands:
  - `terraform plan -out tfplan`
  - `terraform show tfplan | sed -n '1,220p'`
  - `terraform state show <addr> | sed -n '1,220p'`
- Root cause: ForceNew attribute changed, immutable field modified, drift, wrong input.
- Fix: Revert input change, use `create_before_destroy` where safe, or do a two-step migration.
- Verify: New plan is additive or controlled.
- Prevention: Small diffs, review gates, ADR for lifecycle choices.

### Scenario 02 — State lock stuck / cannot acquire lock

- Symptoms: “Error acquiring the state lock”; pipeline blocked.
- Constraints: Avoid forced unlock unless you’re sure no apply is running.
- Diagnosis commands:
  - Identify locker info from error output.
  - Confirm no active applies in CI.
- Root cause: Crashed apply or backend issue.
- Fix: Follow backend-specific safe unlock steps; re-run plan/apply.
- Verify: Lock clears; apply succeeds once.
- Prevention: Timeouts, CI cancellation handling, backend health checks.

### Scenario 03 — Drift detected (manual change)

- Symptoms: Plan shows changes without code change.
- Constraints: Don’t “accept drift” silently.
- Diagnosis commands:
  - `terraform plan`
  - `terraform refresh` (if applicable) or provider data sources.
- Root cause: Manual console edits, auto-scaling, out-of-band automation.
- Fix: Decide: revert drift to code, or codify change then apply.
- Verify: Plan converges to desired state.
- Prevention: Policy guardrails, permissions, drift alerts.

### Scenario 04 — Provider auth failure

- Symptoms: 401/403; “no valid credential sources”.
- Constraints: Do not paste secrets in logs.
- Diagnosis commands:
  - `env | sort | grep -E 'AWS_|AZURE_|GOOGLE_|TF_' || true` (redact before sharing)
  - `terraform providers`
- Root cause: Missing env vars, expired session, wrong profile, wrong region/project.
- Fix: Re-auth; use workload identity; validate account/region.
- Verify: `terraform plan` reaches provider APIs.
- Prevention: Short-lived creds, CI OIDC, explicit env separation.

### Scenario 05 — Cyclic dependency / graph error

- Symptoms: Terraform error about cycle.
- Diagnosis commands:
  - `terraform graph | head -n 50`
  - Review module outputs/inputs.
- Root cause: Mutual references between resources/modules.
- Fix: Break cycle via data source, separate module, or explicit dependency inversion.
- Verify: Plan succeeds.
- Prevention: Module boundaries + ADRs.

### Scenario 06 — Ansible playbook not idempotent

- Symptoms: Second run still reports changes; causes unnecessary restarts.
- Diagnosis commands:
  - `ansible-playbook ... --check --diff`
  - `ansible-playbook ... -vvv | tail -n 200`
- Root cause: Shell commands without guards, non-deterministic templates, missing `creates=`.
- Fix: Replace with idempotent module, add `changed_when`, enforce stable templates.
- Verify: Second run reports 0 changes.
- Prevention: CI idempotency gate, rubrics and checklists.

### Scenario 07 — Ansible fails with “UNREACHABLE”

- Symptoms: SSH timeouts, host unreachable.
- Diagnosis commands:
  - `ansible -i inventory all -m ping -vvv`
  - `ssh -vvv <host> true`
- Root cause: Network ACL, DNS, wrong user/key, host down.
- Fix: Restore connectivity; fix inventory vars; rotate keys safely.
- Verify: `ansible -m ping` succeeds.
- Prevention: Connectivity checks, bastion strategy, runbook for access restore.

### Scenario 08 — Drift after config rollout (partial apply)

- Symptoms: Some hosts updated, others not; inconsistent behavior.
- Constraints: Avoid widening blast radius.
- Diagnosis commands:
  - `ansible-playbook ... --limit <subset> -vv`
  - Compare host facts and service versions.
- Root cause: Batch/serial misconfig, failed handler mid-run, flaky dependency.
- Fix: Resume safely with `--limit`; ensure handlers run; document run state.
- Verify: Fleet converges; health checks stable.
- Prevention: Serial batches, health gates, resume-safe design.

