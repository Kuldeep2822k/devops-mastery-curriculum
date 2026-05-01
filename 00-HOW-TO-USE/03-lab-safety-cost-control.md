---
title: Lab Safety and Cost Control
tags:
  - safety
  - security
  - cost
---

# Lab Safety and Cost Control

This vault is intentionally hands-on. You must practice safe defaults like you would in production.

## Non-Negotiables

- Never paste secrets into terminals, files, screenshots, issues, or chat logs.
- Never store credentials in git.
- Prefer ephemeral credentials; rotate aggressively.
- Prefer immutable artifacts; do not “hot patch” running systems as the default.
- Always include a cleanup step and verify deletion.

## Local-First Safety

Local labs still have risk:

- You can delete your own files; avoid running destructive commands outside lab directories.
- Containers can bind privileged ports, mount host paths, and consume disk/memory.
- Kubernetes can create large volumes and runaway workloads.

Safe practices:

- Use a dedicated workspace folder for labs (avoid running in `$HOME` root).
- Prefer `make` targets that scope actions to the repo.
- Use namespaces in Kubernetes; never operate in `default` for complex labs.
- Use resource limits in containers and Kubernetes labs.

## Secrets Handling Rules (Labs)

Allowed patterns:

- read secrets from environment variables without printing them
- use `.env` files excluded by `.gitignore` in your evidence repo
- use local secret stores (OS keychain) if available
- use Kubernetes Secrets for mechanism practice, but treat them as base64 containers, not “encryption”

Disallowed patterns:

- `echo $TOKEN` to screen
- embedding tokens in command history
- committing any `.tfstate`, kubeconfig, or CI logs that may contain secrets

## Cloud Extension Labs: Cost Control Baseline

If you run optional cloud labs:

- Create a dedicated “learning” account/subscription/project.
- Set budgets and alerts before you provision resources.
- Use short-lived credentials and least privilege policies.
- Tag everything with owner, purpose, expiry date, and module name.
- Prefer small instance types and managed free-tier options where safe.
- Delete resources immediately after verification.

Recommended guardrails (cloud-agnostic):

- A hard budget limit and an alert at 50%, 80%, 100%
- A required tag policy: `owner`, `purpose`, `expires_on`, `module`
- A periodic cleanup process: find resources past expiry and delete

## Cleanup Verification Standard

Every cleanup step should include a verification signal, for example:

- `docker ps` shows no lab containers
- `docker volume ls` shows no lab volumes
- `kind get clusters` shows no lab cluster
- `kubectl get ns` shows lab namespace removed
- Terraform: `terraform state list` is empty after destroy and state file removed from evidence repo

## Supply Chain Hygiene (Even Locally)

Treat build outputs like production artifacts:

- pin tool versions where possible
- record inputs: base images, dependencies, commit SHA
- generate an SBOM for at least one project pipeline later in the curriculum

## Failure Injection Safety

Breaking things is required, but do it safely:

- isolate blast radius to a namespace or lab environment
- snapshot configs before breaking
- log the changes you make so you can revert
- prefer reversible faults (timeouts, wrong config) over irreversible data deletion
