---
title: IaC Tooling (Terraform)
tags:
  - setup
  - iac
  - terraform
---

# IaC Tooling (Terraform)

## Goal

Install and validate Infrastructure-as-Code tooling used throughout the curriculum, with safe defaults for state handling and reproducibility.

## Prereqs

- Git installed
- A workspace directory for labs

## Tooling Choices (Core Path)

- Terraform: required for MODULES/08-iac and related projects

Cloud CLIs are optional and only needed for cloud extension labs.

## Install Terraform

Install via your OS package manager or official binaries. Prefer pinned versions for reproducibility.

Verify:

```bash
terraform version
```

Expected signals:

- Terraform prints version output

## Baseline Terraform Conventions (Use Everywhere)

### State Hygiene

- Never commit `.tfstate` to git.
- Treat state as sensitive; it can contain secrets or identifiers.
- Use separate state per environment (even locally).

### Directory Layout (Recommended)

```
terraform/
  envs/
    local/
      main.tf
      variables.tf
      outputs.tf
      terraform.tfvars
  modules/
    <module-name>/
```

### Lockfiles and Providers

- Commit `.terraform.lock.hcl` (reproducibility).
- Avoid unbounded provider version ranges.

## Verification (Local-Only)

Create a tiny “hello” config to validate init/plan/apply/destroy flows. This does not create cloud resources.

1. Create a directory:

```bash
mkdir -p ~/work/devops-labs/tf-hello
cd ~/work/devops-labs/tf-hello
```

2. Create a minimal Terraform file using a safe local-only provider in Module 08 later. For now, just validate the CLI tool works:

```bash
terraform -help | head -n 20
```

Expected signals:

- help output prints without errors

## Troubleshooting

### Symptom: “terraform: command not found”

Fix:

- install Terraform properly
- ensure PATH includes the install directory

### Symptom: provider install fails

Diagnosis:

- check proxy/DNS/TLS settings

Fix:

- configure proxy as needed
- retry with a pinned version and a clean `.terraform/` directory

## Why This Matters in Production

- Tooling drift causes broken deploys and risky “just apply again” behavior.
- State mishandling creates outages and security incidents.
- Operators must be able to reason about diff, lifecycle, and rollback safely.
