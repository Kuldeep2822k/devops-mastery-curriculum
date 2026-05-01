---
title: Troubleshooting Lab (Scenarios)
tags:
  - troubleshooting-lab
  - ansible
  - oncall
module: "09"
---

# Troubleshooting Lab — Module 09 Config Mgmt (Ansible)

## Instructions

- Do 12–20 scenarios.
- Time-box each: 10–20 minutes.
- Record: symptoms, constraints, hints, diagnosis commands, root cause, fix, verification, prevention.

## Scenarios

### Scenario 01 — Wrong Host Targeted

- Symptoms: playbook changes unexpected machine.
- Constraints: do not run on entire fleet.
- Hints: inventory and `--limit`.
- Diagnosis:
  - `ansible-inventory -i inventory.ini --graph`
  - `ansible-playbook ... --list-hosts`
- Root cause: inventory groups wrong.
- Fix: correct inventory; use `--limit` for safety.
- Verification: only intended hosts listed.
- Prevention: canary-first standard.

### Scenario 02 — Variables Overridden Unexpectedly

- Symptoms: wrong config values rendered.
- Constraints: must identify variable source.
- Hints: precedence.
- Diagnosis:
  - print vars safely (no secrets)
  - inspect group_vars/host_vars
- Root cause: variable override from higher precedence source.
- Fix: simplify variable sources; document defaults.
- Verification: rendered file has expected values.
- Prevention: variable policy and review checklist.

### Scenario 03 — Playbook Not Idempotent

- Symptoms: every run shows changed.
- Constraints: must be idempotent.
- Hints: unstable templates or shell.
- Diagnosis:
  - run twice and compare output
  - `--diff` to see what changes
- Root cause: nondeterministic content.
- Fix: remove timestamps/randomness; use modules.
- Verification: second run no changes.
- Prevention: CI job that runs playbook twice.

### Scenario 04 — Handler Never Runs

- Symptoms: config changes but service not restarted.
- Constraints: restarts must be conditional.
- Hints: notify name mismatch.
- Diagnosis:
  - `ansible-playbook -v`
- Root cause: notify not triggered or handler name mismatch.
- Fix: correct notify/handler names.
- Verification: handler runs when config changes.
- Prevention: unit tests for playbooks (later).

### Scenario 05 — Handler Runs Every Time

- Symptoms: service restarts every run.
- Constraints: minimize disruption.
- Hints: tasks always changed.
- Diagnosis:
  - identify task causing change
- Root cause: non-idempotent task.
- Fix: make task idempotent; restart only on actual change.
- Verification: second run no handler.
- Prevention: enforce idempotency checks.

### Scenario 06 — Check Mode Shows Changes but Apply Doesn’t

- Symptoms: `--check` indicates change but apply results differ.
- Constraints: must interpret check-mode limitations.
- Hints: module may not fully support check.
- Diagnosis:
  - compare check output vs actual run
- Root cause: check mode limitation.
- Fix: rely on check as preview only; verify after apply.
- Verification: actual state matches desired.
- Prevention: choose modules that support check/diff.

### Scenario 07 — Connection Failures

- Symptoms: unreachable host.
- Constraints: cannot disable SSH security.
- Hints: SSH keys and inventory host.
- Diagnosis:
  - `ssh -vvv user@host` (redacted)
- Root cause: wrong key, wrong host, firewall.
- Fix: correct inventory and auth.
- Verification: `ansible -m ping` works.
- Prevention: standard SSH baseline.

### Scenario 08 — Permissions Denied Writing File

- Symptoms: file module fails with permission denied.
- Constraints: least privilege.
- Hints: become only where needed.
- Diagnosis:
  - confirm target path ownership
- Root cause: needs become or wrong path.
- Fix: use become for that task only; fix permissions.
- Verification: file created and correct perms.
- Prevention: define ownership model.

### Scenario 09 — Drift Returns After Playbook

- Symptoms: config gets changed back.
- Constraints: identify external actor.
- Hints: other automation or manual edits.
- Diagnosis:
  - check timestamps and external processes
- Root cause: competing automation.
- Fix: stop competing tool or define ownership boundaries.
- Verification: drift stays fixed over time window.
- Prevention: single source of truth.

### Scenario 10 — Large Blast Radius Change

- Symptoms: playbook changes too many hosts at once.
- Constraints: must stage rollout.
- Hints: serial/batches and canary.
- Diagnosis:
  - `--list-hosts`
- Root cause: no limit/serial set.
- Fix: run canary with `--limit`, set `serial`.
- Verification: only small subset changed first.
- Prevention: runbook for rollouts.

### Scenario 11 — Template Renders Secrets to Disk

- Symptoms: secret values appear in rendered files.
- Constraints: do not store secrets in plaintext if avoidable.
- Hints: vault/secret manager; file permissions.
- Diagnosis:
  - inspect template inputs (do not print secrets)
- Root cause: secrets in plain vars.
- Fix: use vault; restrict permissions; minimize plaintext exposure.
- Verification: secrets handling improved and documented.
- Prevention: policy checks and reviews.

### Scenario 12 — Inventory Drift

- Symptoms: host list inaccurate; playbook misses machines.
- Constraints: inventory must be correct.
- Hints: source-of-truth integration (later).
- Diagnosis:
  - inventory graph and list
- Root cause: inventory not maintained.
- Fix: update inventory; define ownership.
- Verification: inventory reflects reality.
- Prevention: periodic inventory audit.

## Time-Boxed On-Call Drill (30–60 minutes)

Scenario: bad config rollout causes service errors.

- constraints: must use `--limit` canary and rollback template quickly
- deliverables:
  - incident timeline
  - containment and rollback steps
  - prevention tasks (handlers, checks, canary policy)
