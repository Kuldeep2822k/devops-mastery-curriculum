---
title: Operational Writing (Runbooks and ADRs)
tags:
  - foundations
  - documentation
  - runbooks
  - adr
module: "01"
---

# Operational Writing (Runbooks and ADRs)

## Why Writing Is an SRE/Platform Superpower

Writing is how you:

- reduce MTTR by making diagnosis steps repeatable
- avoid repeated incidents by documenting prevention
- communicate tradeoffs so teams can move fast without arguing forever
- scale yourself as an operator

## Runbooks: What They Are and Aren’t

Runbook:

- a step-by-step operational procedure with verification signals
- includes safe containment, diagnosis, fix, and escalation paths

Not a runbook:

- “restart the service”
- a wall of logs
- a list of links without a decision flow

Minimum runbook sections:

- impact definition
- prechecks (what to verify before acting)
- diagnosis (commands + expected signals)
- containment (safe mitigations)
- fix (minimal actions + rollback)
- verification (signals + time window)
- prevention (follow-ups)

## ADRs: Capture the Tradeoff, Not the Winner

An ADR is valuable because it records:

- what constraints existed
- what options were considered
- why the chosen option won under those constraints
- what risks were accepted and how they will be mitigated

This prevents:

- rewriting history during incidents
- repeating the same debate every quarter

## Operational Writing Under Pressure

During an incident:

- you will forget what you did
- you will lose evidence as systems restart

So you write as you act:

- log timestamps and reasons
- capture commands and outputs (redacted)
- record verification signals after every change

Use the incident log template: [Notes Template](../../00-HOW-TO-USE/06-notes-template.md)
