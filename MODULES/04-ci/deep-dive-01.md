---
title: "Deep Dive 01: CI Threat Model and Supply-Chain Risk"
tags:
  - ci
  - deep-dive
  - security
module: "04"
---

# Deep Dive 01 — CI Threat Model and Supply-Chain Risk

## Why CI Is a High-Value Target

CI often has:

- access to secrets
- the ability to publish artifacts
- the ability to deploy

If CI is compromised, attackers can ship malicious code through your normal process.

## Common CI Threats

- malicious PRs attempting to exfiltrate secrets
- compromised dependencies or build scripts
- runner compromise (shared runners, untrusted code execution)
- cache poisoning (untrusted artifacts reused)

## Controls (Practical)

- restrict secrets to trusted contexts (no secrets on PRs from forks)
- use protected environments and approvals
- minimize token scope and rotate
- pin dependencies and base images
- generate SBOM and store provenance metadata

## Policy as Code (Preview)

Later modules will implement:

- policy checks for IaC
- supply-chain gates (SBOM, signing)

In CI, start by:

- enforcing lockfile checks
- preventing deployments from untrusted branches
- recording source SHA in artifacts
