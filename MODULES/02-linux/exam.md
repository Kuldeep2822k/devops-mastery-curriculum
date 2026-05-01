---
title: Practical Exam
tags:
  - exam
  - linux
module: "02"
---

# Practical Exam — Module 02 Linux

## Rules

- Time-box: 90 minutes.
- No internet.
- Produce an exam submission (commands + expected signals + reasoning).
- Do not print secrets.

## Tasks

### Task 1: Triage a “Service Down” Host

Scenario:

- A demo service should be listening on port 9099 but clients see failures.

Requirements:

- Use evidence to determine whether it is not running, running but not listening, or blocked by a local issue.

Grading criteria:

- correct layer identification (process vs socket vs permission vs config)
- appropriate commands and interpretation
- minimal, reversible fix
- verification via connectivity and logs

### Task 2: systemd Unit Debug

Requirements:

- Create or use a systemd unit for a demo service.
- Break it (bad ExecStart path or wrong working directory), then recover using:
  - `systemctl status`
  - `journalctl`
  - unit inspection (`systemctl cat` / `systemctl show`)

Grading criteria:

- uses logs and unit inspection to identify root cause
- fixes the unit correctly and verifies it is active
- records a rollback path (previous unit contents)

### Task 3: Resource Pressure Diagnosis

Requirements:

- Demonstrate how you would detect CPU pressure, disk pressure, and memory pressure.
- Provide commands and the expected signals for each.

Grading criteria:

- distinguishes failure patterns
- includes prevention ideas (limits, monitoring, cleanup)

### Task 4: Write Runbook + ADR

Requirements:

- Runbook: “service not listening / restart loop” with steps, commands, expected signals.
- ADR: choose a restart policy and justify tradeoffs and risks.

Grading criteria:

- runbook is executable and has verification
- ADR records constraints, options, decision, consequences

## Submission Checklist

- timeline of actions
- commands and outputs (redacted)
- root cause and fix
- verification
- prevention and follow-ups
