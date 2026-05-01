---
title: 'Linux Service Down'
tags:
  - catalog
  - linux
  - systemd
  - incident
---

# Linux Service Down — Troubleshooting Scenarios

Use this when a service is down or degraded on a Linux host and you need a fast, safe path from symptoms → diagnosis → mitigation → prevention.

## Triage Loop (Do This Every Time)

1. Define impact (what is broken, for whom, and since when).
2. Confirm whether this is a process issue, dependency issue, or infrastructure issue.
3. Collect evidence before changing anything.
4. Apply the smallest reversible change.
5. Verify recovery with explicit signals.
6. Write the runbook update and follow-ups while the context is fresh.

## Baseline Commands (Copy/Paste Set)

- Identify service and state:
  - `systemctl status <service> --no-pager`
  - `systemctl show <service> -p ActiveState,SubState,ExecMainStatus,MainPID`
- Logs:
  - `journalctl -u <service> -S -30m --no-pager`
  - `journalctl -u <service> -S -30m -o short-iso --no-pager | tail -n 200`
- Process + resources:
  - `ps -eo pid,ppid,cmd,%cpu,%mem,etime --sort=-%cpu | head`
  - `free -h && df -h && df -hi`
  - `ss -lntup | head -n 50`
- Dependency checks (customize):
  - `getent hosts <dep-host> && nc -vz <dep-host> <dep-port>`

## Scenarios

### Scenario 01 — Service is inactive (dead) after deploy

- Symptoms: Service not running; upstream returns connection refused.
- Constraints: No reboot; change must be reversible.
- Hints: Look for `ExecStart` exit codes and recent unit file changes.
- Diagnosis commands:
  - `systemctl status <service> --no-pager`
  - `journalctl -u <service> -S -60m --no-pager | tail -n 200`
  - `systemctl cat <service>`
- Likely root cause: Broken unit file, missing binary, wrong working directory, or config parse failure.
- Fix:
  - Roll back the unit/config change; validate config; restart.
- Verify:
  - `systemctl is-active <service>`
  - application health check (HTTP/CLI) and log shows ready.
- Prevention:
  - CI lint for unit/config, deploy canary, and `ExecStartPre` config test hook.

### Scenario 02 — Service in CrashLoop (restart loop)

- Symptoms: `Active: activating (auto-restart)`; frequent restarts.
- Constraints: Preserve evidence; avoid log spam.
- Hints: Capture exit code and last successful startup time.
- Diagnosis commands:
  - `systemctl show <service> -p NRestarts,ExecMainStatus,ExecMainStartTimestamp,ExecMainExitTimestamp`
  - `journalctl -u <service> -S -30m --no-pager | tail -n 300`
  - `ulimit -n` (if file-descriptor related)
- Likely root cause: Bad config, missing secret mount, dependency not reachable, incompatible runtime flags.
- Fix:
  - Stabilize: set `RestartSec=10` temporarily; revert config; restore dependency connectivity.
- Verify:
  - Restarts stop increasing; sustained healthy probes.
- Prevention:
  - Startup self-checks, dependency health gating, staged rollout.

### Scenario 03 — Service is running but health endpoint fails

- Symptoms: Process exists; liveness/readiness probe fails; 5xx responses.
- Constraints: No restart until evidence collected.
- Hints: This is often config or downstream dependency.
- Diagnosis commands:
  - `curl -fsS http://127.0.0.1:<port>/health || true`
  - `ss -lntup | grep <port> || true`
  - `journalctl -u <service> -S -30m --no-pager | tail -n 200`
- Likely root cause: App stuck during init, wrong bind address, dependency failure.
- Fix:
  - Correct bind address/listen port; remediate downstream; restart only if necessary.
- Verify:
  - Health endpoint OK; error rate drops.
- Prevention:
  - Separate readiness vs liveness; dependency timeouts; clear startup logs.

### Scenario 04 — Port binding failure (address already in use)

- Symptoms: Service exits with bind error; logs mention EADDRINUSE.
- Constraints: Avoid killing unknown processes blindly.
- Diagnosis commands:
  - `ss -lntup | grep ':<port> ' || true`
  - `lsof -iTCP:<port> -sTCP:LISTEN || true`
  - `systemctl status <service> --no-pager`
- Likely root cause: Old process never stopped, duplicate unit, or sidecar uses same port.
- Fix:
  - Stop the conflicting unit/process; fix port allocation; restart service.
- Verify:
  - Port is owned by intended PID; health checks pass.
- Prevention:
  - Port registry conventions; deploy validation; systemd `ExecStop` correctness.

### Scenario 05 — Disk full causes cascading failures

- Symptoms: Writes fail; logs show ENOSPC; service becomes unstable.
- Constraints: Data safety first.
- Diagnosis commands:
  - `df -h && df -hi`
  - `sudo du -xhd1 /var | sort -h | tail -n 20`
  - `journalctl -S -30m --no-pager | grep -i -E 'ENOSPC|no space' | tail -n 50 || true`
- Likely root cause: Unbounded logs, core dumps, or runaway temp files.
- Fix:
  - Free space safely (rotate logs, remove old artifacts); consider `SystemMaxUse` for journald.
- Verify:
  - Disk usage drops; service stabilizes; error rate drops.
- Prevention:
  - Disk alerts, log retention, quotas, and periodic cleanup jobs.

### Scenario 06 — High memory leads to OOM kills

- Symptoms: Service disappears; kernel logs show OOM; restarts repeatedly.
- Constraints: Minimize downtime; avoid memory “fixes” that hide leaks.
- Diagnosis commands:
  - `dmesg -T | tail -n 200 | grep -i -E 'oom|killed process' || true`
  - `journalctl -k -S -60m --no-pager | grep -i oom | tail -n 50 || true`
  - `ps -eo pid,cmd,%mem,rss --sort=-%mem | head`
- Likely root cause: Memory leak, bad cache settings, large batch job.
- Fix:
  - Reduce load; tune limits; restart to recover; open bug with evidence.
- Verify:
  - RSS stable; no more OOM events.
- Prevention:
  - Memory dashboards, leak detection, resource limits, safe defaults.

### Scenario 07 — Time sync issues break TLS/auth

- Symptoms: TLS failures, token validation errors, “not yet valid” certs.
- Constraints: Avoid changing security policies under pressure.
- Diagnosis commands:
  - `timedatectl status`
  - `chronyc tracking || true`
  - `date -u`
- Likely root cause: NTP drift; VM clock issue.
- Fix:
  - Restore time sync; restart affected services if required.
- Verify:
  - TLS handshakes succeed; auth errors disappear.
- Prevention:
  - NTP monitoring and drift alerts.

### Scenario 08 — DNS failures on host (dependency unreachable)

- Symptoms: Cannot resolve dependency hostnames; intermittent timeouts.
- Constraints: Don’t overwrite resolver config without recording current state.
- Diagnosis commands:
  - `resolvectl status || true`
  - `cat /etc/resolv.conf`
  - `dig +short <dep-host> || true`
- Likely root cause: Bad resolv.conf, upstream resolver outage, split-horizon mismatch.
- Fix:
  - Restore resolver; fail over; use IP only as short-term mitigation.
- Verify:
  - DNS resolves; dependency connectivity restored.
- Prevention:
  - Resolver health checks; caching resolver; runbook for DNS failover.

### Scenario 09 — Permissions error after deploy

- Symptoms: Service fails to read config/socket; “permission denied”.
- Constraints: Don’t broaden permissions permanently as a quick fix.
- Diagnosis commands:
  - `namei -l /path/to/file`
  - `ls -l /path/to/file`
  - `systemctl cat <service> | sed -n '1,160p'`
- Likely root cause: Wrong user/group, missing ACL, incorrect SELinux/AppArmor policy.
- Fix:
  - Correct ownership; update unit `User=`/`Group=`; fix policy.
- Verify:
  - Service starts; access works; no permission errors.
- Prevention:
  - Deployment checks for ownership and policies.

### Scenario 10 — Dependency outage (database/cache) causes service to fail start

- Symptoms: Startup fails waiting for DB/Redis; retry loops.
- Constraints: Avoid tight retries that stampede dependency.
- Diagnosis commands:
  - `nc -vz <dep-host> <dep-port>`
  - application logs for connection failures
  - `ss -tan state syn-sent | head`
- Likely root cause: Dependency down, wrong endpoint, networking policy change.
- Fix:
  - Restore dependency; add exponential backoff; use degraded mode if designed.
- Verify:
  - Service reaches ready; error rate returns to baseline.
- Prevention:
  - Dependency health gating; backoff; graceful degradation.

