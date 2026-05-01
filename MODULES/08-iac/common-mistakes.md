---
title: Common Mistakes (Wrong vs Right)
tags:
  - mistakes
  - terraform
module: "08"
---

# Common Mistakes — Module 08 IaC (Terraform)

## 1) Wrong: Commit State to Git

Wrong pattern:

- `terraform.tfstate` in repo

Right pattern:

- state excluded and stored in a protected backend when working in teams

## 2) Wrong: Apply Without Reading Plan

Wrong pattern:

- apply large diffs blindly

Right pattern:

- review plan, especially replacements and destroys

## 3) Wrong: Fix Drift by Ignoring It

Wrong pattern:

- `ignore_changes` to silence real drift

Right pattern:

- fix the process causing drift; reconcile safely

## 4) Wrong: Delete State to “Fix” Errors

Wrong pattern:

- delete state and reapply

Right pattern:

- use state tools carefully; restore from backups; understand ownership

## 5) Wrong: Unpinned Providers

Wrong pattern:

- wide provider ranges causing breaking upgrades

Right pattern:

- pin versions and commit `.terraform.lock.hcl`
