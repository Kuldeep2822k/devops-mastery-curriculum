---
title: Artifacts, Caching, and Parallelism
tags:
  - ci
  - artifacts
  - caching
  - parallelism
module: "04"
---

# Artifacts, Caching, and Parallelism

## Artifacts: What to Save and Why

Artifacts should be:

- reproducible (built from known inputs)
- immutable (identified by digest or version)
- promotable (same artifact moves through environments)

Common artifacts:

- build outputs (dist/, binaries)
- container images (by digest)
- test reports and coverage
- SBOM and provenance metadata

Anti-pattern:

- rebuilding in CD with slightly different inputs

## Caching: Speed vs Correctness

Caches improve speed but can break correctness.

Cache categories:

- dependency cache (e.g., package manager caches)
- build cache (compiler outputs)
- test cache (dangerous unless deterministic)

Cache key design:

- include lockfile hash
- include tool version
- include OS/arch

Failure modes:

- stale cache produces missing files
- cache poisoned by untrusted inputs
- cache key too broad (cross-branch contamination)

## Parallelism and Fan-Out

Parallelism helps when:

- tasks are independent
- setup is expensive but shared

Patterns:

- matrix builds (OS versions, language versions)
- split test suites by shard

Tradeoffs:

- too much parallelism increases cost and can saturate CI runners
- debugging parallel failures can be harder without good log structure

## Fail-Fast vs Collect-All

Fail-fast:

- reduces wasted compute
- speeds feedback

Collect-all:

- useful when failures are cheap and you want a full report (lint + tests + security)

Staff-level choice:

- fail-fast early for obvious failures
- collect-all later for richer insights
