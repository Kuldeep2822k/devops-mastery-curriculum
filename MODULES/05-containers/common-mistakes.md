---
title: Common Mistakes (Wrong vs Right)
tags:
  - mistakes
  - containers
module: "05"
---

# Common Mistakes — Module 05 Containers

## 1) Wrong: Use :latest in Deployments

Wrong pattern:

- deploy `myapp:latest`

Right pattern:

- deploy by digest and record it
- use tags only as labels, not identity

## 2) Wrong: Bake Secrets into Images

Wrong pattern:

- copy credentials into image layers

Right pattern:

- inject secrets at runtime
- ensure logs never print secrets

## 3) Wrong: Debug by Editing Running Container

Wrong pattern:

- exec into container and patch files manually

Right pattern:

- rebuild image and redeploy (immutable infrastructure)

## 4) Wrong: Ignore Volume Permissions

Wrong pattern:

- run as root “to make it work”

Right pattern:

- run as non-root and fix ownership/permissions intentionally
- document writable paths

## 5) Wrong: Restart Without Evidence

Wrong pattern:

- remove container immediately

Right pattern:

- capture logs, inspect config, and exit codes first
