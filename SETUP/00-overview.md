---
title: Setup Overview
tags:
  - setup
  - local-first
---

# Setup Overview

This section bootstraps a workstation for the entire curriculum. The goal is not “install tools”, but to build a stable, debuggable, repeatable operator workstation that behaves predictably under incident pressure.

## Outcomes

- A workstation baseline you can reproduce (versions, configs, sane defaults).
- A CLI toolbelt for triage, networking, and automation.
- Local container and Kubernetes environments suitable for failure injection.
- IaC and config-mgmt tooling with safe defaults and clean state handling.
- Security and observability tooling to support DevSecOps and SRE labs.

## Principles (Production-Grade)

- Pin versions where feasible (avoid “works yesterday, breaks today”).
- Prefer declarative config and reproducible installation.
- Keep your shell history and logs safe (avoid leaking secrets).
- Use isolated lab environments and deterministic cleanup.
- Instrument and observe your local lab like production.

## Required Setup Modules

Follow in order:

1. [01-workstation-baseline.md](01-workstation-baseline.md)
2. [02-git-and-github.md](02-git-and-github.md)
3. [03-docker.md](03-docker.md)
4. [04-kubernetes-local.md](04-kubernetes-local.md)
5. [05-cli-toolbelt.md](05-cli-toolbelt.md)
6. [06-iac-tooling.md](06-iac-tooling.md)
7. [07-config-mgmt-tooling.md](07-config-mgmt-tooling.md)
8. [08-security-tooling.md](08-security-tooling.md)
9. [09-observability-tooling.md](09-observability-tooling.md)

## Suggested Directory Layout

Keep the guide vault separate from your evidence repo:

```
devops-staff-guide/                  (this vault)
my-devops-evidence/                  (your work artifacts)
my-devops-labs/                      (projects you build)
```

## Verification Standard

Each setup doc includes:

- commands to verify installation
- expected signals (version output, cluster status, etc.)
- common failure modes and fixes

Safety rules still apply: [03-lab-safety-cost-control.md](../00-HOW-TO-USE/03-lab-safety-cost-control.md)
