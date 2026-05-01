---
title: "Module 10: Security"
tags:
  - module
  - security
  - devsecops
module: "10"
---

# Module 10 — Security (DevSecOps for Operators)

## Outcomes

You can:

- Apply threat modeling basics to operational systems (not just code).
- Handle secrets safely across local dev, CI, and runtime (no leaks in logs/repos).
- Implement practical security guardrails in delivery pipelines (policy gates).
- Understand supply-chain basics: dependencies, base images, SBOM mindset, provenance intent.
- Debug common security incidents: leaked token, compromised dependency, unsafe permissions, misconfigured network exposure.
- Write runbooks and ADRs for security controls and tradeoffs.

## Prereqs

- Git and CI basics: [Module 03](../03-git/00-overview.md), [Module 04](../04-ci/00-overview.md)
- Containers basics: [Module 05](../05-containers/00-overview.md)

## Module Map

- Concepts:
  - [01-threat-modeling-for-operators.md](01-threat-modeling-for-operators.md)
  - [02-secrets-hygiene.md](02-secrets-hygiene.md)
  - [03-policy-gates-and-least-privilege.md](03-policy-gates-and-least-privilege.md)
  - [04-supply-chain-basics.md](04-supply-chain-basics.md)
- Deep dives:
  - [deep-dive-01.md](deep-dive-01.md)
  - [deep-dive-02.md](deep-dive-02.md)
- Labs:
  - [lab-01-secret-leak-prevention-git-hook.md](lab-01-secret-leak-prevention-git-hook.md)
  - [lab-02-ci-security-gates-manifest.md](lab-02-ci-security-gates-manifest.md)
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
2. Do lab-01 (prevent secret commits with a local guardrail).
3. Do lab-02 (pipeline-style security gates with dependency manifest + policy).
4. Run troubleshooting scenarios time-boxed and write incident logs.
5. Complete exam and self-grade.
6. Produce a runbook and ADR for security controls in your learning stack.
