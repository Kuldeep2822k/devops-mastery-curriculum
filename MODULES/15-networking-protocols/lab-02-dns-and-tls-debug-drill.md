---
title: "Lab 02: DNS and TLS Debug Drill"
tags:
  - lab
  - networking
  - dns
  - tls
module: "15"
---

# Lab 02 — DNS and TLS Debug Drill

## Goal

Practice a repeatable flow to debug:

- DNS resolution problems
- TLS handshake and cert issues

## Prereqs

- `dig` or `nslookup`
- `openssl`
- `curl`

## Setup

Pick a hostname to test (example used below: `example.com`):

```bash
host=example.com
echo "$host"
```

## Steps

### 1) DNS Resolution

Pick a public hostname (example.com) and run:

```bash
dig example.com +short || nslookup example.com
cat /etc/resolv.conf
```

Expected:

- you get one or more IPs

### 2) Distinguish DNS Failure vs Connect Failure

Resolve first, then connect:

```bash
ip="$(dig example.com +short | head -n 1)"
echo "$ip"
curl -m 3 -v "http://$ip/" 2>&1 | head -n 40 || true
```

Interpretation:

- if resolve fails, it’s DNS
- if resolve works but connect fails, it’s routing/firewall/service

### 3) TLS Handshake and Cert Inspection

```bash
openssl s_client -connect example.com:443 -servername example.com </dev/null | head -n 120
```

Then:

```bash
curl -m 5 -v https://example.com/ 2>&1 | head -n 80
```

Expected:

- handshake succeeds and certificate matches hostname

### 4) Time Skew Check

```bash
date
timedatectl status 2>/dev/null || true
```

## Verify

- you can classify DNS vs connect vs TLS failure modes and list the first 5 commands for each.

## Cleanup

- none

## Troubleshooting

### Symptom: dig is not installed

Fix:

- use `nslookup` instead

### Symptom: openssl s_client output is confusing

Fix:

- focus on: `Verify return code`, certificate subject/issuer, and negotiated protocol/cipher
- compare with `curl -v` output to see where the handshake fails

## Definition of Done

- you can debug a DNS/TLS issue without disabling verification or guessing.
