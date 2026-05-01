---
title: Rubric (0–4)
tags:
  - rubric
  - foundations
module: "01"
---

# Rubric — Module 01 Foundations

Score each skill 0–4 using the global rubric definitions in [Evidence and Rubrics](../../00-HOW-TO-USE/04-evidence-rubrics.md).

## Skill 1: Systems Thinking (Service as a Graph)

- 0: Describes only the app; cannot name dependencies or failure modes.
- 1: Names some dependencies but cannot link symptoms to layers.
- 2: Can draw a dependency graph and identify common failure modes and signals.
- 3: Can reason across layers and propose safe containment actions.
- 4: Anticipates failure domains, documents invariants, and mentors others.

## Skill 2: Signal-Based Debugging

- 0: Guesses fixes; no clear signals.
- 1: Uses a few commands but cannot interpret them reliably.
- 2: Establishes baseline, detects regression, verifies recovery via signals.
- 3: Forms hypotheses, uses minimal reversible changes, preserves evidence.
- 4: Builds reusable diagnostic playbooks and improves signals/runbooks.

## Skill 3: Change Control and Rollback

- 0: No rollback plan; changes are ad hoc.
- 1: Rollback exists but untested; unclear triggers.
- 2: Can rollback reliably and verify recovery.
- 3: Can define rollback criteria and isolate risk (small changes, clear scope).
- 4: Designs rollout/rollback as system capabilities (promotion, gating, safety).

## Skill 4: Operational Writing (Runbook + ADR)

- 0: Notes are incomplete; no step-by-step procedures.
- 1: Runbook exists but lacks verification signals and decision flow.
- 2: Runbook includes diagnosis + fix + verification; ADR records tradeoff.
- 3: Runbook supports delegation; ADR includes constraints/risks/mitigations.
- 4: Writing is concise, actionable, and prevents recurrence across incidents.

## Overall Rating Guidance

- Staff-ready signal in this module: consistent signal-based behavior and clear runbook writing under time pressure.
