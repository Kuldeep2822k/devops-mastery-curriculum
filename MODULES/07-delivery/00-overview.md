---
title: "Module 07: Delivery"
tags:
  - module
  - delivery
  - cd
module: "07"
---

# Module 07 — Delivery (CD and Deployments)

## Outcomes

You can:

- Design a delivery pipeline that promotes the same artifact across environments.
- Choose deployment strategies (rolling, blue/green, canary) based on risk and constraints.
- Execute rollouts and rollbacks safely using measurable signals and clear triggers.
- Handle config and schema change coordination (migrations) with rollback safety in mind.
- Debug delivery incidents: wrong version, stuck rollout, unhealthy canary, misrouted traffic, config drift.
- Write runbooks and ADRs that capture delivery tradeoffs and operational responsibilities.

## Prereqs

- Kubernetes basics: [Module 06](../06-kubernetes/00-overview.md)
- CI basics and artifacts: [Module 04](../04-ci/00-overview.md)
- Containers and image identity: [Module 05](../05-containers/00-overview.md)

## Module Map

- Concepts:
  - [01-cd-and-promotion-model.md](01-cd-and-promotion-model.md)
  - [02-deployment-strategies.md](02-deployment-strategies.md)
  - [03-rollback-and-migration-safety.md](03-rollback-and-migration-safety.md)
  - [04-release-gates-and-progressive-delivery.md](04-release-gates-and-progressive-delivery.md)
- Deep dives:
  - [deep-dive-01.md](deep-dive-01.md)
  - [deep-dive-02.md](deep-dive-02.md)
- Labs:
  - [lab-01-rollout-and-rollback-kind.md](lab-01-rollout-and-rollback-kind.md)
  - [lab-02-canary-and-promotion.md](lab-02-canary-and-promotion.md)
- Cloud extension:
  - [cloud-extension-lab.md](cloud-extension-lab.md)
- Assessment and practice:
  - [checklist.md](checklist.md)
  - [rubric.md](rubric.md)
  - [review-questions.md](review-questions.md)
  - [exam.md](exam.md)
  - [common-mistakes.md](common-mistakes.md)
  - [troubleshooting.md](troubleshooting.md)
  - [troubleshooting-lab.md](troubleshooting-lab.md)
- Writing templates:
  - [runbook-template.md](runbook-template.md)
  - [decision-record-template.md](decision-record-template.md)

## Completion Path (Recommended)

1. Read concepts (01–04) and deep dives.
2. Do lab-01 (rollout + rollback).
3. Do lab-02 (canary + promotion and a rollback drill).
4. Run troubleshooting scenarios time-boxed and write incident logs.
5. Complete exam and self-grade.
6. Produce a delivery runbook and an ADR about your chosen strategy.
