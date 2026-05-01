---
title: Inventory and Variables (group_vars/host_vars)
tags:
  - ansible
  - inventory
  - variables
module: "09"
---

# Inventory and Variables (group_vars/host_vars)

## Inventory Is Your Fleet Model

Inventory defines:

- what hosts exist
- how to connect
- what groups represent (roles, environments, regions)

Operator habit:

- keep inventory structured and readable; it becomes production-critical data.

## Variable Precedence (High Level)

Variables come from many places:

- inventory vars
- group_vars / host_vars
- play vars
- role defaults

Risk:

- unexpected overrides cause “it worked yesterday” failures

Staff-level practice:

- keep variable sources simple
- document critical variables and defaults

## Recommended Structure

```
ansible/
  inventory/
    hosts.ini
  group_vars/
    all.yml
  playbooks/
    site.yml
```

## Secrets and Variables

Do not store secrets in plain YAML in repos.

Patterns:

- use Ansible Vault (or secret manager integration)
- inject secrets at runtime via CI secret store

## Anti-Patterns

- putting environment-specific logic in many playbooks instead of variables
- “magic” variables with unclear source
- committing secrets
