---
title: Evidence and Rubrics
tags:
  - evidence
  - assessment
---

# Evidence and Rubrics

This vault includes per-module checklists, rubrics, and exams. Use them as acceptance tests. The goal is to make your skill measurable.

## Evidence Standards (What “Good” Looks Like)

Evidence is not a screenshot of success. Evidence is a record that allows someone else to:

- reproduce your system
- verify it is working
- diagnose it when it fails

Minimum evidence per module:

- lab reports (two labs)
- troubleshooting log (at least one time-boxed drill)
- exam submission outputs (commands + expected signals + reasoning)
- a runbook draft and an ADR draft
- completed checklist.md and rubric.md scoring

## How to Use checklist.md (Definition of Done)

Treat checklist.md as the contract:

- If an item says “verify”, capture the verification signal.
- If an item says “runbook”, write at least the first usable version.
- If an item says “prevention”, include at least one change to reduce recurrence.

If you cannot complete a checklist item, record:

- what blocked you
- what you tried
- what you’d need (time, access, missing tooling)

## Rubric Scoring (0–4)

Use the same scale across modules:

- 0: cannot perform without step-by-step instructions; unsafe defaults
- 1: can perform with guidance; cannot troubleshoot reliably
- 2: can perform independently in a known environment; can troubleshoot common failures
- 3: can adapt to unfamiliar constraints; can reason from signals and write solid runbooks
- 4: can design improvements, anticipate failure modes, and mentor others; makes sound tradeoffs

## “Staff Signal” Behaviors (What You Practice)

You demonstrate staff-level capability by:

- documenting tradeoffs (ADRs), not just choosing tools
- designing safe rollout/rollback paths
- building observability as a feature (not a last-minute add-on)
- reducing MTTR through runbooks, alerts, and guardrails
- handling incidents with calm process and clear comms

## Exam Submissions: How to Write Them

An exam submission should include:

- assumptions and constraints (what you are/aren’t allowed to do)
- commands you ran (with brief intent)
- outputs or expected signals (not secrets)
- diagnosis notes (why you concluded root cause)
- fix steps (minimal change)
- verification (what proves the issue is resolved)
- prevention (what reduces recurrence)

## Evidence Packaging

Keep evidence in your own repo/folder. A clean structure:

```
modules/<module-id>/
  lab-01-report.md
  lab-02-report.md
  troubleshooting-log.md
  exam-submission.md
  runbook.md
  adr-0001-<topic>.md
```

Use the note templates: [06-notes-template.md](06-notes-template.md)
