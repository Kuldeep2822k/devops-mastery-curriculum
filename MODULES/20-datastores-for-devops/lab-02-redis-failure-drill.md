---
title: 'Redis Failure Drill'
tags:
  - lab
  - redis
  - reliability
module: "20"
---

# Lab 02 — Redis Failure Drill (Restart + Eviction + Recovery)

## Goal

Practice common Redis incident patterns locally:

- client errors during restart (connection drops)
- cache data loss without persistence
- eviction when maxmemory is too low

This is local-first and intentionally uses simple signals: CLI commands + INFO stats.

## Prereqs

- Docker installed and working: `docker version`
- `python3`

## Setup

```bash
mkdir -p ~/work/devops-labs/mod20-redis
cd ~/work/devops-labs/mod20-redis
```

Start Redis (no persistence):

```bash
docker run -d --name mod20-redis -p 16379:6379 redis:7-alpine
```

Wait until ready:

```bash
until docker exec mod20-redis redis-cli ping >/dev/null 2>&1; do sleep 1; done
```

## Steps

### 1) Baseline Writes and Reads

```bash
docker exec mod20-redis redis-cli set key1 value1
docker exec mod20-redis redis-cli get key1
docker exec mod20-redis redis-cli info stats | grep -E 'keyspace_hits|keyspace_misses'
```

Expected:

- `get` returns `value1`

### 2) Failure: Restart Causes Client Errors + Data Loss (No Persistence)

```bash
docker restart mod20-redis
until docker exec mod20-redis redis-cli ping >/dev/null 2>&1; do sleep 1; done
docker exec mod20-redis redis-cli get key1
```

Expected:

- key is missing (nil), demonstrating why Redis-as-cache differs from Redis-as-datastore

### 3) Recovery: Enable Persistence (AOF) and Prove Survival

Restart Redis with AOF enabled and a volume:

```bash
docker rm -f mod20-redis
docker volume create mod20-redis-data
docker run -d --name mod20-redis -p 16379:6379 -v mod20-redis-data:/data redis:7-alpine redis-server --appendonly yes
until docker exec mod20-redis redis-cli ping >/dev/null 2>&1; do sleep 1; done
```

Write and restart:

```bash
docker exec mod20-redis redis-cli set key2 value2
docker restart mod20-redis
until docker exec mod20-redis redis-cli ping >/dev/null 2>&1; do sleep 1; done
docker exec mod20-redis redis-cli get key2
```

Expected:

- key2 still exists after restart

### 4) Failure: Eviction Under Low Maxmemory

Configure eviction:

```bash
docker exec mod20-redis redis-cli config set maxmemory 5mb
docker exec mod20-redis redis-cli config set maxmemory-policy allkeys-lru
```

Load data:

```bash
python3 - <<'PY' | docker exec -i mod20-redis redis-cli --pipe
payload = "x" * 10240
for i in range(2000):
  k = f"k{i}"
  v = payload
  # RESP for: SET <key> <value>
  print(f"*3\r\n$3\r\nSET\r\n${len(k)}\r\n{k}\r\n${len(v)}\r\n{v}\r\n", end="")
PY
```

Observe eviction:

```bash
docker exec mod20-redis redis-cli info stats | grep -E '^evicted_keys:'
docker exec mod20-redis redis-cli dbsize
```

Expected:

- `evicted_keys` increases (non-zero)
- dbsize is bounded despite attempts to insert many keys

### 5) Recovery: Right-Size and Verify

```bash
docker exec mod20-redis redis-cli config set maxmemory 0
docker exec mod20-redis redis-cli info stats | grep -E '^evicted_keys:'
```

Expected:

- future evictions stop increasing after you remove the limit (for this lab)

## Verify

```bash
docker exec mod20-redis redis-cli ping
docker exec mod20-redis redis-cli get key2
docker exec mod20-redis redis-cli info stats | grep -E '^evicted_keys:'
```

Expected signals:

- ping returns PONG
- key2 returns value2
- eviction counter is visible (and non-zero if you ran the eviction step)

## Cleanup

```bash
docker rm -f mod20-redis 2>/dev/null || true
docker volume rm mod20-redis-data 2>/dev/null || true
cd ~
rm -rf ~/work/devops-labs/mod20-redis
```

## Troubleshooting

### Symptom: evicted_keys stays zero

Fix:

- lower `maxmemory` further and increase inserted keys/payload size
