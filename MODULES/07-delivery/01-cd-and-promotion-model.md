---
title: CD and Promotion Model (Build Once, Promote Many)
tags:
  - delivery
  - cd
  - artifacts
module: "07"
---

# CD and Promotion Model (Build Once, Promote Many)

## The Core Rule

Build once, then promote the same artifact across environments.

If you rebuild per environment:

- you lose provenance
- rollbacks are ambiguous
- you create “it worked in staging but prod build differs”

## Promotion Model (Practical)

Minimum model:

- PR: validate (tests, lint, security baseline)
- main: build and create a release candidate artifact
- release/prod: deploy the already-built artifact by immutable identity (digest/version)

Artifact identities you can promote:

- container image digest
- package checksum
- commit SHA plus build provenance (weaker than digest but still usable early)

## Environments and Configuration

Environment separation should be mostly configuration:

- endpoints
- feature flags
- credentials (never hardcoded)

Avoid env-specific code branches when possible; they create snowflakes.

## Delivery Signals and Gates

Define gates that are signals, not opinions:

- health and readiness
- error rate and latency
- smoke checks
- rollback triggers

## Anti-Patterns

- deploying “latest”
- manual hotfixes in production without recording artifact identity
- running migrations without a rollback plan
- “green deploy” without verifying user-facing signals
