---
title: Practical Exam
tags:
  - exam
  - web-proxy
module: "14"
---

# Practical Exam — Module 14 Web Proxy

## Rules

- Time-box: 120 minutes.
- No internet.
- Submit evidence: configs + commands + outputs (redacted) + reasoning.

## Tasks

### Task 1: Configure a Reverse Proxy

Requirements:

- run a local upstream service
- run a reverse proxy in front of it
- forward required headers safely

Grading criteria:

- proxy routes correctly and verification is clear

### Task 2: Break/Fix Drill

Choose 2:

- wrong upstream port (502)
- timeout (504)
- missing headers (client IP / host)

For each:

- reproduce
- diagnose using evidence
- fix and verify

### Task 3: Runbook + ADR

Requirements:

- Runbook: “502/503/504 from proxy” with decision flow and first commands.
- ADR: choose proxy timeouts/retry/header policy and justify tradeoffs.
