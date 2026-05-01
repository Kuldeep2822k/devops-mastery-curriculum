---
title: Checklist (Definition of Done)
tags:
  - checklist
  - terraform
module: "08"
---

# Checklist — Module 08 IaC (Terraform) (DoD)

- [ ] Explain Terraform core loop and why state matters.
- [ ] Demonstrate safe hygiene:
  - [ ] `terraform fmt`
  - [ ] `terraform validate`
  - [ ] plan review before apply
- [ ] Demonstrate state safety:
  - [ ] state is not committed to git
  - [ ] can explain locking and why concurrent applies corrupt state
- [ ] Complete Lab 01 with evidence (init/plan/apply/destroy).
- [ ] Complete Lab 02 with evidence:
  - [ ] drift introduced and detected via plan
  - [ ] drift reconciled via apply
  - [ ] resource refactor via `state mv` without replacement
- [ ] Complete at least 12 scenarios from [troubleshooting-lab.md](troubleshooting-lab.md).
- [ ] Complete [exam.md](exam.md) with evidence (commands + expected signals + reasoning).
- [ ] Write a Terraform runbook using [runbook-template.md](runbook-template.md).
- [ ] Write one ADR using [decision-record-template.md](decision-record-template.md) about module layout/state backend/drift policy.
- [ ] Self-grade via [rubric.md](rubric.md) and list 3 improvement actions.
