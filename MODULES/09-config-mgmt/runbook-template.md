---
title: Runbook Template (Module 09)
tags:
  - runbook
  - template
  - ansible
module: "09"
---

# Runbook Template — Module 09 Ansible

## Title

Ansible Rollout — <Symptom (bad config / playbook failing / drift detected)>

## Impact

- Service/user impact:
- Scope (hosts/groups):
- Severity:

## Safety and Preconditions

- Use canary first: `--limit`.
- Use check mode before apply for risky changes.
- Do not print secrets.

## Quick Triage

```bash
ansible-inventory -i inventory.ini --graph
ansible-playbook -i inventory.ini site.yml --list-hosts
ansible-playbook -i inventory.ini site.yml --check --diff
```

## Diagnosis

- confirm target hosts
- confirm variable sources
- identify first failing task

Debug commands:

```bash
ansible-playbook -i inventory.ini site.yml -vv
```

## Containment

- stop rollout
- revert to previous template/config
- apply rollback to a canary group first

## Fix

- correct template/vars
- use handlers for restarts
- reduce blast radius (serial/batches)

## Verification

- second run is idempotent (no changes)
- service health checks pass
- logs normal after restart/reload

## Prevention / Follow-Ups

- enforce check-mode preview in CI
- idempotency test (run twice)
- secrets policy and audits
