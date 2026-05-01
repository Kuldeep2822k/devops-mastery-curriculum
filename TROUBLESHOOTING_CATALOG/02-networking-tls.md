---
title: 'Networking + TLS'
tags:
  - catalog
  - networking
  - tls
  - dns
---

# Networking + TLS — Troubleshooting Scenarios

Use this when you see timeouts, connection resets, DNS failures, or TLS handshake/cert problems.

## Scenarios

### Scenario 01 — DNS resolution fails in one environment

- Symptoms: `NXDOMAIN` or timeout; works elsewhere.
- Diagnosis commands:
  - `dig +short <name> || true`
  - `nslookup <name> || true`
  - `cat /etc/resolv.conf`
- Root cause: Wrong resolver, split-horizon mismatch, missing record.
- Fix: Restore resolver config; correct DNS record; add caching resolver.
- Verify: Consistent resolution; dependency reachable.
- Prevention: DNS health checks; environment-specific records documented.

### Scenario 02 — TCP connection timeout to dependency

- Symptoms: Client timeouts; no immediate reset.
- Diagnosis commands:
  - `nc -vz <host> <port> || true`
  - `ss -tan state syn-sent | head`
  - `traceroute <host> || true`
- Root cause: Firewall/SG/NACL change, route issue, dependency down.
- Fix: Restore route/ACL; fail over; reduce retries to avoid stampede.
- Verify: Connection succeeds; latency normalizes.
- Prevention: Change control for network policies; connectivity canaries.

### Scenario 03 — Connection refused after deploy

- Symptoms: Immediate failure; `ECONNREFUSED`.
- Diagnosis commands:
  - `nc -vz <host> <port> || true`
  - `ss -lntup | grep ':<port> ' || true` (server side)
  - Service logs for bind/listen.
- Root cause: Service not listening, wrong port, wrong bind addr.
- Fix: Correct config; ensure service up; rollback bad deploy.
- Verify: Port listens and responds.
- Prevention: Pre-deploy smoke tests; health checks.

### Scenario 04 — Intermittent resets (RST) under load

- Symptoms: Spiky errors; “connection reset by peer”.
- Diagnosis commands:
  - `ss -s`
  - `netstat -s | head -n 120 || true`
  - Check load balancer/proxy logs.
- Root cause: Backend crashes, conntrack exhaustion, proxy timeouts.
- Fix: Increase limits; tune timeouts; fix backend stability.
- Verify: Error rate drops; resets stop.
- Prevention: Capacity planning; SLO-based alerts; load tests.

### Scenario 05 — TLS handshake failure: unknown CA / bad cert

- Symptoms: `x509: certificate signed by unknown authority`.
- Diagnosis commands:
  - `openssl s_client -connect <host>:<port> -servername <sni> </dev/null 2>/dev/null | openssl x509 -noout -issuer -subject -dates || true`
  - `curl -v https://<host> 2>&1 | sed -n '1,80p' || true`
- Root cause: Missing intermediate CA, wrong trust store, wrong cert deployed.
- Fix: Deploy correct chain; update trust store; rollback cert change.
- Verify: Handshake succeeds; client requests succeed.
- Prevention: Cert automation; chain validation in CI.

### Scenario 06 — TLS failure after rotation (notBefore/notAfter)

- Symptoms: “certificate not yet valid” or expired.
- Diagnosis commands:
  - `date -u`
  - `timedatectl status`
  - `openssl s_client ... | openssl x509 -noout -dates || true`
- Root cause: Clock skew, rotated cert with wrong validity.
- Fix: Restore time sync; redeploy cert.
- Verify: Cert validity ok; connections succeed.
- Prevention: NTP alerts; rotation pipelines validate dates.

### Scenario 07 — mTLS: client cert required / handshake alert

- Symptoms: 400/495/handshake failure; server requires client cert.
- Diagnosis commands:
  - Server logs for TLS client auth
  - `openssl s_client -connect ... -cert <client.crt> -key <client.key> ...` (use safe handling; avoid exposing keys)
- Root cause: Missing client cert, wrong SAN, expired client cert.
- Fix: Provision correct client identity; rotate safely.
- Verify: mTLS works; auth errors stop.
- Prevention: Workload identity patterns; runbooks for cert rotation.
