---
title: "Lab 01: Sockets and Connectivity Drill (Refused vs Timeout)"
tags:
  - lab
  - networking
  - tcp
module: "15"
---

# Lab 01 — Sockets and Connectivity Drill (Refused vs Timeout)

## Goal

Practice distinguishing:

- connection refused
- timeout
- “service is listening but wrong interface/port”

## Prereqs

- `python3`
- `curl` and/or `nc`

## Setup

Create a simple local server:

```bash
python3 -m http.server 18030 --bind 127.0.0.1
```

## Steps

### 1) Success Case

```bash
curl -v http://127.0.0.1:18030/ 2>&1 | head -n 20
```

### 2) Connection Refused (No Listener)

Try a port with no service:

```bash
curl -v http://127.0.0.1:19999/ 2>&1 | head -n 20 || true
```

Expected:

- “connection refused” quickly

### 3) Timeout (Simulated)

Timeout simulation depends on your environment and firewall rules. Use a non-routable IP:

```bash
curl -m 2 -v http://10.255.255.1:18030/ 2>&1 | head -n 30 || true
```

Expected:

- timeout after ~2 seconds

### 4) Prove Listener State

```bash
ss -tulpn | grep -E ':(18030|19999)\b' || true
```

## Verify

- you can classify refused vs timeout and explain what it implies.

## Cleanup

- stop server (Ctrl+C)

## Troubleshooting

### Symptom: “timeout” test returns immediately

Explanation:

- some environments route or reject traffic differently; you may see `No route to host` instead of a timeout.

Fix:

- try a different non-routable IP (examples): `10.255.255.1`, `192.0.2.1`, `198.51.100.1`
- ensure you use `curl -m <seconds>` to bound the wait

### Symptom: you can’t tell if anything is listening

Fix:

```bash
ss -tulpn | head
```

## Definition of Done

- you can move from symptom to likely layer (listener vs routing/firewall) using evidence.
