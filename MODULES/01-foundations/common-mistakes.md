---
title: Common Mistakes (Wrong vs Right)
tags:
  - mistakes
  - foundations
module: "01"
---

# Common Mistakes — Module 01 Foundations

## 1) Wrong: “It worked once”

Wrong pattern:

- run health check once
- declare success

Right pattern:

- establish baseline with repeated probes
- capture expected signal and failure signal
- verify after changes over a short window

## 2) Wrong: Fix first, observe later

Wrong pattern:

- restart immediately
- change config randomly

Right pattern:

- capture initial signals and state
- write 2–3 hypotheses
- take minimal reversible actions

## 3) Wrong: Rollback = “git revert”

Wrong pattern:

- treat rollback as source history operation only

Right pattern:

- rollback is a system capability (artifact promotion, config compatibility, safe triggers)

## 4) Wrong: Runbooks as documentation dumps

Wrong pattern:

- big docs without decision flow, no commands, no expected outputs

Right pattern:

- step-by-step diagnosis with expected signals
- containment options and when to choose them
- verification steps with time window

## 5) Wrong: “Prod is special”

Wrong pattern:

- manual prod-only fixes
- environment-specific snowflakes

Right pattern:

- environment separation with promotion model
- changes validated via signals before and after deploy
