---
title: Alerting Design (Actionable, Low Noise)
tags:
  - observability
  - alerting
module: "11"
---

# Alerting Design (Actionable, Low Noise)

## An Alert Is a Page to a Human

An alert should mean:

- someone must take action now

If no action is required, it should be:

- a ticket
- a dashboard signal
- a report

## Actionability Checklist

Every alert should answer:

- what is broken?
- what is the impact?
- what changed recently?
- what do I do first?
- where is the runbook?

## Signal Selection

Prefer user-centric signals:

- availability (success rate)
- latency (p95/p99)
- correctness (critical flows)

Avoid paging on:

- raw CPU usage (use symptom-based alerts)
- single host metrics without context

## Windows and Burn Rate Thinking

Noise control:

- use time windows (e.g., 5m, 30m) rather than 1 datapoint
- use multi-window alerts for fast vs slow burns

## Routing and Ownership

If you can’t name the owner, you can’t page reliably.

Operational habit:

- link alert to service owner and escalation path
- include release version and environment

## Anti-Patterns

- paging on symptoms that self-resolve quickly
- pages without runbook links
- pages that don’t include environment/service/version
