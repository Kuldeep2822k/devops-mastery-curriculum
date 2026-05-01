---
title: "Module 06: Kubernetes"
tags:
  - module
  - kubernetes
  - operations
module: "06"
---

# Module 06 — Kubernetes

## Outcomes

You can:

- Explain core Kubernetes primitives and how they compose into production systems.
- Deploy workloads with correct labels/selectors, health probes, and resource requests/limits.
- Debug outages using an evidence-based playbook: events → describe → logs → exec → network/DNS checks.
- Diagnose scheduling issues (Pending), pull failures, crash loops, probe failures, OOM, and traffic routing failures (Service/Ingress).
- Apply basic RBAC troubleshooting and least privilege thinking.
- Write Kubernetes runbooks and ADRs for workload patterns and operational tradeoffs.

## Prereqs

- Local Kubernetes installed: [Kubernetes Local](../../SETUP/04-kubernetes-local.md)
- Docker basics (images and registries): [Module 05](../05-containers/00-overview.md)

## Module Map

- Concepts:
  - [01-workloads-labels-and-rollouts.md](01-workloads-labels-and-rollouts.md)
  - [02-probes-resources-and-scheduling.md](02-probes-resources-and-scheduling.md)
  - [03-services-ingress-and-networking.md](03-services-ingress-and-networking.md)
  - [04-storage-basics.md](04-storage-basics.md)
- Deep dives:
  - [deep-dive-01.md](deep-dive-01.md)
  - [deep-dive-02.md](deep-dive-02.md)
- Labs:
  - [lab-01-deploy-break-fix.md](lab-01-deploy-break-fix.md)
  - [lab-02-network-dns-debug.md](lab-02-network-dns-debug.md)
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

1. Read concepts (01–04) quickly for mental model.
2. Read deep dives (tradeoffs and edge cases).
3. Complete lab-01 (deploy + break + fix) and lab-02 (network + DNS debug).
4. Run the troubleshooting lab scenarios time-boxed and write incident logs.
5. Complete exam and self-grade rubric.
6. Produce a runbook and an ADR (workload pattern choice or resource policy).
