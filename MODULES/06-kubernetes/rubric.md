---
title: Rubric (0–4)
tags:
  - rubric
  - kubernetes
module: "06"
---

# Rubric — Module 06 Kubernetes

Use global rubric meanings: [Evidence and Rubrics](../../00-HOW-TO-USE/04-evidence-rubrics.md)

## Skill 1: Workload and Rollout Operations

- 0: Cannot deploy and reason about controllers.
- 1: Can apply YAML but struggles to interpret rollouts and events.
- 2: Can deploy, observe rollout status, and recover common failures.
- 3: Designs safe rollouts with probes and clear rollback triggers.
- 4: Improves platform defaults and reduces org MTTR via runbooks and guardrails.

## Skill 2: Debugging Playbook (Events/Describe/Logs/Exec)

- 0: Deletes pods blindly; no evidence.
- 1: Uses some commands but misses critical signals (endpoints/events).
- 2: Uses playbook to find root cause and verify fixes.
- 3: Handles multi-layer incidents (network/DNS/RBAC/resources) under time pressure.
- 4: Teaches others and builds standardized debug procedures.

## Skill 3: Networking and DNS

- 0: Cannot distinguish service vs ingress vs pod issues.
- 1: Understands basics but struggles with selector/endpoints debugging.
- 2: Diagnoses endpoints and DNS issues using in-cluster probes.
- 3: Builds reliable runbooks and avoids common routing anti-patterns.
- 4: Designs traffic and DNS strategy with clear failure domain thinking.

## Skill 4: Resource Management and Scheduling

- 0: No resource model; unstable workloads.
- 1: Sets limits without evidence; frequent Pending/OOM issues.
- 2: Sets requests/limits reasonably and diagnoses Pending/OOM/probe failures.
- 3: Tunes resources using measurements; avoids performance cliffs.
- 4: Creates org-wide standards for requests/limits and capacity planning.
