---
title: CI Pipeline Architecture (Stages, Gates, and Signals)
tags:
  - ci
  - architecture
module: "04"
---

# CI Pipeline Architecture (Stages, Gates, and Signals)

## CI as a Delivery System Component

CI is not “run tests”. CI is:

- a change validation system
- a packaging system (artifacts)
- a policy enforcement system (security, compliance)
- a signal generator (confidence for deploy)

CI fails become delivery outages.

## Stages and Gates (A Useful Default)

Think in layers:

1) Fast checks (lint, formatting, unit tests)  
2) Build/package (produce artifacts)  
3) Integration checks (heavier, slower)  
4) Security/policy (SAST, dependency scan, SBOM generation)  
5) Release preparation (versioning, changelog, signing)  

Gates:

- required checks before merge
- required approvals before release/deploy

## Artifacts as Contracts

Artifacts are the output of CI:

- compiled binaries
- container images
- SBOM files
- test reports

Operational requirement:

- every downstream step must consume artifacts, not rebuild randomly

## Environment Separation in CI

Different things should happen in different contexts:

- PR validation: no deploy, no prod access, minimal secrets
- main branch: build and produce release candidates
- release tags: sign and publish artifacts

## Signals and Observability of CI

Your pipeline should expose:

- duration by stage
- failure rates and top failure reasons
- cache hit rates
- flaky test rate

Staff-level insight:

- CI is an SLO-bearing system. If CI is unreliable, delivery is unreliable.
