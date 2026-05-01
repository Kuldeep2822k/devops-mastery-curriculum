---
title: "Lab 02: Drift and Safe Rollouts (--limit, --check, serial)"
tags:
  - lab
  - ansible
  - drift
  - safety
module: "09"
---

# Lab 02 — Drift and Safe Rollouts (--limit, --check, serial)

## Goal

Practice:

- drift detection (file modified outside Ansible)
- check mode as a safety gate
- safe rollout controls (`--limit`, `serial`)

Local-first lab using localhost and two logical host entries.

## Prereqs

- Ansible installed

## Setup

```bash
mkdir -p ~/work/devops-labs/mod09-ansible-rollout/ansible
cd ~/work/devops-labs/mod09-ansible-rollout/ansible
```

Inventory with two logical hosts mapping to localhost:

```bash
cat > inventory.ini <<'EOF'
[fleet]
host1 ansible_host=127.0.0.1 ansible_connection=local
host2 ansible_host=127.0.0.1 ansible_connection=local
EOF
```

Playbook that writes per-host config:

```bash
cat > site.yml <<'EOF'
- name: Configure fleet config safely
  hosts: fleet
  gather_facts: true
  serial: 1
  vars:
    base_dir: "{{ ansible_env.HOME }}/work/devops-labs/mod09-ansible-rollout/out"
  tasks:
    - name: Ensure host config dir exists
      file:
        path: "{{ base_dir }}/{{ inventory_hostname }}"
        state: directory
        mode: "0755"

    - name: Write config file
      copy:
        dest: "{{ base_dir }}/{{ inventory_hostname }}/config.txt"
        content: |
          host={{ inventory_hostname }}
          managed_by=ansible
        mode: "0644"
EOF
```

## Steps

### 1) Apply to Only One Host (Canary)

```bash
ansible-playbook -i inventory.ini site.yml --limit host1
```

### 2) Simulate Drift on host1

```bash
printf "DRIFT\n" >> ~/work/devops-labs/mod09-ansible-rollout/out/host1/config.txt
tail -n 3 ~/work/devops-labs/mod09-ansible-rollout/out/host1/config.txt
```

### 3) Detect Drift in Check Mode

```bash
ansible-playbook -i inventory.ini site.yml --limit host1 --check --diff
```

Expected:

- indicates a change would be made (drift would be corrected)

### 4) Reconcile Drift and Roll Out to Fleet

```bash
ansible-playbook -i inventory.ini site.yml
```

Expected:

- serial execution (one host at a time)
- drift corrected

Verify:

```bash
grep -q '^managed_by=ansible$' ~/work/devops-labs/mod09-ansible-rollout/out/host1/config.txt
grep -q '^managed_by=ansible$' ~/work/devops-labs/mod09-ansible-rollout/out/host2/config.txt
```

## Verify

### Verify Canary, Drift Detection, and Safe Rollout

```bash
cat ~/work/devops-labs/mod09-ansible-rollout/out/host1/config.txt
test ! -f ~/work/devops-labs/mod09-ansible-rollout/out/host2/config.txt && echo "ok: host2 untouched"
ansible-playbook -i inventory.ini site.yml --limit host1 --check --diff
ansible-playbook -i inventory.ini site.yml
grep -q '^managed_by=ansible$' ~/work/devops-labs/mod09-ansible-rollout/out/host1/config.txt
grep -q '^managed_by=ansible$' ~/work/devops-labs/mod09-ansible-rollout/out/host2/config.txt
```

Expected signals:

- canary applies only to host1 initially
- check mode indicates drift would be corrected
- fleet rollout runs serially and converges host configs

## Cleanup

```bash
rm -rf ~/work/devops-labs/mod09-ansible-rollout
```

## Troubleshooting

### Symptom: check mode shows no drift when you expected drift

Diagnosis:

- the module may not detect drift for certain actions

Fix:

- use modules that support check mode and diff well (copy/template/file)

## Why This Matters in Production

- `--limit` and serial rollouts reduce blast radius.
- check mode provides a preview safety gate.
- drift detection is a workflow discipline, not an optional nice-to-have.

## Definition of Done

- you can detect drift via check mode and reconcile safely with a canary-first rollout
