---
title: Systems Thinking for Operators
tags:
  - foundations
  - systems-thinking
module: "01"
---

# Systems Thinking for Operators

## The Operator’s Job

You do not “run code”. You operate a system that includes:

- software (services, jobs, schedulers)
- infrastructure (compute, network, storage)
- identity (who/what can do what)
- delivery mechanisms (builds, deployments, rollbacks)
- signals (metrics, logs, traces, events)
- humans and processes (on-call, change control, communication)

When the system fails, your job is to restore service safely and prevent recurrence.

## Model Every Service as a Graph

Start with a dependency graph:

- upstreams: clients, gateways, CDNs, proxies
- core service: application, worker, scheduler
- downstreams: databases, caches, queues, third-party APIs
- platform: runtime, orchestrator, IAM, network policies
- observability: metrics/logs/alerts/dashboards

### What to Write (Always)

- Invariants: what must always be true (e.g., “DB migrations are backward compatible”, “every request carries a trace ID”).
- Failure domains: what can fail independently (node, AZ, region, dependency).
- Capacity assumptions: expected QPS, concurrency, storage growth, connection limits.
- Operational interfaces: health endpoints, admin endpoints, runbooks, dashboards.

## Signals Over Stories

Under incident pressure, stories are seductive:

- “This looks like a network issue”
- “It’s probably the last deploy”

Replace stories with signals:

- error rates, latency, saturation (RED)
- queue depth / consumer lag
- DB connections, slow queries
- node/pod restarts, OOM kills
- timeouts and retry storms

## The “Two-Layer” Debug Habit

Most outages are multi-layer:

- an application symptom with an infrastructure trigger
- a CI failure that is actually a supply-chain/input problem
- a Kubernetes issue that is actually DNS, image registry auth, or resource pressure

Habit:

1) establish ground truth at the edge (client symptom)
2) work inward (proxy → service → dependencies → platform)

## Anti-Pattern: “Fix First, Understand Later”

If you fix by random changes:

- you increase blast radius
- you lose evidence
- you create non-reproducible outcomes

Instead:

- capture initial state
- write hypotheses
- take minimal reversible actions

Use the core framework: [Troubleshooting Framework](../../00-HOW-TO-USE/05-troubleshooting-framework.md)
