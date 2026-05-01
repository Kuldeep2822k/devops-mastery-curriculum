---
title: TLS Basics and Debugging
tags:
  - networking
  - tls
module: "15"
---

# TLS Basics and Debugging

## What TLS Provides

- encryption in transit
- server identity verification (cert chain)
- optional client auth (mTLS)

## Common TLS Failures

- cert expired / not yet valid (clock drift)
- hostname mismatch (SNI and CN/SAN mismatch)
- incomplete chain (missing intermediate)
- protocol/cipher mismatch (legacy clients/servers)

## Operator Debug Tools

- check time: `date`
- inspect cert and handshake:

```bash
openssl s_client -connect example.com:443 -servername example.com </dev/null | head -n 80
```

- curl TLS info:

```bash
curl -v https://example.com/ 2>&1 | head -n 80
```

## SNI

SNI selects the correct certificate on multi-tenant servers. Missing/wrong SNI causes wrong cert and handshake errors.

## Anti-Patterns

- disabling TLS verification to “make it work”
- ignoring time synchronization on hosts
