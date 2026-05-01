---
title: 'Mock Practical Exams'
tags:
  - progress
  - exams
---

# Mock Practical Exams

Use these as timed, end-to-end drills (60–120 minutes) that combine multiple modules.

## Mock Exam 01 — CI + Containers + Delivery

- Scenario: A pipeline builds an image and deploys to a local cluster, but the deploy step fails and the wrong version is running.
- Constraints: No secrets in logs; promote by digest.
- Tasks:
  - Diagnose why the pipeline failed.
  - Fix pipeline to produce and publish the correct artifact.
  - Prove the correct version is deployed (explicit verification signals).
  - Write a short runbook entry + ADR note.

## Mock Exam 02 — Kubernetes + Networking

- Scenario: Service returns 503 after a rollout; DNS resolution is flaky.
- Tasks:
  - Use events/describe/logs/exec to isolate the failure.
  - Fix readiness/service selectors/DNS (as appropriate).
  - Create a 30-minute incident timeline + follow-ups.

## Mock Exam 03 — IaC + Drift + Safety

- Scenario: Terraform plan shows unexpected replacements and drift.
- Tasks:
  - Identify why replacement is happening.
  - Resolve drift safely without destructive changes.
  - Document “safe change” checklist in a runbook.

