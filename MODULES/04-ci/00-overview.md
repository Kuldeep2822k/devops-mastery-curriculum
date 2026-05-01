---
title: "Module 04: CI"
tags:
  - module
  - ci
  - pipelines
module: "04"
---

# Module 04 — CI (Continuous Integration)

## Outcomes

You can:

- Design CI pipeline architecture: stages, artifacts, caching, parallelism, fan-in/fan-out.
- Build a portable local-first pipeline using Makefile targets and a local runner simulation.
- Build a GitHub Actions pipeline with caching, artifacts, fail-fast, and smoke tests.
- Handle secrets safely: environment separation, approvals, and least-privilege access.
- Improve CI reliability: flaky tests, nondeterminism, timeouts, reruns, quarantines, and pipeline health metrics.
- Troubleshoot CI failures using evidence: logs, artifact inspection, environment diffs, lockfile integrity, and provenance basics.

## Prereqs

- Module 03 Git completed or equivalent.
- A GitHub account for the cloud extension lab (optional but recommended for the main CI lab).
- Docker installed (helpful for local runner simulation).

## Module Map

- Concepts:
  - [01-ci-pipeline-architecture.md](01-ci-pipeline-architecture.md)
  - [02-artifacts-caching-parallelism.md](02-artifacts-caching-parallelism.md)
  - [03-secrets-environments-approvals.md](03-secrets-environments-approvals.md)
  - [04-ci-reliability.md](04-ci-reliability.md)
- Deep dives:
  - [deep-dive-01.md](deep-dive-01.md)
  - [deep-dive-02.md](deep-dive-02.md)
- Labs:
  - [lab-01-portable-pipeline-local-runner.md](lab-01-portable-pipeline-local-runner.md)
  - [lab-02-github-actions-pipeline.md](lab-02-github-actions-pipeline.md)
- Cloud extension:
  - [cloud-extension-lab.md](cloud-extension-lab.md)
- Assessment and practice:
  - [checklist.md](checklist.md)
  - [rubric.md](rubric.md)
  - [review-questions.md](review-questions.md)
  - [exam.md](exam.md)
  - [common-mistakes.md](common-mistakes.md)
  - [troubleshooting.md](troubleshooting.md)
  - [troubleshooting-lab.md](troubleshooting-lab.md)
- Writing templates:
  - [runbook-template.md](runbook-template.md)
  - [decision-record-template.md](decision-record-template.md)

## Completion Path (Recommended)

1. Read concepts (01–04), then deep dives.
2. Build the portable pipeline locally (lab-01) and verify determinism.
3. Implement GitHub Actions pipeline (lab-02) and validate caching/artifacts/fail-fast.
4. Run troubleshooting-lab scenarios time-boxed.
5. Complete exam and self-grade via rubric.
6. Write a CI runbook and an ADR about pipeline design tradeoffs.
