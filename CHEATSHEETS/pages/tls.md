---
title: 'TLS'
tags:
  - cheatsheet
  - tls
  - security
---

# TLS Cheatsheet (Debugging)

## Inspect Certificate Presented by Server

- `openssl s_client -connect <host>:443 -servername <sni> </dev/null 2>/dev/null | openssl x509 -noout -subject -issuer -dates`

## Verify Chain (Local)

- `openssl verify -CAfile <ca.pem> <server-cert.pem>`

## Quick curl Debug

- `curl -v https://<host> 2>&1 | sed -n '1,120p'`

## Common Failure Messages → Likely Cause

- `certificate signed by unknown authority`: missing CA/intermediate, wrong trust store.
- `certificate has expired`: expired cert or clock skew.
- `not yet valid`: clock skew or bad rotation timing.
- `handshake failure`: protocol/cipher mismatch, mTLS required, SNI mismatch.

## Safety Notes

- Don’t paste private keys into terminals or tickets.
- Prefer keyless/workload identity patterns for production systems where feasible.

