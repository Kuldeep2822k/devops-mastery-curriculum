---
title: Common Mistakes (Wrong vs Right)
tags:
  - mistakes
  - delivery
module: "07"
---

# Common Mistakes — Module 07 Delivery

## 1) Wrong: Deploy “Latest”

Wrong pattern:

- deploy `:latest` or untracked images

Right pattern:

- deploy immutable identity (digest/tag+SHA) and record it

## 2) Wrong: No Rollback Triggers

Wrong pattern:

- “watch and see”

Right pattern:

- explicit error/latency/health thresholds and a time window

## 3) Wrong: Migrations Without Plan

Wrong pattern:

- run destructive migrations with no rollback path

Right pattern:

- expand/contract; feature flags; staged changes

## 4) Wrong: Canary Without Signals

Wrong pattern:

- canary is “one pod” but no telemetry or decision criteria

Right pattern:

- define signals and stop conditions before rollout

## 5) Wrong: Thrash Deploying During Incidents

Wrong pattern:

- multiple deploys without verification windows

Right pattern:

- contain (rollback) first when needed; preserve evidence; fix later with controlled change
