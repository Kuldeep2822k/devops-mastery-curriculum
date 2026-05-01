---
title: Troubleshooting Lab (Scenarios)
tags:
  - troubleshooting-lab
  - web-proxy
  - oncall
module: "14"
---

# Troubleshooting Lab — Module 14 Web Proxy

## Instructions

- Do 12–20 scenarios.
- Time-box each: 10–20 minutes.
- Record: symptoms, constraints, hints, diagnosis steps, fix, verification, prevention.

## Scenarios

### Scenario 01 — 502 After Deploy

- Symptoms: proxy returns 502; upstream changed.
- Diagnosis: direct upstream curl; proxy logs; upstream port/listener.
- Fix: correct upstream address/port; rollback deploy if needed.
- Verification: 200 via proxy and stable.

### Scenario 02 — 504 Under Load

- Symptoms: timeouts increase at peak.
- Diagnosis: upstream latency vs proxy timeout; saturation.
- Fix: tune timeouts, reduce retries, add capacity.
- Verification: timeout rate drops.

### Scenario 03 — 503 No Healthy Upstreams

- Symptoms: proxy indicates no upstreams.
- Diagnosis: health checks failing; endpoints empty.
- Fix: restore health; fix selector/routing.
- Verification: upstream pool healthy.

### Scenario 04 — Wrong Host Routing

- Symptoms: wrong backend receives traffic.
- Diagnosis: host/path rules.
- Fix: correct routing rules.
- Verification: correct backend selected.

### Scenario 05 — Client IP Lost

- Symptoms: upstream sees proxy IP for all requests.
- Diagnosis: forwarded headers and trust config.
- Fix: set/parse X-Forwarded-For safely.
- Verification: upstream logs show client IP.

### Scenario 06 — Websocket Upgrade Fails

- Symptoms: websocket disconnects through proxy.
- Diagnosis: upgrade headers and HTTP version settings.
- Fix: enable upgrade/connection headers.
- Verification: websocket stable.

### Scenario 07 — Header Injection Risk

- Symptoms: upstream redirect or auth confusion.
- Diagnosis: Host header trust; allowed hosts policy.
- Fix: validate hosts; normalize headers.
- Verification: spoofed host rejected.

### Scenario 08 — Buffering Breaks Streaming

- Symptoms: streaming endpoint stalls.
- Diagnosis: buffering enabled.
- Fix: disable buffering for that location.
- Verification: streaming works.

### Scenario 09 — Connection Reuse Issues

- Symptoms: sporadic upstream resets.
- Diagnosis: keepalive settings and upstream behavior.
- Fix: tune keepalive; check upstream limits.
- Verification: error rate drops.

### Scenario 10 — Proxy Saturation

- Symptoms: proxy CPU high; latency spikes.
- Diagnosis: connections, buffers, logs.
- Fix: scale proxy; reduce buffering; tune timeouts.
- Verification: latency normalizes.

### Scenario 11 — TLS Termination Misconfig (Concept)

- Symptoms: handshake failures.
- Diagnosis: cert mismatch, protocols, SNI.
- Fix: correct cert and host mappings.
- Verification: handshake succeeds.

### Scenario 12 — Retry Storm Amplified by Proxy

- Symptoms: upstream collapse under retries.
- Diagnosis: proxy retry policy.
- Fix: reduce retries; require idempotency.
- Verification: upstream recovers.

## Time-Boxed On-Call Drill (30–60 minutes)

Scenario: 502/504 after deploy.

- deliverables:
  - isolate layer (proxy vs upstream)
  - containment (rollback or routing change)
  - verification window
  - prevention actions (timeout policy, runbook)
