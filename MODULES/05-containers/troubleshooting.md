---
title: Troubleshooting Guide
tags:
  - troubleshooting
  - containers
module: "05"
---

# Troubleshooting — Module 05 Containers

## Fast Triage Checklist

1. Is the container running or exited?
2. If exited: what is the exit code and last logs?
3. Is the app listening on the expected port inside the container?
4. Is host port mapping correct?
5. Are mounts correct and writable?
6. Is DNS working inside container?
7. Is it resource constrained (memory/cpu)?

## Core Commands

State:

```bash
docker ps -a
docker inspect <container>
```

Logs:

```bash
docker logs --tail 200 <container>
```

Exec:

```bash
docker exec -it <container> sh
```

Ports:

```bash
docker port <container> || true
ss -tulpn
```

Resources:

```bash
docker stats --no-stream
```

## Symptom Patterns

### Container Exits Immediately

Likely:

- wrong CMD/ENTRYPOINT
- missing binary/file
- permission denied

Fix approach:

- inspect logs and exit code
- verify file paths inside image

### Service Unreachable

Likely:

- app not listening
- host port mapping wrong
- app bound to localhost inside container

Fix approach:

- check host mapping (`-p host:container`)
- check listener inside container
- ensure app binds to 0.0.0.0

### Permission Denied on Volume

Likely:

- non-root user cannot write to mounted path

Fix approach:

- correct host directory permissions/ownership for lab
- document required writable paths; avoid broad chmod

### Suspected OOM / Memory Pressure

Likely:

- memory limit too low
- memory leak

Fix approach:

- confirm exit code patterns and observe `docker stats`
- raise limit with evidence, then add telemetry and follow-up tasks
