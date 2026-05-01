---
title: Runbook Template (Module 15)
tags:
  - runbook
  - template
  - networking
module: "15"
---

# Runbook Template — Module 15 Networking Protocols

## Title

<Service> — Service Unreachable / TLS Failure / DNS Failure

## Impact

- user impact:
- scope:
- severity:

## Safety and Preconditions

- capture evidence before restarts/rollbacks
- do not disable TLS verification as a fix

## Quick Triage (10 minutes)

1) DNS:

```bash
dig <name> +short || nslookup <name>
cat /etc/resolv.conf
```

2) Connect:

```bash
nc -vz <host> <port> || true
curl -m 3 -v http://<host>:<port>/ 2>&1 | head -n 40 || true
```

3) TLS:

```bash
openssl s_client -connect <host>:443 -servername <host> </dev/null | head -n 80
date
```

4) HTTP:

```bash
curl -m 5 -v https://<host>/ 2>&1 | head -n 80
```

## Diagnosis

- classify: refused vs timeout vs TLS vs DNS
- check proxy layers if present

## Containment

- rollback recent changes
- reduce load/retries
- route traffic away from failing dependency

## Verification

- connectivity restored
- TLS handshake succeeds
- user-facing checks pass over a window

## Prevention

- DNS/TLS monitoring and renewal automation
- timeout and retry policy
