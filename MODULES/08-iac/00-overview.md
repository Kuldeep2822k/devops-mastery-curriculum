---
title: "Module 08: IaC (Terraform)"
tags:
  - module
  - iac
  - terraform
module: "08"
---

# Module 08 — IaC (Terraform)

## Outcomes

You can:

- Explain Terraform’s mental model: configuration → plan → apply → state.
- Manage state safely: locking, drift detection, and recovery from common state mistakes.
- Design reusable modules with clear inputs/outputs and safe defaults.
- Perform safe changes: small diffs, reversible actions, and explicit verification.
- Troubleshoot Terraform failures with evidence (state, provider errors, dependency graphs).
- Write runbooks and ADRs for IaC decisions (state backend choice, module layout, drift policy).

## Prereqs

- Terraform installed: [IaC Tooling](../../SETUP/06-iac-tooling.md)
- Git basics: [Module 03](../03-git/00-overview.md)

## Module Map

- Concepts:
  - [01-terraform-mental-model.md](01-terraform-mental-model.md)
  - [02-state-locking-and-drift.md](02-state-locking-and-drift.md)
  - [03-modules-and-composition.md](03-modules-and-composition.md)
  - [04-safe-changes-and-lifecycle.md](04-safe-changes-and-lifecycle.md)
- Deep dives:
  - [deep-dive-01.md](deep-dive-01.md)
  - [deep-dive-02.md](deep-dive-02.md)
- Labs:
  - [lab-01-local-state-plan-apply-destroy.md](lab-01-local-state-plan-apply-destroy.md)
  - [lab-02-drift-and-state-recovery.md](lab-02-drift-and-state-recovery.md)
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
2. Do lab-01 to internalize plan/apply/destroy and state hygiene.
3. Do lab-02 to practice drift detection and safe state recovery.
4. Run troubleshooting scenarios time-boxed.
5. Complete exam and self-grade.
6. Produce a runbook and an ADR about your IaC structure/backends.
