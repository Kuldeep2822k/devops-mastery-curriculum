---
title: Runtime Ops and Debugging (Docker)
tags:
  - containers
  - debugging
module: "05"
---

# Runtime Ops and Debugging (Docker)

## Inspect the Running State

```bash
docker ps
docker ps -a
docker inspect <container>
```

What you’re looking for:

- exit code and error message
- env vars and command
- mounts and networks
- port bindings

## Logs

```bash
docker logs <container>
docker logs --tail 200 <container>
```

Operator habit:

- capture logs before restart/removal

## Exec and Filesystem

```bash
docker exec -it <container> sh
ls -la
cat /etc/os-release || true
env | sort | head
```

Common failures:

- missing config file
- wrong working directory
- permissions on mounted volume

## Networking Debug

From host:

```bash
curl -v http://localhost:<host_port>/
ss -tulpn | grep -E ':(<host_port>)\b' || true
```

From inside container:

```bash
docker exec -it <container> sh -lc "ss -tulpn || true"
```

Key interpretation:

- host port mapping exists but app is not listening in container → 502/connection errors
- app listens in container but host mapping missing → unreachable from host

## Resource Debug

```bash
docker stats --no-stream
docker inspect <container> | head -n 50
```

Watch for:

- OOM kills (container exits abruptly, sometimes with 137)
- CPU throttling (latency without obvious errors)

## Volumes and Data

```bash
docker volume ls
docker inspect <container> | grep -n '"Mounts"' -n || true
```

Failure mode:

- app cannot write to mounted path due to ownership mismatch
