---
title: "Module 05: Containers"
tags:
  - module
  - containers
  - docker
module: "05"
---

# Module 05 — Containers

## Outcomes

You can:

- Build container images reproducibly (no “works on my machine” drift).
- Understand image layers, tags vs digests, and why promotion by digest matters.
- Operate containers: logs, exec, inspect, networking, volumes, resource limits.
- Debug common container runtime failures quickly: image pull issues, permissions, missing files, env/config errors, OOM, port binding, DNS.
- Apply secure defaults: least privilege, no secrets baked into images, minimized base images, SBOM mindset.

## Prereqs

- Docker installed and working: [Docker setup](../../SETUP/03-docker.md)
- Basic Linux process and networking triage: [Module 02](../02-linux/00-overview.md)

## Module Map

- Concepts:
  - [01-container-mental-model.md](01-container-mental-model.md)
  - [02-images-layers-tags-digests.md](02-images-layers-tags-digests.md)
  - [03-runtime-ops-debugging.md](03-runtime-ops-debugging.md)
  - [04-secure-container-practices.md](04-secure-container-practices.md)
- Deep dives:
  - [deep-dive-01.md](deep-dive-01.md)
  - [deep-dive-02.md](deep-dive-02.md)
- Labs:
  - [lab-01-build-run-debug-image.md](lab-01-build-run-debug-image.md)
  - [lab-02-break-fix-runtime-failures.md](lab-02-break-fix-runtime-failures.md)
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

1. Read concepts (01–04) to understand image vs container, layers, networking, and security basics.
2. Read deep dives for tradeoffs and production failure patterns.
3. Complete lab-01 (build/run/debug) then lab-02 (failure injection).
4. Run troubleshooting-lab scenarios time-boxed.
5. Complete exam tasks and self-grade.
6. Write a container operations runbook and one ADR (base image choice, user model, tagging policy).
