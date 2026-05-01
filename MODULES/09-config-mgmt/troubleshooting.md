---
title: Troubleshooting Guide
tags:
  - troubleshooting
  - ansible
module: "09"
---

# Troubleshooting — Module 09 Config Mgmt (Ansible)

## First Checks

- Confirm inventory and target scope:
  - are you running against the intended hosts?
- Confirm connection method (SSH vs local).
- Confirm variable sources (group_vars/host_vars).

## Core Commands

```bash
ansible --version
ansible-inventory -i inventory.ini --graph
ansible-inventory -i inventory.ini --list | head -n 50
ansible-playbook -i inventory.ini site.yml --check --diff
ansible-playbook -i inventory.ini site.yml -vv
```

## Common Failures

### Host Unreachable

Likely:

- wrong host/IP
- SSH key issues
- firewall

Fix:

- verify SSH access manually
- correct inventory and auth

### Playbook Always Changes

Likely:

- template includes timestamps or unstable ordering
- shell commands not idempotent

Fix:

- make rendering deterministic
- use modules

### Handlers Not Running

Likely:

- task did not report changed
- handler name mismatch

Fix:

- check notify name
- inspect task changed status with `-v`

### Check Mode Misleads

Interpretation:

- some modules cannot fully simulate changes

Fix:

- use check mode as a preview, not as proof; verify after apply
