---
title: Common Mistakes (Wrong vs Right)
tags:
  - mistakes
  - ci
module: "04"
---

# Common Mistakes — Module 04 CI

## 1) Wrong: CI Is “Someone Else’s Problem”

Wrong pattern:

- treat pipeline failures as random

Right pattern:

- CI is a production-like system with reliability requirements

## 2) Wrong: Secrets Everywhere

Wrong pattern:

- put deploy credentials in global env

Right pattern:

- inject secrets only to specific steps and only in trusted contexts

## 3) Wrong: Cache Without Correctness

Wrong pattern:

- cache keys ignore lockfiles/tool versions

Right pattern:

- cache keys include lockfile hash, OS, language/tool version

## 4) Wrong: Rebuild Artifacts Later

Wrong pattern:

- build in CI, then rebuild in CD

Right pattern:

- build once; promote by digest/immutable identifier; attach provenance metadata

## 5) Wrong: Flaky Tests Hidden by Retries

Wrong pattern:

- rerun until green without tracking

Right pattern:

- measure flake rate, quarantine intentionally, and pay down flake debt
