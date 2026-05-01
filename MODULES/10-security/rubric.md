---
title: Rubric (0–4)
tags:
  - rubric
  - security
module: "10"
---

# Rubric — Module 10 Security

Use global rubric meanings: [Evidence and Rubrics](../../00-HOW-TO-USE/04-evidence-rubrics.md)

## Skill 1: Secrets Hygiene

- 0: Secrets in repo/logs; no rotation behavior.
- 1: Avoids obvious leaks but inconsistent; weak environment separation.
- 2: Strong baseline hygiene; responds correctly to leaks (rotate/revoke).
- 3: Implements guardrails and detection; least privilege in CI/runtime.
- 4: Designs org-wide secrets and incident practices.

## Skill 2: Threat Modeling and Risk Thinking

- 0: No model; security is reactive.
- 1: Can list threats but not link to controls and signals.
- 2: Identifies assets and trust boundaries; proposes practical controls.
- 3: Designs controls with detection/containment and tradeoffs.
- 4: Mentors others; security becomes part of system design culture.

## Skill 3: Policy Gates and Supply Chain Hygiene

- 0: No gates; uses latest/unpinned deps.
- 1: Some checks but not enforced; bypassed often.
- 2: Enforces basic gates (secrets, base image pinning, manifests).
- 3: Builds layered gates in CI with clear messages and low false positives.
- 4: Integrates SBOM/provenance/signing in a scalable way.

## Skill 4: Incident Response and Writing

- 0: No runbooks; ad hoc response.
- 1: Partial response but poor evidence preservation.
- 2: Clear containment steps; runbook usable.
- 3: Strong post-incident prevention and documentation.
- 4: Builds repeatable incident playbooks and drills.
