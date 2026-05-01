---
title: '06-ansible-fleet: Steps'
tags:
  - project
---

# 06-ansible-fleet — Steps

## Prereqs

- Ansible installed

## Setup

```bash
mkdir -p ~/work/devops-labs/06-ansible-fleet
cd ~/work/devops-labs/06-ansible-fleet
mkdir -p hosts/host1 hosts/host2
```

Inventory (local-only; each host maps to a directory):

```bash
cat > inventory.ini <<'EOF'
[all]
host1 ansible_connection=local host_root=hosts/host1
host2 ansible_connection=local host_root=hosts/host2
EOF
```

Playbook:

```bash
cat > site.yml <<'EOF'
- hosts: all
  gather_facts: false
  tasks:
    - name: Write managed config
      copy:
        dest: "{{ host_root }}/app.conf"
        content: |
          managed_by=ansible
          host={{ inventory_hostname }}
EOF
```

## Steps

### 1) Canary Rollout (host1 only)

```bash
ansible-playbook -i inventory.ini site.yml --limit host1
test -f hosts/host1/app.conf
test ! -f hosts/host2/app.conf && echo "ok: host2 untouched"
```

### 2) Simulate Drift + Converge

```bash
printf 'DRIFT\n' > hosts/host1/app.conf
ansible-playbook -i inventory.ini site.yml --limit host1 --check --diff || true
ansible-playbook -i inventory.ini site.yml --limit host1
```

### 3) Fleet Rollout + Idempotency

```bash
ansible-playbook -i inventory.ini site.yml
ansible-playbook -i inventory.ini site.yml | grep -E 'changed=0\\b' || true
```

## Verify

```bash
grep -q '^managed_by=ansible$' hosts/host1/app.conf
grep -q '^managed_by=ansible$' hosts/host2/app.conf
```

## Cleanup

```bash
cd ~
rm -rf ~/work/devops-labs/06-ansible-fleet
```

## Troubleshooting

- If ansible can’t find python, set `ansible_python_interpreter` in inventory.
