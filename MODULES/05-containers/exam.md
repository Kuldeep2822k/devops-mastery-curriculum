---
title: Practical Exam
tags:
  - exam
  - containers
module: "05"
---

# Practical Exam — Module 05 Containers

## Rules

- Time-box: 120 minutes.
- No internet.
- Submit evidence: commands + expected signals + reasoning.
- Do not print secrets.

## Tasks

### Task 1: Build and Run

Requirements:

- Build an image for a small service.
- Run it with port mapping.
- Verify health endpoint from host.

Grading criteria:

- Dockerfile is minimal and correct
- port mapping and health check succeed
- cleanup removes containers/images created

### Task 2: Evidence-Based Debug

Requirements:

- Create a failing container (wrong command, missing file, wrong port mapping, or permission issue).
- Diagnose using logs and inspect (no guessing).
- Fix and verify recovery.

Grading criteria:

- correct root cause identified
- fix is minimal and reversible
- verification uses health checks and runtime inspection

### Task 3: Resource Limits Drill

Requirements:

- Run a container with constrained memory and observe behavior.
- Explain what evidence indicates OOM or memory pressure.

Grading criteria:

- uses `docker stats` and inspect/exit codes
- provides a tuning strategy based on evidence

### Task 4: Runbook + ADR

Requirements:

- Runbook: “Container exits on start / service unreachable / permission denied on volume” with commands and expected outputs.
- ADR: choose tagging policy (digest promotion vs tags) or debug image strategy and justify tradeoffs.

Grading criteria:

- runbook is executable and safe
- ADR captures constraints, options, decision, risks, mitigations
