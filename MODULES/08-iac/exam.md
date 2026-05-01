---
title: Practical Exam
tags:
  - exam
  - terraform
module: "08"
---

# Practical Exam — Module 08 IaC (Terraform)

## Rules

- Time-box: 120 minutes.
- No internet.
- Submit evidence: commands + expected signals + reasoning.
- Do not commit or print secrets/state.

## Tasks

### Task 1: Build a Small Local Terraform Stack

Requirements:

- use a local-only provider (local_file or equivalent)
- produce an output value
- include a `.gitignore` that excludes state

Grading criteria:

- init/validate/plan/apply succeed
- output is correct
- state not committed

### Task 2: Drift Detection and Reconciliation

Requirements:

- introduce drift outside Terraform
- detect drift via plan
- reconcile via apply

Grading criteria:

- evidence shows drift diff and reconciliation

### Task 3: Safe Refactor with state mv

Requirements:

- rename a resource in code
- use `terraform state mv` to avoid replace
- plan shows no destroy/create

Grading criteria:

- state move is correct and verified

### Task 4: Runbook + ADR

Requirements:

- Runbook: “Terraform plan shows unexpected changes / apply failed” with evidence-based steps.
- ADR: choose environment separation approach (dirs vs workspaces) or backend choice and justify tradeoffs.

Grading criteria:

- runbook executable and safe
- ADR includes constraints, options, decision, risks, mitigations
