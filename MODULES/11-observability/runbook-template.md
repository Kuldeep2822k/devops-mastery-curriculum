---
title: Runbook Template (Module 11)
tags:
  - runbook
  - template
  - observability
module: "11"
---

# Runbook Template — Module 11 Observability

## Title

<Service> — <Alert (high errors / high latency / saturation)>

## Impact

- user impact:
- scope:
- severity:

## Safety and Preconditions

- do not print secrets
- capture evidence before rollback/restart

## Quick Triage (10 minutes)

- confirm signal:
  - error rate
  - p95 latency
  - traffic and saturation
- check deploy markers/version changes
- check dependency signals (timeouts, retries)

## Evidence to Capture

- last 15–30 minutes of logs (structured)
- current deployed version/artifact identity
- key dashboard snapshots (errors/latency/saturation)
- example trace IDs (if available)

## Diagnosis

- hypotheses:
- evidence for/against each hypothesis:

## Containment

- rollback to known good
- disable feature flag
- reduce traffic/load

## Verification

- error/latency signals recover over a window
- no new alert storms
- user-facing checks pass

## Prevention / Follow-Ups

- alert hygiene improvements
- add missing signals (version, dependency health)
- update runbook with proven first steps
