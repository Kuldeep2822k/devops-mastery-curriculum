---
title: Troubleshooting Guide
tags:
  - troubleshooting
  - linux
module: "02"
---

# Troubleshooting — Module 02 Linux

Use the global process: [Troubleshooting Framework](../../00-HOW-TO-USE/05-troubleshooting-framework.md)

## “Service Down” Fast Path (Host)

1) Confirm impact:

- what fails (connection refused? timeout? errors? latency?)

2) Check process:

```bash
ps -eo pid,ppid,cmd,%cpu,%mem --sort=-%cpu | head -n 15
```

3) Check listener:

```bash
ss -tulpn
```

4) Check service manager state (systemd):

```bash
systemctl status <service> --no-pager || true
journalctl -u <service> --since "15 min ago" --no-pager | tail -n 200 || true
```

5) Check resources:

```bash
uptime
df -h | head -n 20
free -m 2>/dev/null || true
```

## Interpreting Common Symptoms

### Connection Refused

Likely:

- no listener on port
- service down or bound to different address/port
- local firewall rejects

Commands:

- `ss -tulpn`
- `systemctl status <service>`

### Timeout

Likely:

- packet drops / firewall blocks
- dependency hang
- service hung or overloaded

Commands:

- local curl/nc first, then remote checks
- check CPU/mem and logs for timeouts/retries

### Restart Loop

Likely:

- bad ExecStart path
- missing config
- permission failure on startup
- dependency not reachable

Commands:

- `systemctl status <service> --no-pager`
- `journalctl -u <service> -b --no-pager | tail -n 200`
- inspect unit: `systemctl cat <service>`

## Evidence Preservation Checklist

Before restart/rollback:

- last 200 log lines
- current unit file
- socket/listener state
- resource snapshots (df/free/uptime)

Write these into your incident log.
