---
title: Toil and Automation
tags:
  - sre
  - toil
  - automation
module: "12"
---

# Toil and Automation

## What Toil Is

Toil is work that is:

- manual
- repetitive
- automatable
- reactive
- scales with load or fleet size

Toil is not:

- engineering work that improves systems permanently

## Measuring Toil

Track:

- hours/week spent on repetitive ops tasks
- top incident types and repetitive mitigations
- pages per on-call shift

## Toil Reduction Strategy

- stabilize the system (reduce incidents)
- improve tooling (faster diagnosis and safe actions)
- automate repetitive steps (runbooks → scripts → pipelines)

## Automation Safety

Automation should:

- be idempotent
- have limits and blast radius control
- produce evidence
- be auditable

## Anti-Patterns

- automating a broken process without fixing the root cause
- “run this script on prod” without guardrails
