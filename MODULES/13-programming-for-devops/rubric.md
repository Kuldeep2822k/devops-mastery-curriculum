---
title: Rubric (0–4)
tags:
  - rubric
  - programming
module: "13"
---

# Rubric — Module 13 Programming for DevOps

Use global rubric meanings: [Evidence and Rubrics](../../00-HOW-TO-USE/04-evidence-rubrics.md)

## Skill 1: Safe Automation Design

- 0: Unsafe scripts; no limits or dry-run.
- 1: Some safety but inconsistent; unclear failure behavior.
- 2: Safe defaults, bounded actions, clear exit codes.
- 3: Strong idempotency and observability; reproducible execution.
- 4: Builds automation standards and reduces operational risk org-wide.

## Skill 2: API Resilience

- 0: No timeouts; unbounded retries.
- 1: Timeouts exist but retry logic unsafe.
- 2: Retries with backoff; respects rate limits; bounded work.
- 3: Checkpointing and resumability; protects upstreams from storms.
- 4: Designs client libraries and safety budgets across teams.

## Skill 3: Data Handling

- 0: brittle parsing; poor schemas.
- 1: partial structured output.
- 2: reliable JSON parsing/generation; avoids secret exposure.
- 3: schema versioning and validation.
- 4: creates reusable data contracts and tooling.

## Skill 4: Troubleshooting and Writing

- 0: cannot debug tool failures.
- 1: ad hoc debugging.
- 2: evidence-based debugging; runbook usable.
- 3: runbooks reduce MTTR; ADRs capture tradeoffs.
- 4: mentors others; creates reliable operational tooling ecosystem.
