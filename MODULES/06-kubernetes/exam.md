---
title: Practical Exam
tags:
  - exam
  - kubernetes
module: "06"
---

# Practical Exam — Module 06 Kubernetes

## Rules

- Time-box: 120 minutes.
- No internet.
- Submit evidence: commands + expected signals + reasoning.

## Tasks

### Task 1: Deploy a Service Correctly

Requirements:

- Create a namespace.
- Deploy a web workload with:
  - correct labels and selectors
  - readiness and liveness probes
  - resource requests and limits
- Create a Service and verify endpoints exist.

Grading criteria:

- rollout succeeds
- endpoints not empty
- probes configured sensibly

### Task 2: Debug Drill (Pick 3 Failures)

Choose any three:

- ImagePullBackOff
- CrashLoopBackOff
- Pending
- OOMKilled
- readiness probe failures
- Service has no endpoints / 503
- DNS failures
- RBAC denies

For each:

- create the failure (safe, reversible)
- diagnose using events/describe/logs/exec
- fix and verify

Grading criteria:

- evidence-based diagnosis (events and object state)
- minimal reversible fixes
- verification commands included

### Task 3: Runbook + ADR

Requirements:

- Runbook: “Service 503” including:
  - ingress/service/endpoints/probe checks
  - containment actions (rollback) and verification
- ADR: choose a resource and probe policy for a service and justify tradeoffs.

Grading criteria:

- runbook is executable and includes expected signals
- ADR includes constraints, options, decision, risks, mitigations
