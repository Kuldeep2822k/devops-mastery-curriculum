---
title: Rubric (0–4)
tags:
  - rubric
  - containers
module: "05"
---

# Rubric — Module 05 Containers

Use global rubric meanings: [Evidence and Rubrics](../../00-HOW-TO-USE/04-evidence-rubrics.md)

## Skill 1: Image Build and Reproducibility

- 0: Builds ad hoc; cannot reproduce reliably.
- 1: Can build but relies on local environment quirks; poor Dockerfile structure.
- 2: Can build consistently; understands layers and caching.
- 3: Uses digest-based identity and clear artifact/version recording.
- 4: Designs build pipelines with provenance mindset and strong guardrails.

## Skill 2: Runtime Operations and Debugging

- 0: Cannot inspect logs/state; guesses fixes.
- 1: Can use basic commands but struggles with root cause.
- 2: Can diagnose common failures (ports, command, env, mounts) and verify fixes.
- 3: Can debug under constraints; uses minimal reversible changes and preserves evidence.
- 4: Builds runbooks/tooling that reduce MTTR across teams.

## Skill 3: Resource and Performance Awareness

- 0: No awareness of limits or host impact.
- 1: Knows about limits but cannot interpret symptoms.
- 2: Can detect and respond to OOM/CPU issues using evidence.
- 3: Can tune limits with telemetry and avoid cascading failure patterns.
- 4: Anticipates performance cliffs; creates standards and monitoring.

## Skill 4: Security and Supply-Chain Hygiene

- 0: Runs as root; uses latest tags; secrets in images/logs.
- 1: Some best practices but inconsistent.
- 2: Uses non-root, avoids secrets, understands tag/digest risks.
- 3: Implements a debug strategy and documents tradeoffs.
- 4: Integrates security posture into pipeline and operational standards.
