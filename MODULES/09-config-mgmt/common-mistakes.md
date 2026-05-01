---
title: Common Mistakes (Wrong vs Right)
tags:
  - mistakes
  - ansible
module: "09"
---

# Common Mistakes — Module 09 Config Mgmt (Ansible)

## 1) Wrong: Run Against All Hosts

Wrong pattern:

- `ansible-playbook ...` with no limits on risky changes

Right pattern:

- canary with `--limit`
- expand rollout in batches/serial

## 2) Wrong: Shell Everywhere

Wrong pattern:

- use `shell` for file and service operations

Right pattern:

- use modules for idempotency and structured results

## 3) Wrong: Restart Every Time

Wrong pattern:

- restart service on every run

Right pattern:

- handlers triggered only on change

## 4) Wrong: Secrets in Repo

Wrong pattern:

- secrets in group_vars YAML committed to git

Right pattern:

- vault or secret manager integration; inject at runtime

## 5) Wrong: No Verification

Wrong pattern:

- playbook green means done

Right pattern:

- verify service health and logs after changes
