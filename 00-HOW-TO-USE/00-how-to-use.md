---
title: How to Use This Vault
tags:
  - how-to
  - operating-model
---

# How to Use This Vault

This vault is designed to produce operational capability through repeatable labs, failure injection, evidence artifacts, and assessment. It is written to be used like an internal engineering enablement program: you can onboard a team with it.

> **Brand new?** Read [START-HERE.md](../START-HERE.md) first, and keep the [Glossary](../APPENDICES/glossary.md) open in another tab — the concept lessons introduce a lot of vocabulary quickly, and looking terms up as you go is expected, not cheating.

## The Core Loop (Per Module)

1. Read concept lessons (01–04) fast to build a mental model.
2. Read deep dives (deep-dive-01/02) to learn tradeoffs and anti-patterns.
3. Do lab-01 and lab-02 end-to-end, including cleanup and troubleshooting.
4. Run the troubleshooting-lab time-boxed and write an incident log.
5. Fill checklist.md, self-grade rubric.md, and complete exam.md tasks.
6. Write a runbook draft and at least one ADR draft using templates in the module.

## What “Local-First” Means (In Practice)

- You can do the core path without a cloud account.
- You still practice real production skills: state, artifact promotion, rollbacks, observability, and incident response.
- Cloud extension labs exist to map patterns to AWS/Azure/GCP only after you can do them locally.

## How to Work Without External Guides

You are intentionally building the skill of reasoning from first principles and system signals:

- Use man pages, `--help`, and tool logs as your “documentation”.
- Use the troubleshooting framework: [05-troubleshooting-framework.md](05-troubleshooting-framework.md)
- Build internal docs like you would at work: runbooks and ADRs are required evidence.

## Evidence Folder (Recommended Convention)

Keep artifacts outside this guide folder, in a separate repo or folder:

```
my-devops-evidence/
  modules/
    04-ci/
      lab-01-report.md
      lab-02-report.md
      troubleshooting-log.md
      exam-submission.md
      runbook.md
      adr-0001-pipeline-layout.md
  projects/
    02-git-ci-demo/
      ...
```

What to write and how to grade: [04-evidence-rubrics.md](04-evidence-rubrics.md)

## “DoD First” Execution

Before you start a module, open checklist.md and skim the Definition of Done. Treat it like an acceptance test.

- If a lab doesn’t produce measurable signals, add them.
- If you can’t explain “why” a step exists, stop and write it down.

## Runbooks and Decision Records Are Not Optional

Operational maturity is visible in writing:

- Runbooks reduce MTTR, reduce cognitive load, and allow safe delegation.
- ADRs make tradeoffs explicit and prevent repeated arguments.

Templates exist per module so you practice writing in context.

## How to Use the Schedules

Schedules tell you what to do each week, which labs are required, and when to do mock exams.

- 30-day boot: minimum survival, minimal breadth
- 90-day deep: complete core and start staff breadth
- 180-day staff: deep integration and repeated incident practice
- 365-day mastery: repetition cycles, capstones, interview readiness

Schedules live in SCHEDULES/.
