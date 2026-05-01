---
title: Troubleshooting Lab (Scenarios)
tags:
  - troubleshooting-lab
  - sre
  - oncall
module: "12"
---

# Troubleshooting Lab — Module 12 SRE

## Instructions

- Do 12–20 scenarios.
- Time-box each: 10–20 minutes.
- Produce: timeline, containment, verification, prevention tasks.

## Scenarios

### Scenario 01 — No IC, Everyone “Doing Stuff”

- Symptoms: confusion, duplicated work, no comms.
- Fix: assign IC and comms; enforce timeline logging.
- Verification: clear single-threaded decision flow.

### Scenario 02 — Rollback vs Forward-Fix Decision

- Symptoms: regression after deploy.
- Constraints: rollback safe unless migrations incompatible.
- Fix: decide based on evidence and rollback safety.
- Verification: user signals improve over window.

### Scenario 03 — Error Budget Exhausted

- Symptoms: repeated incidents; SLO budget depleted.
- Fix: define release freeze policy; prioritize reliability work.
- Verification: fewer incidents and slower budget burn.

### Scenario 04 — Alert Storm

- Symptoms: 20 pages from one dependency outage.
- Fix: define top-level alert; suppress symptoms.
- Verification: next incident pages fewer times.

### Scenario 05 — Toil Spike

- Symptoms: on-call spends hours on repetitive restarts.
- Fix: write runbook; automate safe restart with guardrails.
- Verification: toil hours reduce.

### Scenario 06 — Capacity Cliff

- Symptoms: traffic spike causes cascading failures.
- Fix: add headroom policy; tune retries/timeouts; load shedding.
- Verification: system survives same spike in drill.

### Scenario 07 — Comms Failure

- Symptoms: stakeholders not informed; rumors.
- Fix: comms role and cadence; clear updates.
- Verification: updates include impact/status/next steps/next update time.

### Scenario 08 — Postmortem Has No Action Items

- Symptoms: same incident repeats.
- Fix: add concrete prevention tasks with owners.
- Verification: tasks tracked and completed.

### Scenario 09 — Incident Evidence Lost

- Symptoms: logs overwritten after restart; no RCA.
- Fix: capture evidence first; adjust retention.
- Verification: evidence artifacts exist after incident.

### Scenario 10 — “Recovered” Too Early

- Symptoms: incident reopens; no monitoring window.
- Fix: define verification window and rollback triggers.
- Verification: stability confirmed for defined period.

### Scenario 11 — Break-Glass Misuse

- Symptoms: approvals bypassed casually.
- Fix: define break-glass rules and audits.
- Verification: overrides are rare and documented.

### Scenario 12 — Runbook Rot

- Symptoms: runbook steps don’t match reality.
- Fix: update runbook from live incident evidence.
- Verification: next on-call can execute runbook successfully.

## Time-Boxed On-Call Drill (30–60 minutes)

Scenario: user-facing outage after deploy.

- deliverables:
  - roles and comms cadence
  - containment decision with evidence
  - verification window
  - postmortem with prevention tasks
