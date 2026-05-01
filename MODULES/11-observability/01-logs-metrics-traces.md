---
title: Logs vs Metrics vs Traces
tags:
  - observability
  - logs
  - metrics
  - traces
module: "11"
---

# Logs vs Metrics vs Traces

## Logs

Logs are events:

- good for debugging specific failures
- high cardinality (many unique values)
- often expensive to store and query at scale

Operational best practices:

- structured logs (key=value or JSON)
- include request ID / trace ID
- avoid secrets in logs

## Metrics

Metrics are aggregated numbers over time:

- good for alerting and trend analysis
- stable cardinality is critical
- good for SLOs and dashboards

Common metric types:

- counters (requests_total)
- gauges (queue_depth)
- histograms/summaries (latency distributions)

## Traces

Traces show a request path across components:

- best for understanding latency and dependencies
- correlates work across services

Operator usage:

- identify which span dominates latency
- identify dependency that fails or retries

## Choosing the Right Tool

- “Is the service down?” → metrics (availability, error rate), then logs for root cause
- “Why is it slow?” → traces, then logs for the slow component
- “What changed?” → release metadata + logs around deploy + metrics shift

## Anti-Patterns

- alerting on raw logs without aggregation strategy
- high-cardinality labels in metrics (user_id, request_id)
- storing secrets in logs for debugging convenience
