---
title: Practical Exam
tags:
  - exam
  - ansible
module: "09"
---

# Practical Exam — Module 09 Config Mgmt (Ansible)

## Rules

- Time-box: 120 minutes.
- No internet.
- Submit evidence: commands + expected signals + reasoning.
- Do not print secrets.

## Tasks

### Task 1: Idempotent Playbook

Requirements:

- create a playbook that creates a directory and a templated config file
- demonstrate second run produces no changes

Grading criteria:

- module usage (file/template/copy)
- idempotency proven by repeated run

### Task 2: Check Mode Gate

Requirements:

- introduce drift by editing file manually
- show check mode detects planned correction
- apply change and verify drift corrected

Grading criteria:

- correct `--check --diff` usage and interpretation

### Task 3: Safe Rollout Controls

Requirements:

- demonstrate `--limit` and `serial` behavior to reduce blast radius
- define a stop condition and rollback plan in writing

Grading criteria:

- canary-first logic and clear rollback plan

### Task 4: Runbook + ADR

Requirements:

- Runbook: “Bad config rollout” with steps, commands, verification signals.
- ADR: choose rollout policy and secret handling approach.
