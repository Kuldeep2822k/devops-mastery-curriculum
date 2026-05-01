---
title: Observability Tooling
tags:
  - setup
  - observability
  - sre
---

# Observability Tooling

## Goal

Install and validate local observability tooling so you can treat telemetry as part of every lab: metrics, logs, and tracing concepts.

## Principles

- Observability is a product: reliable, documented, and actionable.
- Start simple: metrics + logs before distributed tracing.
- Prefer repeatable setups that you can tear down and rebuild.

## Local Stack (Recommended)

- Prometheus (metrics collection)
- Grafana (dashboards and visualization)

Optional later:

- Loki (logs)
- Tempo/Jaeger (traces)

## Local-First Approach

You will run the observability stack in one of two ways:

1. Docker Compose (simpler early)
2. Kubernetes (kind/minikube) later for realistic cluster operations

## Verification Goals

By the end of this setup section, you should be able to:

- run Prometheus and Grafana locally
- reach Grafana UI
- confirm Prometheus is scraping at least one target
- create one dashboard panel from a real metric

## Minimal Verification Commands (CLI-Level)

You will implement full labs in Module 11 Observability. For now verify you have the basic CLI/network tools:

```bash
curl --version
```

If you already have a local stack running:

```bash
curl -fsS http://localhost:9090/-/ready
curl -fsS http://localhost:3000/api/health
```

Expected signals:

- Prometheus ready endpoint returns success
- Grafana health endpoint returns JSON with “ok”

## Troubleshooting

### Symptom: Grafana/Prometheus ports already in use

Diagnosis:

- `ss -tulpn | grep -E ':(3000|9090)\b' || true`

Fix:

- stop conflicting services
- or run on different ports and document it in your lab notes

### Symptom: dashboards show “No data”

Diagnosis:

- verify Prometheus is scraping: check targets endpoint in UI or query `up`
- confirm correct time range

Fix:

- add a scrape target
- fix labels and job names used in queries

## Why This Matters in Production

- Without telemetry you operate by guesswork and increase MTTR.
- Poorly designed dashboards and alerts create noise and burnout.
- Staff-level engineers treat observability as a first-class feature.
