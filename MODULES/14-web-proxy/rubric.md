---
title: Rubric (0–4)
tags:
  - rubric
  - web-proxy
module: "14"
---

# Rubric — Module 14 Web Proxy

Use global rubric meanings: [Evidence and Rubrics](../../00-HOW-TO-USE/04-evidence-rubrics.md)

## Skill 1: Proxy Configuration and Routing

- 0: Cannot configure routing safely.
- 1: Can configure but breaks easily; no verification discipline.
- 2: Configures routing and headers; verifies with direct vs proxy tests.
- 3: Designs safe rollout/rollback and clear policies.
- 4: Builds platform standards and reduces proxy incidents org-wide.

## Skill 2: Timeouts, Buffering, and Resilience

- 0: No understanding of timeouts; random tuning.
- 1: Some tuning but misses failure patterns.
- 2: Diagnoses 502/504 patterns and tunes safely with evidence.
- 3: Avoids retry storms; selects buffering policies intentionally.
- 4: Designs proxy capacity posture and resilience standards.

## Skill 3: Security Edges

- 0: Unsafe header handling and auth forwarding.
- 1: Partial understanding; misses host/client IP risks.
- 2: Applies safe header policies and least privilege thinking.
- 3: Documents policies and validates them in tests.
- 4: Integrates proxy security posture into platform governance.

## Skill 4: Troubleshooting and Writing

- 0: Cannot debug proxy incidents.
- 1: Debugs ad hoc; no runbooks.
- 2: Evidence-based diagnosis; runbook usable.
- 3: Runbooks reduce MTTR; ADR captures tradeoffs.
- 4: Teaches others and standardizes proxy operations.
