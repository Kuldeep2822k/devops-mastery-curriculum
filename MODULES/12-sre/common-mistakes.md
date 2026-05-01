---
title: Common Mistakes (Wrong vs Right)
tags:
  - mistakes
  - sre
module: "12"
---

# Common Mistakes — Module 12 SRE

## 1) Wrong: No Timeline

Wrong pattern:

- actions taken but not recorded

Right pattern:

- timeline with evidence and decision reasons

## 2) Wrong: Fix Before Contain

Wrong pattern:

- risky forward-fix during high severity without containment

Right pattern:

- contain first (rollback/flag), then fix with controlled change

## 3) Wrong: Postmortem With No Prevention

Wrong pattern:

- narrative only, no actionable follow-ups

Right pattern:

- concrete prevention work tracked to completion

## 4) Wrong: SLO as Poster

Wrong pattern:

- SLO exists but release behavior never changes

Right pattern:

- error budget policy gates releases and prioritizes reliability work

## 5) Wrong: Alert Fatigue Accepted

Wrong pattern:

- noisy paging treated as normal

Right pattern:

- reduce noise, improve runbooks, and protect on-call sustainability
