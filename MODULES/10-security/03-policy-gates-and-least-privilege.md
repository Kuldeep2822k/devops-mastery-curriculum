---
title: Policy Gates and Least Privilege
tags:
  - security
  - policy
  - least-privilege
module: "10"
---

# Policy Gates and Least Privilege

## Least Privilege as an Operational Tool

Least privilege reduces blast radius:

- a compromised token can do less damage
- mistakes affect fewer systems

Operator habit:

- know which identity is performing an action
- audit and restrict permissions regularly

## Policy Gates in Pipelines

Policy gates prevent unsafe changes from reaching production.

Examples:

- block merges if secret scan fails
- block deploy if image uses forbidden base tag `latest`
- require approvals for prod deploy
- enforce lockfile presence for dependencies

## “Shift Left” Without Breaking Delivery

Security gates must be:

- actionable (clear failure reason)
- fast (or staged so slow checks don’t block all feedback)
- correct (low false positives)

## Anti-Patterns

- gates that are bypassed routinely
- gates that fail with unclear messages
- global tokens with admin privileges in CI
