---
title: Troubleshooting Lab (Scenarios)
tags:
  - troubleshooting-lab
  - programming
  - oncall
module: "13"
---

# Troubleshooting Lab — Module 13 Programming for DevOps

## Instructions

- Do 12–20 scenarios.
- Time-box each: 10–20 minutes.
- Record: symptoms, constraints, hints, diagnosis steps, fix, verification, prevention.

## Scenarios

### Scenario 01 — Runaway Script (No Bounds)

- Symptoms: tool processes far more targets than intended.
- Fix: add max limits and allowlists; dry-run default.
- Verification: tool stops at limit and exits non-zero.

### Scenario 02 — Tool Hangs

- Symptoms: never completes.
- Fix: add timeouts and cancellation.
- Verification: tool fails fast with clear message.

### Scenario 03 — Retry Storm Against API

- Symptoms: 429/5xx spiral; upstream degradation.
- Fix: backoff with jitter; cap retries; respect Retry-After.
- Verification: request rate drops and tool completes safely.

### Scenario 04 — Parsing Breaks After API Change

- Symptoms: JSON shape changed and script crashes.
- Fix: validate schema and handle missing fields.
- Verification: tool prints actionable error and exits non-zero.

### Scenario 05 — Duplicate Writes

- Symptoms: rerun causes duplicates.
- Fix: idempotency keys or upsert logic; checkpoints.
- Verification: rerun produces no new side effects.

### Scenario 06 — Secrets in Logs

- Symptoms: token printed in stdout.
- Fix: redact and rotate token; remove printing.
- Verification: logs contain no secrets.

### Scenario 07 — Wrong Environment Targeted

- Symptoms: prod actions executed unintentionally.
- Fix: environment flag required; safe defaults; confirmations.
- Verification: tool refuses without explicit env.

### Scenario 08 — Partial Failure Mid-Run

- Symptoms: some items processed, then failure.
- Fix: checkpointing and resumability.
- Verification: rerun resumes without duplicating work.

### Scenario 09 — Concurrency Causes Race

- Symptoms: inconsistent results.
- Fix: serialize critical sections; reduce concurrency.
- Verification: repeated runs stable.

### Scenario 10 — CLI UX Confusing

- Symptoms: operator misuses flags.
- Fix: improve help text and defaults.
- Verification: `--help` clearly describes safety behavior.

### Scenario 11 — Output Not Machine-Readable

- Symptoms: cannot integrate into CI.
- Fix: provide JSON output mode.
- Verification: downstream tool can parse output reliably.

### Scenario 12 — Unexpected Deletions Need Rollback

- Symptoms: deleted resources must be restored.
- Fix: implement “trash” pattern or backup mode.
- Verification: rollback recovers state.

## Time-Boxed On-Call Drill (30–60 minutes)

Scenario: automation tool caused API rate limiting incident.

- deliverables:
  - containment (stop tool, reduce load)
  - evidence (request rate, errors)
  - fix (backoff, limits, dry-run)
  - prevention (policy for automation)
