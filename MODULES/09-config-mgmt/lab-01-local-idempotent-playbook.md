---
title: "Lab 01: Local Idempotent Playbook (localhost)"
tags:
  - lab
  - ansible
  - idempotency
module: "09"
---

# Lab 01 — Local Idempotent Playbook (localhost)

## Goal

Create an idempotent playbook that:

- configures a directory and a config file via template
- runs cleanly multiple times (second run shows no changes)
- uses handlers for “reload” behavior (simulated)

This lab runs against localhost (no SSH required).

## Prereqs

- Ansible installed: `ansible --version`

## Setup

Create a lab directory:

```bash
mkdir -p ~/work/devops-labs/mod09-ansible-local/ansible
cd ~/work/devops-labs/mod09-ansible-local/ansible
```

Create inventory:

```bash
cat > inventory.ini <<'EOF'
[local]
localhost ansible_connection=local
EOF
```

Create variables:

```bash
mkdir -p group_vars
cat > group_vars/all.yml <<'EOF'
app_dir: "{{ ansible_env.HOME }}/work/devops-labs/mod09-ansible-local/app"
app_port: 8080
EOF
```

Create template:

```bash
mkdir -p templates
cat > templates/app.conf.j2 <<'EOF'
port={{ app_port }}
managed_by=ansible
EOF
```

Create playbook:

```bash
cat > site.yml <<'EOF'
- name: Configure local app config
  hosts: local
  gather_facts: true
  tasks:
    - name: Ensure app directory exists
      file:
        path: "{{ app_dir }}"
        state: directory
        mode: "0755"

    - name: Render config
      template:
        src: app.conf.j2
        dest: "{{ app_dir }}/app.conf"
        mode: "0644"
      notify: reload app

  handlers:
    - name: reload app
      debug:
        msg: "reload simulated"
EOF
```

## Steps

### 1) Run the Playbook

```bash
ansible-playbook -i inventory.ini site.yml
```

Expected signals:

- tasks report “changed” on first run

### 2) Prove Idempotency

Run again:

```bash
ansible-playbook -i inventory.ini site.yml
```

Expected:

- tasks report “ok” and no changes
- handler not triggered

### 3) Use Check Mode and Diff

```bash
ansible-playbook -i inventory.ini site.yml --check --diff
```

Expected:

- no changes needed

## Verify

### Verify File Content and Idempotency

```bash
cat ~/work/devops-labs/mod09-ansible-local/app/app.conf
ansible-playbook -i inventory.ini site.yml | grep -E 'changed=0\\b' || true
ansible-playbook -i inventory.ini site.yml --check --diff
```

Expected signals:

- config file contains `managed_by=ansible`
- a subsequent run reports no changes
- check mode indicates no drift

## Cleanup

```bash
rm -rf ~/work/devops-labs/mod09-ansible-local
```

## Troubleshooting

### Symptom: localhost connection fails

Fix:

- ensure inventory uses `ansible_connection=local`

### Symptom: template always shows changes

Fix:

- ensure template renders deterministically (stable ordering, no timestamps)

## Why This Matters in Production

- Idempotency reduces risk: you can rerun automation safely during incidents.
- Templates + handlers reduce noise and unnecessary restarts.

## Definition of Done

- second run reports no changes
- check mode reports no drift
