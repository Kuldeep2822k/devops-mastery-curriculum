---
title: "Module 02: Linux"
tags:
  - module
  - linux
  - operations
module: "02"
---

# Module 02 — Linux

## Outcomes

You can:

- Triage a Linux system quickly (CPU, memory, disk, IO, network, processes).
- Understand and debug process lifecycle, signals, and common failure patterns.
- Use systemd/journald for service operations (start/stop/status/logs, unit inspection).
- Diagnose common “service down” incidents: port binding, permission, config, dependencies, resource pressure.
- Apply safe operational changes: limits, file descriptors, sysctls (scoped and reversible), and document them.

## Prereqs

- Setup completed: [SETUP](../../SETUP/00-overview.md)
- Basic shell familiarity.

## Module Map

- Concepts:
  - [01-processes-and-signals.md](01-processes-and-signals.md)
  - [02-filesystems-and-permissions.md](02-filesystems-and-permissions.md)
  - [03-systemd-and-logging.md](03-systemd-and-logging.md)
  - [04-network-triage-linux.md](04-network-triage-linux.md)
- Deep dives:
  - [deep-dive-01.md](deep-dive-01.md)
  - [deep-dive-02.md](deep-dive-02.md)
- Labs:
  - [lab-01-systemd-service-debug.md](lab-01-systemd-service-debug.md)
  - [lab-02-resource-pressure-drill.md](lab-02-resource-pressure-drill.md)
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

1. Read concepts (01–04).
2. Read deep dives.
3. Do lab-01 and lab-02 (include cleanup and evidence capture).
4. Run troubleshooting scenarios time-boxed.
5. Complete exam tasks and self-grade.
6. Produce one runbook and one ADR.
