---
title: Environments and Change Control
tags:
  - foundations
  - environments
  - change-management
module: "01"
---

# Environments and Change Control

## Why Environments Exist

Environments are not “places to run code”. They are risk controls.

- dev: fastest feedback; lowest assurance
- staging/preprod: realism; integration; change rehearsal
- prod: user impact; strongest controls

The key is controlled promotion: the same artifact moves forward with increasing scrutiny.

## Environment Separation (Practical)

Separations to aim for:

- configuration separation (different endpoints, credentials, feature flags)
- data separation (avoid prod data in non-prod)
- identity separation (least privilege per env)
- blast radius separation (a staging failure should not impact prod)

Tradeoff:

- perfect parity is expensive
- insufficient parity creates “works in staging, fails in prod”

## Change Types and Risk

Classify changes by risk:

- low risk: docs, non-functional refactors, internal tooling
- medium risk: config changes, dependency upgrades, scaling changes
- high risk: schema migrations, auth changes, network policies, rollouts affecting routing

### What “Safe Change” Looks Like

- small diff (few moving parts)
- reversible (rollback path exists and is tested)
- observable (you can verify impact quickly)
- scoped (feature flags, progressive rollout, canary)

## Rollback Is an Operational Feature

Rollback is not “git revert”. It is a system capability:

- you can deploy an older artifact safely
- schema and config changes do not break old versions
- you can undo traffic/routing changes
- you can explain what state you’re rolling back to

## Anti-Patterns

- changing code and config at the same time without observability
- deploying “latest” without identifying artifact versions
- environment-specific snowflakes (manual fixes that only exist in prod)
- untested rollback paths

This module’s labs force you to practice safe change + rollback locally before you touch a pipeline.
