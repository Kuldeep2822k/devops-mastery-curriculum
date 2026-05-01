---
title: Config Management Tooling (Ansible)
tags:
  - setup
  - config-mgmt
  - ansible
---

# Config Management Tooling (Ansible)

## Goal

Install and validate configuration management tooling for fleet operations labs: idempotent changes, inventory design, and safe rollouts.

## Prereqs

- Python installed
- SSH client installed

## Install Ansible

Install via your OS package manager or Python tooling. Prefer pinned versions for reproducibility across labs.

Verify:

```bash
ansible --version
ansible-playbook --version
```

Expected signals:

- version output prints without errors

## SSH Baseline

You will use SSH heavily. Safety rules:

- do not reuse passwords across systems
- use SSH keys
- use agent forwarding sparingly and intentionally
- keep keys out of repos and logs

Verify SSH client:

```bash
ssh -V
```

## Local Inventory Strategy (Beginner-Friendly)

For local-first labs, you can treat:

- containers as “hosts” (via SSH into a container running sshd, for practice)
- or VMs as hosts (preferred realism)

Keep a simple inventory:

```
inventory/
  hosts.ini
group_vars/
  all.yml
```

## Verification (Sanity)

Verify Ansible can run local tasks:

```bash
ansible localhost -m ping
```

Expected signals:

- “SUCCESS” with a ping response

## Troubleshooting

### Symptom: “ansible: command not found”

Fix:

- install Ansible properly
- ensure PATH includes the install directory

### Symptom: SSH auth failures

Diagnosis:

- `ssh -vvv user@host`
- verify which key is used and whether agent has it

Fix:

- correct key permissions
- add the right key to the agent
- confirm target host allows key auth

### Symptom: python interpreter issues on remote hosts

Diagnosis:

- check `ansible_python_interpreter`

Fix:

- install Python on the target host
- pin interpreter path in inventory or group_vars

## Why This Matters in Production

- Config drift and manual changes create recurring incidents.
- Idempotent automation reduces human error during urgent fixes.
- Inventory and secrets hygiene are key to safe, scalable operations.
