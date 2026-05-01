---
title: Rubric (0–4)
tags:
  - rubric
  - delivery
module: "07"
---

# Rubric — Module 07 Delivery

Use global rubric meanings: [Evidence and Rubrics](../../00-HOW-TO-USE/04-evidence-rubrics.md)

## Skill 1: Promotion Model and Artifact Identity

- 0: Deploys ad hoc; no artifact identity recorded.
- 1: Uses tags inconsistently; rebuilds per env sometimes.
- 2: Promotes stable artifact identity (tag+SHA or digest) across envs.
- 3: Uses digest promotion and release metadata; rollback targets explicit.
- 4: Designs org-wide promotion practices and supply-chain controls.

## Skill 2: Deployment Strategies and Rollback

- 0: Deploys without rollback plan.
- 1: Can rollback but triggers and verification unclear.
- 2: Can perform safe rollouts and rollbacks with verification signals.
- 3: Chooses strategy based on risk, telemetry, and failure domains.
- 4: Builds progressive delivery standards and reduces incident frequency.

## Skill 3: Incident Handling During Deployments

- 0: Thrashes deploys during incidents.
- 1: Captures some evidence but changes too many variables at once.
- 2: Uses containment vs fix discipline; preserves evidence.
- 3: Uses explicit windows, stop conditions, and postmortem follow-ups.
- 4: Mentors teams on incident deploy policy and prevents recurrence.

## Skill 4: Operational Writing (Runbook + ADR)

- 0: No usable docs.
- 1: Docs exist but lack signals and decision flow.
- 2: Runbook executable; ADR captures tradeoffs and rollback triggers.
- 3: Docs reduce MTTR and clarify responsibilities and gates.
- 4: Docs become reusable standards across services.
