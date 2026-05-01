---
title: Runbook Template (Module 07)
tags:
  - runbook
  - template
  - delivery
module: "07"
---

# Runbook Template — Module 07 Delivery

## Title

Delivery — <Symptom (bad deploy / 503 after rollout / latency regression)>

## Impact

- User impact:
- Scope (service, namespace, environment):
- Severity:

## Safety and Preconditions

- Preserve evidence before rollback.
- No deploy thrash: one change at a time.
- Do not print secrets.

## Quick Triage (10 minutes)

- What changed most recently?
- What artifact identity is deployed now?

Commands:

```bash
kubectl -n <ns> rollout status deploy/<name>
kubectl -n <ns> rollout history deploy/<name>
kubectl -n <ns> get deploy/<name> -o jsonpath='{.spec.template.spec.containers[*].image}{"\n"}'
kubectl -n <ns> get pods,svc,ep -o wide
kubectl -n <ns> get events --sort-by=.lastTimestamp | tail -n 80
```

## Diagnosis

- Check endpoints and readiness (503 patterns).
- Check logs and events for crash loops and probe failures.
- Check config references (ConfigMaps/env) for wrong environment.

## Containment

Preferred containment for regressions:

- rollback to known good artifact identity
- disable feature flag (if available)
- reduce blast radius (scale down canary)

Rollback:

```bash
kubectl -n <ns> rollout undo deploy/<name>
```

## Verification

- endpoints populated
- rollout stable
- user-facing checks pass
- error/latency signals improved over a window

## Prevention / Follow-Ups

- add smoke checks and rollout gates
- enforce artifact promotion by digest
- implement migration safety pattern (expand/contract)
- update runbook with exact rollback triggers
