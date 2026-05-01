---
title: "Lab 02: Break/Fix (Timeouts and Headers)"
tags:
  - lab
  - web-proxy
  - nginx
module: "14"
---

# Lab 02 — Break/Fix (Timeouts and Headers)

## Goal

Practice diagnosing:

- 504 due to timeout
- 502 due to wrong upstream port
- header forwarding mistakes (host, x-forwarded-for)

## Prereqs

- complete Lab 01 or have an equivalent local nginx + upstream setup

## Setup

Create a workspace:

```bash
mkdir -p ~/work/devops-labs/mod14-proxy-breakfix
cd ~/work/devops-labs/mod14-proxy-breakfix
```

If you did Lab 01, copy its files into this directory (or recreate them here):

- upstream.py
- nginx.conf

## Steps

### 1) Break: Wrong Upstream Port (502)

Edit nginx.conf to proxy_pass a wrong port (e.g., 19999), reload nginx, then:

```bash
curl -i http://127.0.0.1:18021/ || true
```

Diagnosis:

- proxy error response
- confirm upstream still works directly:

```bash
curl -i http://127.0.0.1:18020/ || true
```

Fix:

- restore correct upstream port and reload nginx

Verification:

- proxy returns 200 again

### 2) Break: Timeout (504)

Modify upstream to sleep for 2s on `/slow`, and set proxy read timeout to 1s.

Then:

```bash
curl -i http://127.0.0.1:18021/slow || true
```

Diagnosis:

- confirm upstream is slow directly
- confirm proxy timeout config

Fix:

- increase proxy read timeout or fix upstream latency root cause

Verification:

- `/slow` succeeds through proxy with expected latency

### 3) Break: Missing Forwarded Headers

Remove X-Forwarded-For and Host header forwarding.

Then:

```bash
curl -fsS http://127.0.0.1:18021/ | jq . || curl -fsS http://127.0.0.1:18021/
```

Diagnosis:

- upstream response missing expected fields or has incorrect values

Fix:

- restore header forwarding

## Verify

- you can reproduce and recover from 502 and 504 using evidence
- you can explain why headers matter for auth, logging, and rate limiting

## Cleanup

- stop nginx and upstream (Ctrl+C)
- remove directory:

```bash
cd ~
rm -rf ~/work/devops-labs/mod14-proxy-breakfix
```

## Troubleshooting

### Symptom: nginx reload fails

Diagnosis:

```bash
nginx -t -c "$PWD/nginx.conf" -p "$PWD" 2>&1 | head -n 80 || true
```

Fix:

- correct syntax errors and retry

### Symptom: proxy returns 502/504 but upstream seems fine

Diagnosis:

```bash
curl -i http://127.0.0.1:18020/ || true
curl -i http://127.0.0.1:18021/ || true
```

Fix:

- confirm proxy_pass target and timeout directives

## Why This Matters in Production

- most proxy incidents are config mistakes plus unclear debugging discipline.

## Definition of Done

- you can debug 502/504 and header issues without guesswork.
