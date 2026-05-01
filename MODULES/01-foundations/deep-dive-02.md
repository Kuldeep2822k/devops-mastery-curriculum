---
title: "Deep Dive 02: Incident Response Under Pressure (Human Factors)"
tags:
  - foundations
  - deep-dive
  - incident-response
module: "01"
---

# Deep Dive 02 — Incident Response Under Pressure (Human Factors)

## The Hidden Failure Mode: Humans Under Stress

During incidents:

- working memory collapses
- confirmation bias increases
- communication quality drops
- changes get made without logging

Your incident process exists to counter predictable human failure patterns.

## The “Process Is the Product” Mindset

Reliable teams:

- define severity and roles quickly
- separate containment from diagnosis
- limit simultaneous changes
- keep a clean timeline
- communicate clearly and frequently

## Roles (Even in a Team of One)

When solo, you still simulate roles:

- incident commander (keeps time, scope, comms)
- primary responder (runs commands, executes changes)
- scribe (keeps timeline and evidence)

If you are one person, you alternate but you keep the mindset.

## Communication as an Operational Control

Bad comms causes:

- duplicated effort
- contradictory changes
- delayed escalation
- loss of trust

Good comms includes:

- current impact
- what changed recently
- what you tried and results
- next hypothesis and next step

## When to Rollback vs Forward-Fix

Rollback is often the best containment when:

- a deploy correlates strongly with impact
- you have a known-good artifact
- forward-fix requires risky changes

Forward-fix is reasonable when:

- rollback is impossible (schema incompatible, irreversible change)
- bug exists in old versions too
- rollback increases risk (e.g., state mismatch)

## Evidence Preservation

Before you restart or roll back:

- capture current logs/events/state
- record current versions/config

Otherwise you erase the very evidence needed for root cause analysis.

## The “One Restart Budget” Drill

A training constraint:

- you get one restart of the service/pod/host

Why:

- forces diagnosis over superstition
- prevents evidence destruction

## Prevention Must Be Specific

Weak prevention:

- “be more careful”

Strong prevention:

- add a preflight check in CI
- add an alert with a runbook link
- add a canary stage with automatic rollback trigger
- add a safe config validation step before deploy
