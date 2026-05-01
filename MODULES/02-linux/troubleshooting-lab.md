---
title: Troubleshooting Lab (Scenarios)
tags:
  - troubleshooting-lab
  - linux
  - oncall
module: "02"
---

# Troubleshooting Lab — Module 02 Linux

## Instructions

- Do 12–20 scenarios.
- Time-box each scenario: 10–20 minutes.
- For each scenario: symptoms, constraints, hints, diagnosis commands, root cause, fix, verification, prevention.

Use the incident log template: [notes template](../../00-HOW-TO-USE/06-notes-template.md)

## Scenarios

### Scenario 01 — Service Not Running

- Symptoms: clients see connection refused; no listener.
- Constraints: no reboot.
- Hints: check supervisor state.
- Diagnosis commands:
  - `ss -tulpn | grep -E ':(9099)\b' || true`
  - `systemctl --user status mod02-demo.service --no-pager || true`
- Root cause: service stopped.
- Fix: start service.
- Verification: `systemctl --user is-active mod02-demo.service` and `nc -vz 127.0.0.1 9099`.
- Prevention: enable unit; add monitoring and a runbook.

### Scenario 02 — Bad ExecStart Path

- Symptoms: restart loop; status shows exit code.
- Constraints: one restart budget after fix.
- Hints: unit file inspection.
- Diagnosis commands:
  - `systemctl --user status mod02-demo.service --no-pager`
  - `systemctl --user cat mod02-demo.service`
  - `journalctl --user -u mod02-demo.service --since "10 min ago" --no-pager | tail -n 100`
- Root cause: ExecStart references missing binary/script.
- Fix: correct ExecStart, daemon-reload, restart.
- Verification: service active and listening.
- Prevention: CI check for path validity; package artifacts consistently.

### Scenario 03 — Wrong Working Directory

- Symptoms: app fails to find relative config file.
- Constraints: no code changes.
- Hints: set WorkingDirectory or use absolute paths.
- Diagnosis commands:
  - inspect unit file and app error logs
- Root cause: missing WorkingDirectory or incorrect relative paths.
- Fix: set WorkingDirectory or change to absolute config path.
- Verification: service starts and logs expected line.
- Prevention: avoid relative paths in production services.

### Scenario 04 — Permission Denied Reading Config

- Symptoms: service fails; logs show permission denied.
- Constraints: must not broaden permissions beyond least privilege.
- Hints: directory traversal and service user.
- Diagnosis commands:
  - `systemctl --user show -p User,Group mod02-demo.service`
  - `ls -la <path>`
  - `stat <file>`
- Root cause: config unreadable by service user, or directory not executable.
- Fix: adjust ownership/permissions narrowly.
- Verification: service starts; logs show success.
- Prevention: define ownership model; provisioning ensures correct perms.

### Scenario 05 — Port Already In Use

- Symptoms: bind error; no listener for service; another process listens.
- Constraints: must identify and stop correct process safely.
- Hints: use `ss` and `ps`.
- Diagnosis commands:
  - `ss -tulpn | grep -E ':(9099)\b' || true`
  - `ps -eo pid,cmd --sort=pid | tail -n 50`
- Root cause: conflicting process bound to port.
- Fix: stop conflicting process or change service port.
- Verification: service listener appears and connectivity succeeds.
- Prevention: reserve ports; add preflight port check.

### Scenario 06 — Connection Refused vs Timeout Confusion

- Symptoms: remote client times out; local client sees refused.
- Constraints: assume firewall/security rules exist.
- Hints: local listener and firewall can differ.
- Diagnosis commands:
  - local: `ss -tulpn`
  - local: `curl -v http://localhost:<port>/`
  - check firewall tooling as available
- Root cause: no listener or firewall rules blocking.
- Fix: correct listener and/or firewall rules.
- Verification: remote connectivity succeeds.
- Prevention: document network policy and ports; add checks.

### Scenario 07 — Disk Full

- Symptoms: service cannot write logs; random failures; package manager fails.
- Constraints: do not delete unknown data.
- Hints: find top consumers.
- Diagnosis commands:
  - `df -h`
  - `du -sh /var/log/* 2>/dev/null | sort -h | tail -n 20 || true`
- Root cause: disk exhaustion (logs, images, dumps).
- Fix: remove safe-to-remove data (rotated logs, lab artifacts); rotate logs.
- Verification: disk usage returns to safe level; service recovers.
- Prevention: log rotation, disk alerts, bounded retention.

### Scenario 08 — CPU Saturation

- Symptoms: latency spikes; system sluggish.
- Constraints: do not restart service.
- Hints: top offenders.
- Diagnosis commands:
  - `uptime`
  - `ps -eo pid,cmd,%cpu --sort=-%cpu | head -n 10`
- Root cause: runaway process consuming CPU.
- Fix: stop/limit offender; reduce load; adjust scheduling.
- Verification: load and latency trend down.
- Prevention: resource limits, alerts, capacity planning.

### Scenario 09 — Memory Pressure / OOM Kill

- Symptoms: service exits unexpectedly; logs stop abruptly.
- Constraints: must prove it was OOM.
- Hints: kernel messages and exit patterns.
- Diagnosis commands:
  - `dmesg | tail -n 200 || true`
  - `journalctl -b --no-pager | tail -n 200 || true`
- Root cause: OOM killer terminated the process.
- Fix: reduce memory usage, increase limits, optimize; restart service after addressing cause.
- Verification: service stays running under similar load.
- Prevention: memory monitoring, limits, leak detection.

### Scenario 10 — Too Many Open Files

- Symptoms: errors like EMFILE; new connections fail.
- Constraints: must avoid “just raise limits” without evidence.
- Hints: check fd counts and limits.
- Diagnosis commands:
  - `ulimit -n`
  - `lsof -p <pid> | wc -l`
- Root cause: fd leak or too-low limit for workload.
- Fix: fix leak if possible; raise limit with documentation and monitoring.
- Verification: error stops; fd usage stable.
- Prevention: alert on fd usage growth; connection pool sanity.

### Scenario 11 — Logging Missing After Restart

- Symptoms: service is active but logs appear missing.
- Constraints: do not assume journald is broken.
- Hints: unit logging and output streams.
- Diagnosis commands:
  - `systemctl --user cat <service>`
  - `journalctl --user -u <service> --since "10 min ago" --no-pager`
- Root cause: app logs to a file or stderr suppressed; misconfigured unit.
- Fix: ensure stdout/stderr captured; document log location.
- Verification: logs visible and contain expected fields.
- Prevention: standard logging contract across services.

### Scenario 12 — Zombie Processes Accumulating

- Symptoms: many defunct processes; PID table pressure.
- Constraints: cannot reboot.
- Hints: parent must reap.
- Diagnosis commands:
  - `ps aux | grep -i defunct | head`
  - `pstree -ap | head -n 50`
- Root cause: parent process not reaping children.
- Fix: restart or fix parent; adjust process handling in app.
- Verification: zombies stop increasing after fix.
- Prevention: code review and tests for process handling; supervision patterns.

### Scenario 13 — DNS Resolution Failure

- Symptoms: service can’t reach dependency by name.
- Constraints: must confirm whether it’s DNS or network.
- Hints: use dig and direct IP if available.
- Diagnosis commands:
  - `dig <name> +short || nslookup <name>`
  - `curl -v https://<dependency>` (if applicable)
- Root cause: DNS misconfig or resolver issues.
- Fix: correct resolv.conf / DNS settings; use correct names.
- Verification: name resolves and dependency reachable.
- Prevention: monitor DNS, cache strategy, fallback behavior.

### Scenario 14 — Time Drift Causes TLS/Auth Failures

- Symptoms: TLS errors; tokens invalid; “not yet valid”/“expired”.
- Constraints: cannot bypass TLS verification.
- Hints: time sync.
- Diagnosis commands:
  - `date`
  - `timedatectl status || true`
- Root cause: clock not synchronized.
- Fix: enable/repair time sync.
- Verification: time sync yes; failures resolve.
- Prevention: monitor time drift; enforce NTP baseline.

## Time-Boxed On-Call Drill (30–60 minutes)

Pick two scenarios that interact (example: disk full + restart loop). Time-box 45 minutes:

- define impact and severity
- capture evidence (status, logs, sockets, df/free/uptime)
- choose containment action
- implement minimal fix
- verify recovery for 10 minutes
- write prevention follow-ups (runbook + guardrail)
