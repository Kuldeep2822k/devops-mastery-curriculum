---
title: Processes, Signals, and Failure Modes
tags:
  - linux
  - processes
  - signals
module: "02"
---

# Processes, Signals, and Failure Modes

## Mental Model

A Linux process is:

- an executable + memory + file descriptors + environment + permissions
- scheduled by the kernel
- communicating through files, sockets, pipes, and syscalls

As an operator, you care about:

- is the process running
- is it healthy (doing useful work)
- is it making progress
- is it resource constrained
- is it blocked on IO or dependencies

## PID, PPID, and Process Trees

Key questions:

- who started the process (parent)?
- will it be restarted on failure (supervisor/systemd/orchestrator)?
- are there child processes (workers) that leak or zombie?

Core commands:

```bash
ps aux | head
ps -eo pid,ppid,cmd,%cpu,%mem --sort=-%cpu | head
pstree -ap | head
```

## Signals (Operator-Relevant)

- SIGTERM: graceful shutdown request (default for service managers)
- SIGKILL: immediate kill (last resort; no cleanup)
- SIGHUP: reload config (app-specific; sometimes used)
- SIGINT: interactive interrupt (Ctrl+C)

Safe practice:

- prefer SIGTERM and verify exit
- use SIGKILL only when you have evidence the process is stuck and cannot exit

## Exit Codes and “Crash Loops”

Exit codes matter because supervisors use them:

- 0: success (clean exit)
- non-zero: failure

Crash loops are often caused by:

- missing config/env vars
- permission errors (cannot read a file, cannot bind a port)
- missing dependencies (DB/redis/DNS)
- invalid flags or incompatible binary

## File Descriptors and Sockets

Most production outages eventually touch file descriptors:

- too many open files (EMFILE)
- leaked connections
- stuck sockets

Core commands:

```bash
ulimit -n
lsof -p <pid> | head
ss -tulpn
```

## The Operator’s Default “First 5 Minutes”

1. Confirm impact (what is failing for the user).
2. Confirm the process exists and is the right version/config.
3. Check logs around the failure window.
4. Check ports/sockets and dependency reachability.
5. Check resource pressure (CPU/mem/disk/IO).

## Anti-Patterns

- restarting without capturing evidence
- killing with SIGKILL as the default
- treating “process running” as “service healthy”
- ignoring file descriptor and disk usage growth until it’s too late
