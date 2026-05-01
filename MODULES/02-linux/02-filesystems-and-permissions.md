---
title: Filesystems, Permissions, and “It Works as Root”
tags:
  - linux
  - filesystem
  - permissions
module: "02"
---

# Filesystems, Permissions, and “It Works as Root”

## Why Operators Care

Many outages are basic:

- config file not readable
- data directory not writable
- log directory missing
- disk full
- wrong ownership after deploy

These failures often look like “app bug” until you check permissions and disk.

## Permissions Model (Practical)

- user (u), group (g), others (o)
- read (r), write (w), execute (x)

Commands:

```bash
ls -la
stat <file>
id
groups
```

Interpretation:

- “execute” on a directory controls traversal
- you can read a file but still fail because you can’t traverse the directory

## Ownership and Service Users

Production services rarely run as root:

- reduced blast radius
- fewer privilege escalation paths

Common failure pattern:

- service runs as `svc-user`
- files owned by root after manual fix or deploy
- service fails on restart

Operator habit:

- confirm service user
- confirm ownership and permissions of config/data/log paths

## Filesystems and Disk Pressure

Disk failures are catastrophic and noisy:

- logs can fill disk
- container images can fill disk
- tmp dirs can fill disk

Commands:

```bash
df -h
du -sh * 2>/dev/null | sort -h | tail -n 20
```

## Common Permission Failures

- cannot bind low ports (<1024) without privilege
- cannot write PID files to `/var/run` if directory permissions wrong
- cannot read TLS certs if permissions too strict
- cannot write logs if log directory not writable

## Anti-Patterns

- “chmod 777” to make it work
- running everything as root
- storing runtime state in a read-only filesystem without a plan
- ignoring disk alerts until the system is already broken
