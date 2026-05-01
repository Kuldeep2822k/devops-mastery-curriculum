---
title: Troubleshooting Guide
tags:
  - troubleshooting
  - terraform
module: "08"
---

# Troubleshooting — Module 08 IaC (Terraform)

## Fast Diagnosis Checklist

- Are you in the correct environment/state?
- Did provider versions change unexpectedly?
- Is state corrupted or out of date?
- Is drift present?
- Is the plan trying to replace/destroy something unexpectedly?

## Core Commands

```bash
terraform version
terraform fmt -check || true
terraform validate
terraform plan
terraform show
terraform state list
terraform state show <addr>
```

## Common Failures

### Init Fails

Likely:

- network/proxy issues
- backend misconfiguration

Fix:

- verify network/proxy
- clear `.terraform/` and retry

### Plan Shows Surprising Replacements

Likely:

- config changed a “ForceNew” attribute
- drift exists

Fix:

- inspect which attribute causes replacement
- split change into smaller steps
- ensure you’re targeting correct env/state

### Apply Fails Midway

Likely:

- permissions or provider errors
- partial apply state (some resources changed)

Fix:

- rerun plan to see current diff
- avoid manual edits; reconcile through Terraform

### State Refactor Needed

Use:

- `terraform state mv` for renames
- avoid `state rm` unless you accept losing management
