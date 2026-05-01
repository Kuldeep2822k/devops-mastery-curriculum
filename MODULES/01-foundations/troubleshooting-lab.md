---
title: Troubleshooting Lab (Scenarios)
tags:
  - troubleshooting-lab
  - foundations
  - oncall
module: "01"
---

# Troubleshooting Lab — Module 01 Foundations

## How to Run This Lab

- Choose 12–20 scenarios (or do all).
- Time-box each scenario: 10–20 minutes.
- For each scenario, produce:
  - symptoms
  - constraints
  - diagnosis commands run (and outputs summary)
  - root cause
  - fix
  - verification
  - prevention

Use the incident log template: [notes template](../../00-HOW-TO-USE/06-notes-template.md)

## Scenarios

### Scenario 01 — Port Already In Use

- Symptoms: service fails to start; error indicates bind failure.
- Constraints: you are not allowed to reboot.
- Hints: a previous process is still listening.
- Diagnosis commands:
  - `ss -tulpn | grep -E ':(8080)\b' || true`
  - `ps aux | grep -E 'app.py|python' | head`
- Root cause: another process is bound to the port.
- Fix: stop the process or choose a different port and restart service.
- Verification:
  - `curl -fsS http://localhost:<port>/healthz`
- Prevention: pick unique ports per lab; verify cleanup step; add port check to run script.

### Scenario 02 — Service Running But Health Fails

- Symptoms: `curl -i /healthz` returns 500.
- Constraints: you may restart once.
- Hints: check environment toggles.
- Diagnosis commands:
  - `env | grep -E '^FAIL_HEALTH=' || true`
  - review current terminal environment
- Root cause: failure injection enabled.
- Fix: unset `FAIL_HEALTH` and restart service.
- Verification: health returns 200 with body `ok`.
- Prevention: adopt “config snapshot” in incident logs; print non-sensitive config on startup.

### Scenario 03 — Wrong Port Assumption

- Symptoms: connection refused on 8080.
- Constraints: you cannot edit the code.
- Hints: service may be running on a different port.
- Diagnosis commands:
  - `ss -tulpn | grep -E 'python|:(8080|18080)\b' || true`
  - inspect env `PORT`
- Root cause: PORT set differently than expected.
- Fix: query correct port or restart with intended port.
- Verification: `curl -fsS http://localhost:<port>/healthz`.
- Prevention: standardize port selection and document it.

### Scenario 04 — Latency Regression Without Errors

- Symptoms: HTTP 200 but `time_total` increases significantly.
- Constraints: you may not rollback immediately; must diagnose first.
- Hints: check intentional injection and host resource pressure.
- Diagnosis commands:
  - `env | grep -E '^LATENCY_MS=' || true`
  - `uptime`
  - `free -m || true`
- Root cause: latency injection enabled (or host under load).
- Fix: remove injection and restart; if host pressure, stop heavy processes.
- Verification: latency returns near baseline.
- Prevention: define latency SLO threshold; add regression gate in CI later.

### Scenario 05 — Flaky Health (Intermittent Failures)

- Symptoms: health alternates between 200 and 500 intermittently.
- Constraints: you must capture evidence before restart.
- Hints: intermittent failures often mean state or concurrency issues.
- Diagnosis commands:
  - loop probe: `for i in $(seq 1 50); do curl -s -o /dev/null -w "%{http_code}\n" http://localhost:8080/healthz; done`
  - check recent changes and env toggles
- Root cause: toggles or code changes causing non-determinism (simulate by toggling env between runs).
- Fix: stabilize configuration; remove uncontrolled variability.
- Verification: 50/50 probes all succeed.
- Prevention: add repeated health checks as acceptance criteria.

### Scenario 06 — Logs Are Useless

- Symptoms: logs are missing key fields; cannot correlate events.
- Constraints: must improve logs without adding external libs.
- Hints: consistent JSON and fields help.
- Diagnosis commands:
  - observe current log lines
- Root cause: logging format inconsistent or missing critical fields.
- Fix: add consistent structured fields (service, event, status, path).
- Verification: logs show consistent keys for start/health/request.
- Prevention: define a logging contract and add verification in tests later.

### Scenario 07 — Health Endpoint Returns 404

- Symptoms: `/healthz` returns 404.
- Constraints: no code changes allowed; configuration only.
- Hints: verify you are hitting the correct path.
- Diagnosis commands:
  - `curl -i http://localhost:8080/healthz`
  - `curl -i http://localhost:8080/health`
- Root cause: incorrect endpoint path used by checker.
- Fix: correct the health check path.
- Verification: 200 ok on `/healthz`.
- Prevention: document operational interfaces in runbook and README.

### Scenario 08 — Client Fails, Server Logs Nothing

- Symptoms: curl times out, server logs show nothing.
- Constraints: do not restart; diagnose.
- Hints: request may not reach server.
- Diagnosis commands:
  - `ss -tulpn | grep -E ':(8080)\b' || true`
  - `curl -v http://localhost:8080/healthz`
- Root cause: server not listening or firewall/local networking issues.
- Fix: start server or use correct bind address/port.
- Verification: logs show health event when curl runs.
- Prevention: add startup verification step.

### Scenario 09 — High CPU on Workstation Affects Service

- Symptoms: latency spikes; host feels slow.
- Constraints: cannot stop the service; must reduce load elsewhere.
- Hints: your laptop can be the bottleneck.
- Diagnosis commands:
  - `uptime`
  - `top -b -n 1 | head -n 20 || true`
- Root cause: unrelated process consuming CPU.
- Fix: stop heavy process; reduce Docker/kind resource usage if applicable.
- Verification: latency returns to baseline.
- Prevention: set resource limits; monitor host health during labs.

### Scenario 10 — Disk Full Breaks Everything

- Symptoms: service crashes or tools fail to write; random errors.
- Constraints: must recover without deleting important data.
- Hints: containers/logs/images often fill disk.
- Diagnosis commands:
  - `df -h`
  - `docker system df || true`
- Root cause: disk exhaustion from logs/images or other files.
- Fix: delete unused images/volumes; clean lab artifacts carefully.
- Verification: sufficient disk free and service runs.
- Prevention: periodic cleanup; avoid verbose logging; cap Docker disk usage.

### Scenario 11 — Time Skew Causes TLS/Auth Weirdness (Theory + Local Check)

- Symptoms: “random” auth/TLS failures in other tools; inconsistent behavior.
- Constraints: cannot bypass TLS verification.
- Hints: time sync matters.
- Diagnosis commands:
  - `date`
  - `timedatectl status || true`
- Root cause: system clock not synchronized.
- Fix: enable time sync and correct clock.
- Verification: time sync enabled; failures disappear.
- Prevention: baseline workstation checks; alert on time drift in real systems.

### Scenario 12 — “Rollback Didn’t Work”

- Symptoms: after rollback, latency still bad or health still failing.
- Constraints: you must prove what version/config is actually running.
- Hints: rollback must restore the running configuration, not just the intent.
- Diagnosis commands:
  - check env vars: `env | grep -E '^(LATENCY_MS|FAIL_HEALTH)=' || true`
  - confirm process restarted after rollback
- Root cause: rollback action incomplete (service not restarted, env persists).
- Fix: fully restart with known-good config; verify.
- Verification: baseline probes succeed again.
- Prevention: make rollback steps explicit and include verification in runbook.

### Scenario 13 — “I Fixed It” But Errors Return

- Symptoms: temporary recovery, then failure returns.
- Constraints: must identify what makes it recur.
- Hints: recurring failures often indicate unresolved root cause or automation.
- Diagnosis commands:
  - run repeated probes over time
  - inspect changes and environment toggles
- Root cause: the trigger condition still exists (env flag, host pressure).
- Fix: remove trigger; add guardrails.
- Verification: sustained success over a time window (e.g., 5 minutes).
- Prevention: add a canary check; document recurrence triggers.

### Scenario 14 — Confusing Signal: 200 OK but Wrong Behavior

- Symptoms: health returns 200 but service is effectively broken (simulated).
- Constraints: must define a better health check.
- Hints: liveness vs readiness vs “real dependency health”.
- Diagnosis commands:
  - probe additional behavior (e.g., another endpoint or dependency simulation)
- Root cause: health check too shallow.
- Fix: redefine health semantics (local: include simple internal invariants).
- Verification: health reflects real failure conditions.
- Prevention: document health definition and keep it stable across versions.

## Time-Boxed On-Call Drill (30–60 minutes)

- Pick any 2 scenarios that combine into a confusing incident (e.g., latency regression + host CPU pressure).
- Start a timer (45 minutes).
- Follow a strict incident process:
  - define impact and severity
  - capture baseline signals
  - choose containment action
  - diagnose root cause with hypotheses
  - execute minimal fix
  - verify over a short window
  - write prevention follow-ups

Deliverables:

- an incident log with timestamps and reasons
- a short postmortem summary (root cause, contributing factors, actions)
