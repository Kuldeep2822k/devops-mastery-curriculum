---
title: 'Observability + Alerting Failures'
tags:
  - catalog
  - observability
  - alerting
  - sre
  - incident
---

# Observability + Alerting Failures — Troubleshooting Scenarios

Use this when alerts are noisy, missing, incorrect, or dashboards lie during an incident.

## Fast Triage Checklist

- Confirm: is the system broken, or is observability broken?
- Identify the failure mode: ingestion, storage, query, alert evaluation, routing, or notification.
- Preserve evidence: alert payloads, query links, timestamps, rule versions.

## Baseline Commands / Checks

- Prometheus:
  - `curl -fsS http://<prometheus>:9090/-/healthy`
  - `curl -fsS http://<prometheus>:9090/api/v1/targets | head -n 80`
- Alertmanager:
  - `curl -fsS http://<alertmanager>:9093/-/healthy`
  - `curl -fsS http://<alertmanager>:9093/api/v2/status | head -n 80`
- Kubernetes (if used):
  - `kubectl -n monitoring get pods -o wide`
  - `kubectl -n monitoring logs deploy/prometheus --tail=200`

## Scenarios

### Scenario 01 — Alerts firing but service is healthy (false positive)

- Symptoms: Page triggers; users unaffected; SLOs look fine.
- Diagnosis commands:
  - Inspect alert expression and thresholds.
  - Compare raw metrics to burn-rate style alerts.
- Root cause: Bad threshold, missing label filter, scrape gaps causing NaNs.
- Fix: Tighten query; add `for:`; add label scoping; use multi-window burn rate.
- Verify: Alert stops firing under normal conditions but triggers on real failure.
- Prevention: Alert reviews, canary alert rules, change management.

### Scenario 02 — Service is broken but no alert (false negative)

- Symptoms: Outage detected by users; no paging.
- Diagnosis commands:
  - Check scrape targets and recent samples.
  - Validate rule group evaluation errors.
- Root cause: Missing instrumentation, scrape failures, rule disabled, route muted.
- Fix: Restore scraping; re-enable rule; add synthetic checks if needed.
- Verify: Controlled failure triggers alert.
- Prevention: Coverage audits, alert tests, “alerts on alerts” meta-monitoring.

### Scenario 03 — Metrics missing for one cluster/region

- Symptoms: Dashboards show gaps; targets down only for subset.
- Diagnosis commands:
  - Prometheus targets endpoint; DNS/network checks.
  - Compare service discovery across clusters.
- Root cause: Network policy, DNS, certificate, relabeling bug.
- Fix: Restore connectivity; fix relabel config; redeploy scrape config safely.
- Verify: Targets up; samples resume.
- Prevention: Multi-tenant isolation, config validation CI.

### Scenario 04 — Alert storm (cardinality explosion)

- Symptoms: Thousands of alerts; Alertmanager overloaded.
- Diagnosis commands:
  - Identify label causing fan-out (pod, container, request_id).
  - Check recent deploy/config changes.
- Root cause: Rule uses high-cardinality labels; missing aggregation.
- Fix: Aggregate alerts; route to ticket instead of paging; add inhibition rules.
- Verify: Alert volume returns to expected.
- Prevention: Cardinality linting, rule templates, review gates.

### Scenario 05 — Alerts not delivered (routing failure)

- Symptoms: Alert exists in Prometheus/Alertmanager but no notification.
- Diagnosis commands:
  - Alertmanager status; route matchers; silences; inhibition.
  - Notification logs for errors (auth, rate limit).
- Root cause: Routing config mismatch, broken webhook/SMTP/Slack token.
- Fix: Correct routing; restore connector; fail over channel.
- Verify: Test alert delivers.
- Prevention: Synthetic “test alert” schedule, runbook for notification channels.

### Scenario 06 — Dashboard shows wrong time window / misleading aggregation

- Symptoms: Graph looks fine but logs show failure; mismatch by time range.
- Diagnosis commands:
  - Confirm dashboard timezone/range, rate windows, and aggregation labels.
- Root cause: Wrong rate/irate window, aggregation hides subset failure.
- Fix: Add breakdown panels; use percentile/histograms; annotate deploys.
- Verify: Dashboard reflects reality during controlled failure.
- Prevention: Dashboard review, standard panels, shared SLO dashboards.

### Scenario 07 — Prometheus out of disk / WAL corruption risk

- Symptoms: Ingestion stops; logs show TSDB issues; disk full.
- Diagnosis commands:
  - `df -h`; Prometheus logs; TSDB status endpoints.
- Root cause: Retention too long, disk undersized, high cardinality.
- Fix: Increase disk; reduce retention; fix cardinality; restart carefully.
- Verify: Ingestion resumes; queries work.
- Prevention: Capacity planning; alerts on disk/cardinality; retention policy ADR.

### Scenario 08 — Clock skew breaks alert evaluation

- Symptoms: Alerts delayed; data appears “in the future”; TLS issues.
- Diagnosis commands:
  - NTP status on nodes and monitoring stack.
- Root cause: Time sync drift.
- Fix: Restore time sync; restart components if needed.
- Verify: Rules evaluate normally; timestamps sane.
- Prevention: NTP monitoring.

