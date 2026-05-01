---
title: 'Data + Queue Incidents'
tags:
  - catalog
  - datastores
  - queues
  - streaming
  - incident
---

# Data + Queue Incidents — Troubleshooting Scenarios

Use this for operational incidents involving Postgres/Redis and message queues/streams (RabbitMQ/Kafka-like systems).

## Scenarios

### Scenario 01 — Postgres connection exhaustion

- Symptoms: App errors “too many connections”; latency spikes.
- Diagnosis commands:
  - `psql -c \"select count(*) from pg_stat_activity;\"` (from a safe admin context)
  - `psql -c \"show max_connections;\"`
  - Check pooler metrics if present.
- Root cause: No pooling, leaked connections, spike in clients.
- Fix: Enable pooling, cap concurrency, kill runaway sessions only with care.
- Verify: Active connections drop; error rate drops.
- Prevention: Pooling defaults, alerts on saturation, runbook for connection storms.

### Scenario 02 — Postgres slow queries after deploy

- Symptoms: P95 latency increases; CPU rises.
- Diagnosis commands:
  - `psql -c \"select now(), state, wait_event_type, wait_event, query from pg_stat_activity where state <> 'idle' limit 20;\"`
  - `psql -c \"select * from pg_stat_statements order by total_time desc limit 10;\"` (if enabled)
- Root cause: Missing index, bad query plan, migration lock.
- Fix: Roll back query; add index; tune; avoid risky changes during incident.
- Verify: Latency returns; query times normalize.
- Prevention: Migration playbook, index reviews, canary.

### Scenario 03 — Postgres migration lock blocks production

- Symptoms: Requests hang; lock waits; deploy coincides.
- Diagnosis commands:
  - `psql -c \"select pid, wait_event_type, wait_event, query from pg_stat_activity where wait_event_type is not null;\"`
  - `psql -c \"select * from pg_locks limit 50;\"`
- Root cause: DDL lock during migration.
- Fix: Stop migration safely; rollback; run migration in low-traffic window with safe patterns.
- Verify: Lock clears; traffic recovers.
- Prevention: Online migration patterns; checklist and rubric gates.

### Scenario 04 — Redis eviction causes cache stampede

- Symptoms: Cache misses spike; DB load surges; latency rises.
- Diagnosis commands:
  - `redis-cli INFO memory | sed -n '1,120p'`
  - `redis-cli CONFIG GET maxmemory-policy`
- Root cause: Maxmemory reached; eviction policy not suited to workload.
- Fix: Increase memory, tune policy (with ADR), add request coalescing.
- Verify: Hit rate improves; DB load drops.
- Prevention: Cache sizing, eviction policy guidance, alerts.

### Scenario 05 — Queue consumer lag grows without recovery

- Symptoms: Backlog grows; processing delayed.
- Diagnosis commands:
  - Inspect consumer group lag metrics (system-specific).
  - Check consumer logs for errors/retries.
- Root cause: Downstream dependency slow, consumer crash loop, poison message.
- Fix: Scale consumers cautiously; isolate poison messages; add DLQ handling.
- Verify: Lag decreases; throughput stable.
- Prevention: Idempotency, DLQ, capacity planning, backpressure.

### Scenario 06 — Duplicate deliveries cause side effects

- Symptoms: Double-charged orders, repeated emails.
- Diagnosis commands:
  - Check retry settings; inspect dedupe keys.
  - Review idempotency handling in consumer.
- Root cause: At-least-once delivery without idempotency.
- Fix: Implement idempotency keys; dedupe store; safe replay.
- Verify: Replays are safe; duplicates no longer cause side effects.
- Prevention: Idempotency by design; runbook for reprocessing.
