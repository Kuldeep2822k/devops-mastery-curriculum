---
title: Runbook Template (Module 12)
tags:
  - runbook
  - template
  - sre
module: "12"
---

# Runbook Template — Module 12 SRE

## Title

<Service> — <Incident Type>

## Impact

- user impact:
- scope:
- severity:

## Roles

- IC:
- responder:
- comms:

## Safety and Preconditions

- preserve evidence before rollback/restart
- one change at a time under high severity

## Quick Triage (10 minutes)

- confirm impact (SLO, errors, latency)
- identify recent changes (deploys/config)
- choose containment action

## Containment

- rollback to known good
- disable feature flag
- reduce load / isolate dependency

## Verification

- define verification window:
- signals to watch:
- rollback triggers:

## Post-Incident

- postmortem timeline
- prevention tasks
- runbook updates
