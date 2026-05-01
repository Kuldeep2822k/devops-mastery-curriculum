---
title: Troubleshooting Lab (Scenarios)
tags:
  - troubleshooting-lab
  - observability
  - oncall
module: "11"
---

# Troubleshooting Lab — Module 11 Observability

## Instructions

- Do 12–20 scenarios.
- Time-box each: 10–20 minutes.
- Record: symptoms, constraints, hints, evidence to check, root cause, fix, verification, prevention.

## Scenarios

### Scenario 01 — Alert Fires With No Runbook

- Symptoms: page with no actionable steps.
- Constraints: must fix alert definition, not just respond.
- Evidence to check: alert payload content.
- Root cause: alert not designed for action.
- Fix: add runbook link and first 5 commands.
- Verification: alert payload contains required fields.
- Prevention: alert definition checklist.

### Scenario 02 — High Error Rate After Deploy

- Symptoms: success rate drops after version change.
- Constraints: avoid deploy thrash.
- Evidence to check: deploy marker, error logs, dependency health.
- Root cause: regression or config change.
- Fix: rollback or disable feature; verify recovery.
- Verification: error rate returns to baseline over window.
- Prevention: canary and SLO-based gates.

### Scenario 03 — Latency Spike Without Errors

- Symptoms: p95 latency up, error rate normal.
- Evidence: saturation, traces (slow spans), dependency latency.
- Root cause: downstream slowness or throttling.
- Fix: reduce load, scale, tune timeouts/retries.
- Verification: p95 returns to baseline.
- Prevention: performance tests and retry policy.

### Scenario 04 — Retry Storm

- Symptoms: throughput spikes, latency increases, dependency errors.
- Evidence: logs show repeated retries; traces show repeated spans.
- Root cause: aggressive retries with no backoff.
- Fix: add backoff/circuit breaker; reduce traffic.
- Verification: retries drop; latency normalizes.
- Prevention: retry budget policy.

### Scenario 05 — Metrics Cardinality Explosion

- Symptoms: metrics backend slow/cost spikes.
- Evidence: label/value counts increase rapidly.
- Root cause: high-cardinality label introduced.
- Fix: remove high-cardinality label; move identifiers to logs.
- Verification: cardinality returns to stable baseline.
- Prevention: metric label review gate.

### Scenario 06 — Logs Missing During Incident

- Symptoms: no logs during outage window.
- Evidence: logging config and stdout/stderr capture.
- Root cause: logging pipeline misconfigured or app logging disabled.
- Fix: restore log pipeline; add startup log line and health logs.
- Verification: logs present for new events.
- Prevention: logging SLO and canary checks.

### Scenario 07 — Logs Too Noisy

- Symptoms: cannot find signal in logs.
- Evidence: log volume and fields.
- Root cause: debug logging enabled in prod.
- Fix: lower log level; add structured fields; sample.
- Verification: useful signal-to-noise ratio restored.
- Prevention: log level controls and guardrails.

### Scenario 08 — Alert Storm From One Root Cause

- Symptoms: many alerts fire simultaneously.
- Evidence: dependency outage correlation.
- Root cause: symptom alerts not grouped and not suppressed.
- Fix: define top-level alert; reduce secondary paging.
- Verification: future incident produces fewer pages.
- Prevention: alert dependency mapping.

### Scenario 09 — Dashboard Misleads (Wrong Units)

- Symptoms: dashboard shows ms but data is seconds.
- Evidence: raw metric units.
- Root cause: unit mismatch.
- Fix: standardize units and rename metrics.
- Verification: dashboard matches reality.
- Prevention: metric schema documentation.

### Scenario 10 — “Green” SLO But Users Complain

- Symptoms: SLO green, but user impact exists.
- Evidence: SLI definition vs real user flows.
- Root cause: SLI doesn’t represent user value.
- Fix: redefine SLI to match real success criteria.
- Verification: SLO reflects failures appropriately.
- Prevention: stakeholder review of SLOs.

### Scenario 11 — Version Unknown During Incident

- Symptoms: cannot tell what version is deployed.
- Evidence: deploy metadata missing.
- Root cause: no version markers in logs/metrics.
- Fix: add version field and deploy annotations.
- Verification: dashboards show version over time.
- Prevention: release metadata policy.

### Scenario 12 — Trace Sampling Too Low

- Symptoms: cannot find trace for incident requests.
- Evidence: sampling configuration.
- Root cause: sampling too aggressive.
- Fix: increase sampling temporarily or for error paths.
- Verification: traces available for error requests.
- Prevention: adaptive sampling strategy.

## Time-Boxed On-Call Drill (30–60 minutes)

Scenario: high latency after deploy with intermittent 5xx.

- constraints: preserve evidence before rollback, one change at a time
- deliverables:
  - timeline with signals used
  - containment decision and verification window
  - prevention improvements (alerts, dashboards, runbook)
