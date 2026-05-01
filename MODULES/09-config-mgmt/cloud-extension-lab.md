---
title: Cloud Extension Lab (Optional)
tags:
  - cloud
  - optional
  - ansible
module: "09"
---

# Cloud Extension Lab (Optional) — Ansible on Real Hosts

Core learning is local-first. This extension maps playbooks to real VMs.

## Goal

Practice:

- SSH connectivity and inventory for real hosts
- least-privilege become usage
- staged rollout (canary → fleet)
- rollback by reapplying previous templates

## Cost Control (Mandatory)

- set budgets and alerts before provisioning
- tag resources with owner/purpose/expires_on/module
- destroy VMs immediately after verification

Read: [Lab Safety and Cost Control](../../00-HOW-TO-USE/03-lab-safety-cost-control.md)

## Steps (High Level)

1. Provision 2 small VMs (or reuse existing safe hosts).
2. Configure SSH keys (no passwords in playbooks).
3. Run a canary playbook on one host with `--limit`.
4. Expand rollout to both hosts using serial execution.
5. Break config on one host manually, detect via `--check`, reconcile.

## Verification Signals

- idempotent runs
- canary-first rollout behavior
- drift detected and corrected

## Cleanup

- remove files created by playbooks if desired
- destroy cloud resources
