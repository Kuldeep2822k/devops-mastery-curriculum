---
title: 'Container Runtime Failures'
tags:
  - catalog
  - containers
  - docker
  - containerd
  - incident
---

# Container Runtime Failures — Troubleshooting Scenarios

Use this when containers won’t build, won’t start, or behave differently than expected at runtime.

## Fast Triage Checklist

- Confirm whether the failure is at build time, pull time, start time, or steady-state runtime.
- Capture the exact image reference (tag + digest if available).
- Confirm host constraints: disk, memory, cgroup, kernel, and filesystem.

## Baseline Commands

- Docker:
  - `docker version`
  - `docker info`
  - `docker ps -a --no-trunc`
  - `docker logs --tail 200 <container>`
  - `docker inspect <container> | head -n 80`
- Containerd (if applicable):
  - `ctr -n k8s.io images ls | head`
  - `ctr -n k8s.io containers ls | head`
- Host resources:
  - `df -h && df -hi`
  - `free -h`
  - `dmesg -T | tail -n 120`

## Scenarios

### Scenario 01 — Image pull fails (not found / auth / rate limit)

- Symptoms: `pull access denied`, `manifest unknown`, 429 rate limit.
- Constraints: Don’t log credentials; avoid disabling TLS verification.
- Diagnosis commands:
  - `docker pull <image>`
  - `docker login <registry>` (only if policy allows; avoid pasting secrets)
  - `curl -fsSI https://<registry>/v2/ | head`
- Root cause: Wrong image name/tag, missing repo permissions, registry throttling.
- Fix: Correct tag, use a mirror, authenticate via credential helper, add pull-through cache.
- Verify: Pull succeeds; digest matches expected.
- Prevention: Pin digests, set up mirror/caching, monitor registry rate limits.

### Scenario 02 — Container exits immediately (bad entrypoint/cmd)

- Symptoms: Exit code 127/126/1; logs show “no such file”.
- Diagnosis commands:
  - `docker logs <container>`
  - `docker inspect <container> --format '{{json .Config.Entrypoint}} {{json .Config.Cmd}}'`
  - `docker run --rm -it --entrypoint sh <image> -lc 'ls -la && echo ok'`
- Root cause: Wrong entrypoint path, missing executable bit, invalid shebang.
- Fix: Fix Dockerfile, ensure executable exists, use absolute path, add `chmod +x`.
- Verify: Container stays running; health checks succeed.
- Prevention: CI smoke test runs the image.

### Scenario 03 — Container stuck restarting due to OOMKilled

- Symptoms: Restarts; dmesg shows OOM kill; memory spikes.
- Diagnosis commands:
  - `docker inspect <container> --format '{{.State.OOMKilled}} {{.State.ExitCode}}'`
  - `docker stats --no-stream`
  - `dmesg -T | grep -i oom | tail -n 30 || true`
- Root cause: Memory leak, mis-sized limits, cache blowup.
- Fix: Reduce concurrency, add limits, tune app cache, restart to recover.
- Verify: Memory stabilizes; no OOMKilled events.
- Prevention: Set sensible defaults; dashboards and alerts.

### Scenario 04 — Disk pressure breaks builds or startup

- Symptoms: “no space left on device”, failed layer extraction, build cache errors.
- Diagnosis commands:
  - `df -h && df -hi`
  - `docker system df`
  - `docker builder prune -f` (only after confirming policy)
- Root cause: Unbounded images/volumes, build cache accumulation.
- Fix: Prune safely; enforce retention; move Docker root dir if needed.
- Verify: Builds succeed; pulls succeed; free space recovered.
- Prevention: Scheduled cleanup; disk alerts; caching strategy.

### Scenario 05 — Permission denied accessing mounted volume

- Symptoms: App cannot read/write volume; “permission denied”.
- Diagnosis commands:
  - `docker inspect <container> --format '{{json .Mounts}}'`
  - `ls -la <host-path>`
  - `id` (host user), container user via `docker exec -it <container> id`
- Root cause: UID/GID mismatch; rootless constraints; SELinux/AppArmor.
- Fix: Align UID/GID; use named volumes; update policy; avoid `chmod 777`.
- Verify: App can write/read; no permission errors.
- Prevention: Document UID strategy; use init container to chown where appropriate.

### Scenario 06 — Networking: container cannot reach dependency

- Symptoms: Timeouts to DB/cache; DNS failures inside container.
- Diagnosis commands:
  - `docker exec -it <container> sh -lc 'cat /etc/resolv.conf; nslookup <host> || true'`
  - `docker exec -it <container> sh -lc 'nc -vz <host> <port> || true'`
  - `docker network ls && docker network inspect <net> | head -n 80`
- Root cause: Wrong network, DNS config, firewall, dependency down.
- Fix: Attach correct network; fix DNS; validate routes; restore dependency.
- Verify: Connectivity succeeds; app error rate drops.
- Prevention: Network conventions; startup dependency checks with backoff.

### Scenario 07 — CPU throttling causes latency spikes

- Symptoms: High latency but low CPU usage; throttling metrics.
- Diagnosis commands:
  - `docker stats --no-stream`
  - `cat /sys/fs/cgroup/cpu.stat 2>/dev/null || true`
- Root cause: CPU quota too low; noisy neighbor.
- Fix: Adjust CPU limits; isolate workloads; pin CPU if needed.
- Verify: Latency drops; throttling reduces.
- Prevention: Capacity planning; sane defaults; load testing.

### Scenario 08 — Image works locally but fails in CI/CD runner

- Symptoms: Works on dev machine; fails in CI runner or prod nodes.
- Diagnosis commands:
  - Compare `docker version` and base image architecture.
  - `docker buildx imagetools inspect <image> | head -n 120 || true`
- Root cause: Arch mismatch (arm64 vs amd64), missing system deps, kernel feature gap.
- Fix: Build multi-arch; pin base images; include dependencies.
- Verify: Same digest works across environments.
- Prevention: Buildx + CI matrix; runtime compatibility checks.

