---
title: Learning Modes
tags:
  - learning
  - practice
---

# Learning Modes

Use different modes to build different forms of skill. Staff-level competence requires all of them.

## Mode A: Concept Sprint (30–60 minutes)

Goal: build a working mental model quickly.

- Read 01–04 concept lessons in a module.
- Write a one-page “model” note: components, invariants, failure modes, signals.
- Answer 5–10 review questions without looking up notes.

Output evidence:

- A short note explaining the mental model and key tradeoffs.

## Mode B: Lab Build (2–6 hours)

Goal: create muscle memory and operational instincts.

- Run lab steps exactly, including verification and cleanup.
- Capture outputs you would need in an incident (logs/events/describes).
- Keep a short timeline: what you tried, what worked, what failed.

Output evidence:

- lab report with verification signals and failure/fix notes.

## Mode C: Failure Injection (30–120 minutes)

Goal: learn recovery paths and build incident reflexes.

- Intentionally break a working system.
- Observe symptoms; do not jump to fixes.
- Diagnose using commands from troubleshooting framework.
- Fix, verify, and write prevention steps.

Output evidence:

- troubleshooting log: symptoms → commands → root cause → fix → prevention.

## Mode D: Time-Boxed On-Call Drill (30–60 minutes)

Goal: simulate pressure: limited time, incomplete info, tradeoffs.

- Pick a scenario (module troubleshooting-lab or catalog).
- Set a timer; follow a structured incident process.
- Produce an incident log and short postmortem.

Output evidence:

- incident log + postmortem + follow-up tasks.

## Mode E: Teach-Back (15–30 minutes)

Goal: validate depth; reveal gaps.

- Explain the system to an imaginary teammate.
- Include “what breaks”, “how we know”, and “how we fix”.

Output evidence:

- 5-minute outline or written explanation.

## Mode F: Assessment

Goal: force completeness.

- Take exam.md tasks with constraints (no internet, time-boxed).
- Score yourself using rubric.md and write one improvement plan.

Output evidence:

- exam submission + rubric score + next actions.
