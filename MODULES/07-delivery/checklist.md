---
title: Checklist (Definition of Done)
tags:
  - checklist
  - delivery
module: "07"
---

# Checklist — Module 07 Delivery (DoD)

- [ ] Explain “build once, promote many” and why rebuild-per-env breaks provenance.
- [ ] Compare rolling, canary, and blue/green deployments and choose correct strategy for a scenario.
- [ ] Define explicit rollback triggers and verification signals (error rate/latency/health).
- [ ] Complete Lab 01 with evidence:
  - [ ] rollout to v1 verified
  - [ ] controlled failure introduced and diagnosed via endpoints/events
  - [ ] rollback executed and verified
- [ ] Complete Lab 02 with evidence:
  - [ ] stable+canary serving verified
  - [ ] canary failure detected and rolled back
  - [ ] canary promoted intentionally and verified
- [ ] Complete at least 12 scenarios from [troubleshooting-lab.md](troubleshooting-lab.md).
- [ ] Complete [exam.md](exam.md) with evidence (commands + expected signals + reasoning).
- [ ] Write a delivery runbook using [runbook-template.md](runbook-template.md).
- [ ] Write one ADR using [decision-record-template.md](decision-record-template.md) about deployment strategy, rollback policy, or migration safety.
- [ ] Self-grade via [rubric.md](rubric.md) and list 3 improvement actions.
