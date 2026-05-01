---
title: "Deep Dive 01: Postmortems That Actually Prevent Incidents"
tags:
  - sre
  - deep-dive
  - postmortem
module: "12"
---

# Deep Dive 01 — Postmortems That Actually Prevent Incidents

## The Output Is Not the Document

The output is prevention work:

- code changes
- alerts fixed
- runbooks improved
- ownership clarified

If postmortems produce no changes, the process is theater.

## Good Postmortem Structure

- impact and timeline
- what happened (facts)
- contributing factors (not blame)
- what went well / what didn’t
- action items with owners and deadlines

## Action Items Quality

Bad:

- “be more careful”

Good:

- “add canary gate based on p95 latency”
- “add runbook for Service 503 with endpoints check”

## Staff-Level Behavior

- drive prevention items to completion
- reduce recurrence and MTTR over time
