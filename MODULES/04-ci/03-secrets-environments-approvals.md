---
title: Secrets, Environments, and Approvals
tags:
  - ci
  - security
  - secrets
module: "04"
---

# Secrets, Environments, and Approvals

## Baseline Rules

- Never print secrets in logs.
- Never store secrets in repo.
- Use least privilege and separate secrets by environment.
- Treat CI runners as potentially hostile; minimize secret exposure surface.

## Secrets Handling Patterns (Safe Defaults)

Preferred patterns:

- pull secrets at runtime from a secret manager (cloud extension later)
- use CI secret stores with environment separation (dev/stage/prod)
- inject secrets only to steps that require them

Avoid:

- “global env” secrets accessible to every step
- long-lived tokens with broad permissions

## Environment Separation

Define distinct contexts:

- PR/feature branches: no prod secrets, no deploy credentials
- main: build + publish candidate artifacts (still avoid prod deploy secrets if possible)
- release: deploy credentials allowed with approvals and strong auditing

## Approvals and Protected Environments

Approvals exist to prevent:

- accidental deploys
- malicious changes in PRs accessing production credentials

Safe patterns:

- manual approval gate for production deploy stages
- protected branches and required checks
- restricted who can approve

## Provenance Basics (Intro)

Provenance is evidence that:

- an artifact came from a specific source revision
- built with a specific pipeline and inputs

You will deepen this in later security/artifacts modules.

Beginner practice:

- record commit SHA in build outputs
- attach SBOM as an artifact when feasible
