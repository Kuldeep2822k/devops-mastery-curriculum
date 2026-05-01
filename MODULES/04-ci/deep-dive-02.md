---
title: "Deep Dive 02: Designing Portable Pipelines (Local-First)"
tags:
  - ci
  - deep-dive
  - portability
module: "04"
---

# Deep Dive 02 — Designing Portable Pipelines (Local-First)

## Why Portability Matters

Portable pipelines:

- reduce “works on CI only” mysteries
- allow faster debugging locally
- reduce vendor lock-in

## The Build Contract Pattern

Define a contract that works locally and in CI:

- `make lint`
- `make test`
- `make build`
- `make package`
- `make smoke`

CI should call the same targets.

## Local Runner Simulation

You can simulate CI locally by:

- running a “clean environment” container
- mounting the repo
- running the Make targets inside the container

This catches:

- missing tool dependencies
- reliance on host environment
- missing generated files (dist/)

## Artifacts as First-Class Outputs

Portability improves when:

- build output paths are standardized
- CI uploads artifacts that are used later, not rebuilt

## Tradeoffs

More portability can mean:

- extra scripting and standardization
- slower initial setup

But pays off by reducing MTTR during pipeline failures and by enabling migrations across CI providers.
