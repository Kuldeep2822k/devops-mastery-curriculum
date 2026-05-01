---
title: Rubric (0–4)
tags:
  - rubric
  - terraform
module: "08"
---

# Rubric — Module 08 IaC (Terraform)

Use global rubric meanings: [Evidence and Rubrics](../../00-HOW-TO-USE/04-evidence-rubrics.md)

## Skill 1: Terraform Mental Model and Planning Discipline

- 0: Applies without understanding diffs; no state awareness.
- 1: Can run commands but struggles to interpret plan and replacement risks.
- 2: Reads plan reliably; keeps diffs small; uses validate/fmt.
- 3: Designs safe change workflows and explicit verification gates.
- 4: Drives org-wide safe IaC standards and review culture.

## Skill 2: State and Drift Operations

- 0: Corrupts or loses state; panics and deletes state.
- 1: Knows state exists but cannot recover or refactor safely.
- 2: Can detect drift and reconcile; uses state show/list.
- 3: Can use state mv/rm safely with a rollback plan and evidence.
- 4: Designs drift policy and safe incident workflows; minimizes manual drift.

## Skill 3: Module Design and Composition

- 0: No module boundaries; copy/paste configs everywhere.
- 1: Uses modules but unclear inputs/outputs and weak defaults.
- 2: Clean module boundaries and useful outputs; envs are thin.
- 3: Can evolve modules without breaking consumers; keeps contracts stable.
- 4: Builds reusable golden modules and guardrails.

## Skill 4: Troubleshooting and Operational Writing

- 0: Cannot debug provider/state errors.
- 1: Debugs via guesswork; no runbooks.
- 2: Evidence-based debugging; runbook is executable.
- 3: Strong runbooks and ADR tradeoff writing; reduces MTTR.
- 4: Creates operational playbooks that scale across teams.
