---
title: Rubric (0–4)
tags:
  - rubric
  - observability
module: "11"
---

# Rubric — Module 11 Observability

Use global rubric meanings: [Evidence and Rubrics](../../00-HOW-TO-USE/04-evidence-rubrics.md)

## Skill 1: Signals and Instrumentation

- 0: No useful signals; relies on guesswork.
- 1: Some logs/metrics exist but inconsistent and hard to use.
- 2: Structured logs and basic metrics; can debug common failures.
- 3: Clear signal design with version markers and low-cardinality metrics.
- 4: Builds signal standards and improves observability across services.

## Skill 2: Alerting Quality

- 0: Alert storms and noisy pages.
- 1: Alerts exist but not actionable; missing runbooks.
- 2: Alerts are actionable and map to user impact.
- 3: Uses burn-rate thinking and multi-window policies to reduce noise.
- 4: Drives org-wide alert hygiene and on-call sustainability.

## Skill 3: SLOs and Dashboards

- 0: No SLOs; dashboards are screenshots.
- 1: Some dashboards exist but don’t answer questions.
- 2: Defines SLOs and builds dashboards with drill-down paths.
- 3: Dashboards tie to incidents and releases; supports fast forensics.
- 4: Dashboards become products with ownership and continuous improvement.

## Skill 4: Incident Use and Writing

- 0: No evidence capture; no runbooks.
- 1: Some evidence but inconsistent; slow diagnosis.
- 2: Uses signals to diagnose; runbook is executable.
- 3: Strong post-incident improvements; reduces MTTR.
- 4: Teaches others and standardizes incident observability playbooks.
