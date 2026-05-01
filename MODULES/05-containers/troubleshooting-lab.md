---
title: Troubleshooting Lab (Scenarios)
tags:
  - troubleshooting-lab
  - containers
  - oncall
module: "05"
---

# Troubleshooting Lab — Module 05 Containers

## Instructions

- Do 12–20 scenarios.
- Time-box each: 10–20 minutes.
- For each scenario: symptoms, constraints, hints, diagnosis commands, root cause, fix, verification, prevention.

Use incident log template: [notes template](../../00-HOW-TO-USE/06-notes-template.md)

## Scenarios

### Scenario 01 — Container Exits Immediately (Wrong Command)

- Symptoms: container status is Exited; logs show file not found.
- Constraints: do not rebuild image initially; diagnose first.
- Hints: inspect CMD/entrypoint and filesystem.
- Diagnosis commands:
  - `docker ps -a`
  - `docker logs <c>`
  - `docker inspect <c> | head -n 80`
- Root cause: wrong command or missing file.
- Fix: run correct command or rebuild image with correct COPY/CMD.
- Verification: container stays running and health probe passes.
- Prevention: add CI smoke test that runs container and hits /healthz.

### Scenario 02 — Port Mapping Wrong

- Symptoms: container running, but curl to host port fails.
- Constraints: must prove whether mapping or listener is wrong.
- Hints: host:container mapping.
- Diagnosis commands:
  - `docker port <c> || true`
  - `curl -v http://localhost:<host_port>/healthz || true`
  - `docker exec -it <c> sh -lc "ss -tulpn || true"`
- Root cause: mapped to wrong container port or app not listening.
- Fix: correct `-p host:container` mapping; ensure app listens on expected port.
- Verification: curl returns 200.
- Prevention: standardize ports and document them; smoke tests.

### Scenario 03 — App Bound to 127.0.0.1 Inside Container

- Symptoms: container port mapping exists but connections fail.
- Constraints: must confirm bind address.
- Hints: app must bind 0.0.0.0 to be reachable.
- Diagnosis commands:
  - inside container: `ss -tulpn || true`
- Root cause: app binds localhost only.
- Fix: configure app to bind 0.0.0.0.
- Verification: service reachable from host.
- Prevention: CI test that curls from outside container.

### Scenario 04 — Permission Denied Writing to Volume

- Symptoms: container exits; logs show permission denied on /data.
- Constraints: do not use chmod 777.
- Hints: container user vs host path ownership.
- Diagnosis commands:
  - `docker logs <c>`
  - `docker inspect <c> | grep -n '"Mounts"' -n || true`
  - `ls -la <host_dir>`
- Root cause: host dir not writable by container user.
- Fix: adjust ownership/permissions narrowly; document required writable paths.
- Verification: container starts; file created in mounted dir.
- Prevention: define ownership model; run as non-root intentionally.

### Scenario 05 — DNS Failure Inside Container

- Symptoms: app cannot resolve dependency; host resolves fine.
- Constraints: cannot disable DNS checks.
- Hints: container resolv.conf differs.
- Diagnosis commands:
  - `docker exec -it <c> sh -lc "cat /etc/resolv.conf; nslookup example.com || true"`
- Root cause: DNS configuration mismatch or blocked network.
- Fix: correct Docker DNS settings or network configuration.
- Verification: name resolves inside container.
- Prevention: include DNS checks in debug playbooks.

### Scenario 06 — Image Pull Fails (Auth / Not Found)

- Symptoms: `docker pull` fails or runtime cannot pull.
- Constraints: do not paste tokens into terminal output.
- Hints: wrong tag vs auth.
- Diagnosis commands:
  - `docker pull <image:tag>`
  - inspect error message class (401 vs 404)
- Root cause: missing image tag or auth failure.
- Fix: correct tag; configure auth safely; rotate tokens if needed.
- Verification: pull succeeds; image present.
- Prevention: pin digests; avoid :latest; monitor registry creds rotation.

### Scenario 07 — “Works Locally, Fails in Container”

- Symptoms: app runs on host, fails in container with missing binary/library.
- Constraints: must identify dependency assumptions.
- Hints: missing runtime dependencies in image.
- Diagnosis commands:
  - inspect logs
  - exec into container; check file paths and installed tools
- Root cause: missing dependency or incorrect build context.
- Fix: add missing dependency to image; correct COPY paths.
- Verification: container runs and serves health endpoint.
- Prevention: clean-runner builds; container smoke tests in CI.

### Scenario 08 — Disk Pressure from Images/Volumes

- Symptoms: host disk fills; docker commands fail.
- Constraints: do not use global prune blindly.
- Hints: identify biggest consumers.
- Diagnosis commands:
  - `df -h`
  - `docker system df || true`
  - `docker volume ls`
- Root cause: unused images/volumes consuming space.
- Fix: remove lab-tagged resources; prune only when safe and understood.
- Verification: disk usage decreases; docker stable.
- Prevention: periodic cleanup policy; bounded logging.

### Scenario 09 — Logs Missing or Truncated

- Symptoms: container running but logs show nothing helpful.
- Constraints: must not add verbose logs containing secrets.
- Hints: logging configuration and stdout/stderr.
- Diagnosis commands:
  - `docker logs --tail 200 <c>`
  - `docker inspect <c> | head -n 80`
- Root cause: app logs to file or wrong stream; log driver differs.
- Fix: log to stdout/stderr; document log location if file-based.
- Verification: logs show startup and request lines.
- Prevention: define logging contract.

### Scenario 10 — Timeouts and Retry Storm (Conceptual Drill)

- Symptoms: dependency slow; app retries; latency skyrockets.
- Constraints: must choose containment.
- Hints: retries amplify load.
- Diagnosis commands:
  - observe logs for repeated timeouts
  - measure response times with curl loop
- Root cause: retry storm under dependency degradation.
- Fix: reduce retries, add backoff, add circuit breaker/fail-open where appropriate; rollback.
- Verification: latency/error stabilize.
- Prevention: timeouts/backoff policy; load tests.

### Scenario 11 — Wrong Architecture Image

- Symptoms: “exec format error”.
- Constraints: must not “just rebuild everything” without understanding.
- Hints: amd64 vs arm mismatch.
- Diagnosis commands:
  - `docker inspect <image> | grep -i architecture || true`
- Root cause: image built for wrong arch.
- Fix: build multi-arch images or select correct base/build platform.
- Verification: container starts and runs.
- Prevention: CI builds per target arch; explicit platform flags.

### Scenario 12 — Environment Variable Missing

- Symptoms: app crashes due to missing config/env var.
- Constraints: do not hardcode secrets.
- Hints: env vars and default config.
- Diagnosis commands:
  - `docker inspect <c> | grep -n 'Env' -n || true`
  - logs for missing config messages
- Root cause: required env var missing.
- Fix: add env var in run command or config file mount.
- Verification: container starts; health passes.
- Prevention: validate config at startup; document required env vars.

### Scenario 13 — Health Check Green but Service Broken

- Symptoms: health endpoint returns 200 but real requests fail.
- Constraints: must improve health semantics (not just “up”).
- Hints: health should reflect critical dependency readiness.
- Diagnosis commands:
  - probe real endpoint behavior
- Root cause: shallow health check.
- Fix: adjust health check to include key invariants.
- Verification: health reflects failure appropriately.
- Prevention: document health contract; align with orchestrator probes later.

### Scenario 14 — Container Networking Confusion (Multiple Networks)

- Symptoms: container can’t reach another container; host can.
- Constraints: must diagnose network attachments.
- Hints: user-defined networks vs default bridge.
- Diagnosis commands:
  - `docker network ls`
  - `docker inspect <c> | grep -n '"Networks"' -n || true`
- Root cause: containers on different networks.
- Fix: attach to same user-defined network or use correct DNS names.
- Verification: connectivity succeeds.
- Prevention: standardized compose/network patterns.

## Time-Boxed On-Call Drill (30–60 minutes)

Scenario suggestion:

- Combine Scenario 02 (port mapping wrong) + Scenario 04 (volume permission denied).
- Constraints: no “chmod 777”, and you must preserve evidence before restarting.

Deliverables:

- incident timeline (commands + reasons)
- root cause(s)
- fix + verification
- prevention follow-ups (smoke tests, runbook update, ownership policy)
