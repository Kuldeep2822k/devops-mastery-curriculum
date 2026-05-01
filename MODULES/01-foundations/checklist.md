---
title: Checklist (Definition of Done)
tags:
  - checklist
  - foundations
module: "01"
---

# Checklist — Module 01 Foundations (DoD)

- [ ] Explain the service-as-a-system model for a simple service (dependencies, failure modes, signals).
- [ ] Define at least 3 invariants for the lab service (e.g., health endpoint behavior, latency target, logging format).
- [ ] Create a minimal operational interface:
  - [ ] `/healthz` endpoint
  - [ ] consistent structured logs
- [ ] Complete Lab 01 with:
  - [ ] verify success signals
  - [ ] inject a controlled failure and recover
  - [ ] cleanup verified (no listener on port)
- [ ] Complete Lab 02 with:
  - [ ] baseline measurement captured
  - [ ] regression introduced and detected via signals
  - [ ] rollback executed and verified
  - [ ] change log created with timestamps and reasons
- [ ] Complete at least 12 troubleshooting scenarios from [troubleshooting-lab.md](troubleshooting-lab.md) with written diagnosis and verification.
- [ ] Complete the practical [exam.md](exam.md) with evidence (commands + expected signals + reasoning).
- [ ] Write a runbook draft using [runbook-template.md](runbook-template.md) and include:
  - [ ] diagnosis commands
  - [ ] containment/rollback steps
  - [ ] verification signals
- [ ] Write one ADR using [decision-record-template.md](decision-record-template.md) about a tradeoff you made (e.g., rollback strategy, health definition, log format).
- [ ] Self-grade via [rubric.md](rubric.md) and identify 2 improvement actions.
