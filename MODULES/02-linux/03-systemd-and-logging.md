---
title: systemd and Logging (journald)
tags:
  - linux
  - systemd
  - logging
module: "02"
---

# systemd and Logging (journald)

## Why systemd Matters

In many Linux environments, systemd is:

- the service supervisor
- the place you inspect service state and restarts
- the gateway to logs (journald)

Even when you later use Kubernetes, the same mental model applies: supervisors restart things; you must understand why.

## Core systemd Commands

Service status:

```bash
systemctl status <service>
systemctl is-enabled <service> || true
systemctl show <service> | head
```

Start/stop/restart:

```bash
systemctl start <service>
systemctl stop <service>
systemctl restart <service>
```

Unit inspection:

```bash
systemctl cat <service>
systemctl show -p ExecStart,User,Group,Restart,RestartSec <service>
```

## journald Basics

Recent logs:

```bash
journalctl -u <service> --since "15 min ago" --no-pager
```

Follow logs:

```bash
journalctl -u <service> -f
```

Boot scoping (useful after host restarts):

```bash
journalctl -u <service> -b --no-pager
```

## Common systemd Failure Patterns

- ExecStart path wrong or binary missing
- service user lacks permissions
- environment variables not set
- working directory wrong
- restart loop (Restart=always + immediate failure)

Operator habit:

- do not just restart; read status and recent logs first
- inspect the unit file to understand user, environment, paths

## Timeouts, KillMode, and “Graceful Shutdown”

Important behaviors:

- systemd sends SIGTERM then SIGKILL after timeout
- if your app needs time to exit cleanly, adjust timeout carefully

Tradeoff:

- too short: corrupt state, incomplete flush, bad shutdown
- too long: slow failover and extended impact

## Anti-Patterns

- copying unit files without understanding paths and users
- hiding failures by suppressing logs
- using restart loops as a substitute for fixing root cause
