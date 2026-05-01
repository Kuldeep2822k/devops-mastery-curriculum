---
title: Troubleshooting Lab (Scenarios)
tags:
  - troubleshooting-lab
  - networking
  - oncall
module: "15"
---

# Troubleshooting Lab — Module 15 Networking Protocols

## Instructions

- Do 12–20 scenarios.
- Time-box each: 10–20 minutes.
- Record: symptom, classification, evidence, fix, verification, prevention.

## Scenarios

### Scenario 01 — Connection Refused

- Symptoms: immediate refused.
- Evidence: ss shows no listener or wrong port.
- Fix: start service or correct port binding.
- Verification: connect succeeds.

### Scenario 02 — Timeout (Firewall/Route)

- Symptoms: connect times out.
- Evidence: DNS resolves, connect fails; traceroute/route hints if available.
- Fix: open firewall or correct routing.
- Verification: connect succeeds.

### Scenario 03 — DNS NXDOMAIN

- Symptoms: name does not resolve.
- Evidence: dig shows NXDOMAIN.
- Fix: correct name or create DNS record.
- Verification: dig resolves to expected IP.

### Scenario 04 — DNS SERVFAIL / Resolver Down

- Symptoms: intermittent resolution failures.
- Evidence: resolver timeouts or SERVFAIL.
- Fix: use redundant resolvers; restore DNS service.
- Verification: resolution stable.

### Scenario 05 — Split-Horizon Surprise

- Symptoms: resolves differently inside vs outside network.
- Evidence: compare resolver results in both contexts.
- Fix: correct internal/external zones; document expected.
- Verification: correct answers per environment.

### Scenario 06 — TLS Hostname Mismatch

- Symptoms: certificate does not match host.
- Evidence: openssl/curl show CN/SAN mismatch.
- Fix: correct certificate or SNI routing.
- Verification: handshake succeeds.

### Scenario 07 — TLS Expired Cert

- Symptoms: cert expired.
- Evidence: openssl shows validity.
- Fix: renew cert; deploy.
- Verification: handshake succeeds.

### Scenario 08 — Time Drift Breaks TLS

- Symptoms: “not yet valid” / “expired” unexpectedly.
- Evidence: date/timedatectl show drift.
- Fix: restore time sync.
- Verification: TLS works.

### Scenario 09 — HTTP Redirect Loop

- Symptoms: curl -L loops.
- Evidence: Location headers bounce.
- Fix: correct scheme/host and proxy headers.
- Verification: single redirect or none.

### Scenario 10 — 401/403 Auth Failure

- Symptoms: unauthorized/forbidden.
- Evidence: request missing auth or wrong identity.
- Fix: correct auth headers/permissions.
- Verification: 200 with correct identity.

### Scenario 11 — 429 Rate Limiting

- Symptoms: too many requests.
- Evidence: Retry-After header.
- Fix: backoff and reduce rate.
- Verification: success rate recovers.

### Scenario 12 — Proxy 502/504

- Symptoms: proxy errors.
- Evidence: direct upstream works vs proxy fails.
- Fix: correct upstream/timeout config.
- Verification: proxy returns 200.

## Time-Boxed On-Call Drill (30–60 minutes)

Scenario: “service unreachable” complaint.

- deliverables:
  - classify failure (DNS vs connect vs TLS vs HTTP)
  - contain if needed (rollback/route change)
  - verification window and prevention tasks
