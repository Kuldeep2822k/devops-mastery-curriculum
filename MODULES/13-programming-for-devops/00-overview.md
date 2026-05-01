---
title: "Module 13: Programming for DevOps"
tags:
  - module
  - programming
  - automation
module: "13"
---

# Module 13 — Programming for DevOps

## Outcomes

You can:

- Write small, reliable automation tools (CLI scripts) with good failure behavior.
- Use APIs safely (timeouts, retries with backoff, pagination, rate limits).
- Parse and generate structured data (JSON/YAML) without brittle grep pipelines.
- Design scripts that are idempotent and safe to rerun in production.
- Add minimal tests and reproducible execution (Make targets).

## Prereqs

- Linux and Git basics: [Module 02](../02-linux/00-overview.md), [Module 03](../03-git/00-overview.md)

## Module Map

- Concepts:
  - [01-reliability-in-scripts.md](01-reliability-in-scripts.md)
  - [02-http-apis-and-resilience.md](02-http-apis-and-resilience.md)
  - [03-data-parsing-and-formats.md](03-data-parsing-and-formats.md)
  - [04-testing-and-packaging.md](04-testing-and-packaging.md)
- Deep dives:
  - [deep-dive-01.md](deep-dive-01.md)
  - [deep-dive-02.md](deep-dive-02.md)
- Labs:
  - [lab-01-build-a-safe-cli-tool.md](lab-01-build-a-safe-cli-tool.md)
  - [lab-02-api-client-retries-and-pagination.md](lab-02-api-client-retries-and-pagination.md)
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

1. Read concepts and deep dives.
2. Complete lab-01 (safe CLI tool).
3. Complete lab-02 (API client resilience).
4. Run troubleshooting scenarios and exam.
5. Write a runbook and ADR about automation safety standards.
