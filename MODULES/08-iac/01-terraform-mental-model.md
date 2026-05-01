---
title: Terraform Mental Model
tags:
  - terraform
  - iac
  - mental-model
module: "08"
---

# Terraform Mental Model

## Terraform Is a State Reconciler

Terraform compares:

- desired state (your configuration)
- current known state (terraform state file)

Then produces:

- a plan (diff)
- an apply (actions to reconcile)

Key implication:

- the state file is a critical operational asset, not a cache.

## The Core Loop

1) `terraform init` (downloads providers, configures backend)  
2) `terraform fmt` and `terraform validate` (hygiene)  
3) `terraform plan` (review diff)  
4) `terraform apply` (execute change)  
5) `terraform destroy` (cleanup)  

## Dependency Graph

Terraform builds a dependency graph from:

- explicit references (resource A uses output of resource B)
- implicit dependencies (depends_on)

Operational habit:

- when you see “forces replacement”, understand which dependency causes it.

## Providers

Providers map Terraform resources to real systems.

Common failure patterns:

- auth/permissions issues
- API rate limiting
- breaking provider version changes

Safety habit:

- pin provider versions and commit lockfile `.terraform.lock.hcl`.

## Outputs Are Operational Interfaces

Outputs are how you:

- expose resource IDs/addresses for other systems
- create a “contract” between modules/environments

## Anti-Patterns

- applying without reading plan
- committing state files to git
- using broad, unpinned provider version ranges
