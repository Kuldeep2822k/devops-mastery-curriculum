---
title: Threat Modeling for Operators
tags:
  - security
  - threat-modeling
module: "10"
---

# Threat Modeling for Operators

## Operator Threat Modeling Is Practical

You are not writing a textbook model. You are answering:

- what can go wrong?
- how would we detect it?
- how would we contain it?
- how do we prevent recurrence?

## Start With Assets

Common assets in DevOps systems:

- credentials (tokens, keys, kubeconfigs)
- source code and build pipelines
- artifacts (images, packages)
- production data (databases, logs)
- identity systems (OIDC, RBAC)

## Attack Surfaces (Common)

- CI runners executing untrusted code
- container registries and base images
- exposed admin endpoints
- overly permissive IAM/RBAC
- secrets in logs, configs, or state files

## Threat Modeling Quick Template

For each component:

- trust boundaries:
- entry points:
- privileges:
- data handled:
- likely attacker goals:

Then define controls:

- authentication and authorization
- least privilege
- audit logs and detection signals
- incident response playbook

## Anti-Pattern

- treating security as a one-time checklist instead of operational behavior under pressure
