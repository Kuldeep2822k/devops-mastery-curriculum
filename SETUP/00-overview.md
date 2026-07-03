---
title: Setup Overview
tags:
  - setup
  - local-first
---

# Setup Overview

This section bootstraps a workstation for the entire curriculum. The goal is not “install tools”, but to build a stable, debuggable, repeatable operator workstation that behaves predictably under incident pressure.

> **New to the terminal or Git? Start with the primer first:** [00-prerequisites-primer.md](00-prerequisites-primer.md). It takes you from "what is a terminal" to "I made my first commit." Come back here afterward.

> **You don't need to install everything before you start.** This page lists the *full* toolbelt for all 25 modules, but you should install tools **as each module needs them** — not all at once. Installing Kubernetes, Terraform, and Ansible on day one is a common way to get stuck before your first win. See "What each stage actually needs" below.

## What Each Stage Actually Needs

| You're doing… | You need | Setup docs |
| --- | --- | --- |
| The primer + **Module 01 (Foundations)** | `python3`, `curl`, `git` | [00-prerequisites-primer.md](00-prerequisites-primer.md), [01](01-workstation-baseline.md), [02](02-git-and-github.md) |
| Modules 02–03 (Linux, Git) | core CLI toolbelt | [01](01-workstation-baseline.md), [05](05-cli-toolbelt.md) |
| Modules 04–05 (CI, Containers) | Docker | [03-docker.md](03-docker.md) |
| Module 06 (Kubernetes) | kind/minikube + kubectl | [04-kubernetes-local.md](04-kubernetes-local.md) |
| Module 08 (IaC) | Terraform/OpenTofu | [06-iac-tooling.md](06-iac-tooling.md) |
| Module 09 (Config Mgmt) | Ansible | [07-config-mgmt-tooling.md](07-config-mgmt-tooling.md) |
| Modules 10–11 (Security, Observability) | security/obs tooling | [08](08-security-tooling.md), [09](09-observability-tooling.md) |

Install the rest when you get there.

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

Follow in order (but install each tool only when the modules above say you need it):

0. [00-prerequisites-primer.md](00-prerequisites-primer.md) — terminal, files, and Git basics (skip only if you're already comfortable)
1. [01-workstation-baseline.md](01-workstation-baseline.md) — includes the Windows/WSL2 path
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
