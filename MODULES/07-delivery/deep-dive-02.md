---
title: "Deep Dive 02: Deployments During Incidents (Containment vs Change)"
tags:
  - delivery
  - deep-dive
  - incident
module: "07"
---

# Deep Dive 02 — Deployments During Incidents (Containment vs Change)

## The Dangerous Moment

During incidents, teams often try:

- “deploy a quick fix”
- “restart everything”
- “scale up”

These can increase blast radius if you are not disciplined.

## Default Incident Rule: Prefer Rollback to Known Good

Rollback is often safer than forward-fix when:

- you have a known-good artifact
- the regression is correlated with the last deploy
- your signals show a clear degradation

Forward-fix is justified when:

- rollback is unsafe (schema incompatibility)
- the bug existed before
- the incident is caused by an external dependency

## Containment vs Fix

Containment:

- reduces impact quickly
- may not explain root cause

Fix:

- addresses root cause
- may be risky during high severity

Staff-level behavior:

- contain first when needed
- preserve evidence
- fix later with a controlled change

## Avoid “Thrash Deploying”

Thrash patterns:

- multiple deploys in quick succession without verification windows
- changing multiple variables (app + config + infra)

Better:

- one change at a time
- explicit verification windows
- clear rollback triggers

## What to Document

During incident deploys, always record:

- which artifact identity was deployed
- why it was deployed
- what signal improved or worsened
