---
title: "Module 01: Foundations"
tags:
  - module
  - foundations
  - operations
module: "01"
---

# Module 01 — Foundations

## Outcomes

You can:

- Describe a service as an operational system (dependencies, failure modes, invariants, and signals).
- Define environments (dev/stage/prod) with separation goals and a minimal promotion model.
- Build a minimal “operationally ready” service locally: health checks, logs, basic SLO thinking.
- Execute a safe change with rollback and capture an incident log under time pressure.
- Write a basic runbook and a decision record (ADR) that captures tradeoffs.

## Prereqs

- Setup section completed: [SETUP](../../SETUP/00-overview.md) — for this module you only need `python3` and `curl`.
- Comfortable running CLI commands and editing files. New to that? Do the [prerequisites primer](../../SETUP/00-prerequisites-primer.md) first.

> **Beginner note:** This module is called "Foundations," but it introduces operator vocabulary quickly (SLO, RED, blast radius, ADR…). That's expected — read the concept pages (01–04) for the *shape* of the ideas, keep the [Glossary](../../APPENDICES/glossary.md) open, and get to **lab-01** for your first hands-on win. The two `deep-dive` pages are **advanced**; skip them on your first pass and return once the basics feel comfortable.

## Module Map

- Concepts:
  - [01-systems-thinking.md](01-systems-thinking.md)
  - [02-environments-and-change.md](02-environments-and-change.md)
  - [03-reliability-basics.md](03-reliability-basics.md)
  - [04-operational-writing.md](04-operational-writing.md)
- Deep dives:
  - [deep-dive-01.md](deep-dive-01.md)
  - [deep-dive-02.md](deep-dive-02.md)
- Labs:
  - [lab-01-local-service-ops-baseline.md](lab-01-local-service-ops-baseline.md)
  - [lab-02-change-and-rollback-drill.md](lab-02-change-and-rollback-drill.md)
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

1. Read concepts (01–04).
2. Read deep dives (two).
3. Complete lab-01, then lab-02.
4. Run the troubleshooting lab time-boxed.
5. Complete exam tasks and self-grade via rubric.
6. Produce a runbook and one ADR using templates.
