---
title: CI Reliability (Flakes, Nondeterminism, and Pipeline Health)
tags:
  - ci
  - reliability
  - flaky-tests
module: "04"
---

# CI Reliability (Flakes, Nondeterminism, and Pipeline Health)

## CI Reliability Is Production Reliability

If CI is unreliable:

- teams bypass it
- risky changes slip through
- hotfixes slow down during incidents

Treat CI like a service with SLOs.

## Flaky Tests: What They Look Like

Symptoms:

- test fails intermittently without code changes
- rerun passes
- failures cluster around timeouts, ordering, and concurrency

Root causes:

- shared state across tests
- reliance on wall clock time
- network calls and external dependencies
- random ordering and race conditions

## Nondeterminism Beyond Tests

Build nondeterminism can come from:

- unpinned dependencies
- OS package updates
- timezone/locale differences
- floating “latest” images

Controls:

- lockfiles
- pinned tool versions
- hermetic builds where possible

## Timeouts, Retries, and Quarantines

Retries can hide real issues:

- retry only when you can classify failures as transient

Quarantine is a tool, not a strategy:

- quarantined tests still represent risk
- track and pay down quarantine debt

Timeout design:

- too short: false failures
- too long: slow feedback and wasted resources

## Pipeline Health Metrics (Minimal Set)

- success rate per pipeline and per stage
- mean and p95 duration per stage
- top failure reasons
- flaky test rate (rerun pass rate)
- cache hit rates

## Anti-Patterns

- rerun everything until it passes without tracking flake rate
- disabling tests during incidents without follow-up
- using “sleep” to fix race conditions
- accepting lockfile drift and unpinned dependencies
