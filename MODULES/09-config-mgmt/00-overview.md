---
title: "Module 09: Config Management (Ansible)"
tags:
  - module
  - ansible
  - config-mgmt
module: "09"
---

# Module 09 — Config Management (Ansible)

## Outcomes

You can:

- Explain idempotency and why it reduces incident risk.
- Build inventories and variables with clear structure (group_vars/host_vars).
- Write safe playbooks with handlers, check mode, and minimal privileges.
- Debug playbook failures using verbose output and target inspection.
- Manage configuration drift and document operational changes.
- Write runbooks and ADRs for fleet management decisions.

## Prereqs

- Ansible installed: [Config Mgmt Tooling](../../SETUP/07-config-mgmt-tooling.md)
- Linux basics: [Module 02](../02-linux/00-overview.md)

## Module Map

- Concepts:
  - [01-idempotency-and-modules.md](01-idempotency-and-modules.md)
  - [02-inventory-and-variables.md](02-inventory-and-variables.md)
  - [03-handlers-templates-and-check-mode.md](03-handlers-templates-and-check-mode.md)
  - [04-operational-safety.md](04-operational-safety.md)
- Deep dives:
  - [deep-dive-01.md](deep-dive-01.md)
  - [deep-dive-02.md](deep-dive-02.md)
- Labs:
  - [lab-01-local-idempotent-playbook.md](lab-01-local-idempotent-playbook.md)
  - [lab-02-drift-and-safe-rollouts.md](lab-02-drift-and-safe-rollouts.md)
- Cloud extension:
  - [cloud-extension-lab.md](cloud-extension-lab.md)
- Assessment and practice:
  - [checklist.md](checklist.md)
  - [rubric.md](rubric.md)
  - [review-questions.md](review-questions.md)
  - [exam.md](exam.md)
  - [common-mistakes.md](common-mistakes.md)
  - [troubleshooting.md](troubleshooting.md)
  - [troubleshooting-lab.md](troubleshooting-lab.md)
- Writing templates:
  - [runbook-template.md](runbook-template.md)
  - [decision-record-template.md](decision-record-template.md)

## Completion Path (Recommended)

1. Read concepts (01–04) and deep dives.
2. Do lab-01 to build idempotent playbook muscle memory.
3. Do lab-02 to practice drift, check mode, and safe changes.
4. Run troubleshooting scenarios time-boxed.
5. Complete exam and self-grade.
6. Write a runbook and an ADR about your automation policy.
