---
title: Study Method (Slow and Complete)
tags:
  - study
  - spaced-repetition
---

# Study Method (Slow and Complete)

This system is optimized for deep operational competence. The goal is fewer modules “completed” per week, but with real retention and confident troubleshooting.

## The Three Pillars

### 1) Retrieval Practice

Prefer recalling from memory over rereading:

- Answer review questions before looking at notes.
- Explain a system without opening docs.
- Reconstruct a pipeline or deployment process from scratch.

### 2) Spaced Repetition

Revisit the same concepts after delays:

- 24 hours: quick recall session
- 7 days: redo a lab verification section only
- 30 days: take a mini practical exam or troubleshooting scenario

### 3) Interleaving

Mix skills so you can apply them under real constraints:

- Pair “CI” with “containers” and “security” in the same week.
- Pair “observability” with “datastores” so you learn “signals over guesses”.

## Weekly Structure (Recommended)

- Day 1–2: concept sprint + deep dive reading
- Day 3–4: lab build + verification
- Day 5: failure injection + troubleshooting lab
- Day 6: exam tasks + rubric scoring
- Day 7: review + write runbook/ADR improvements

If time is limited, keep the failure injection and the runbook. Those build the staff-level behaviors.

## How to Take Notes (Operational Style)

Use notes to reduce future MTTR:

- Always include commands you ran and expected signals.
- Capture “if X, then Y” decision points.
- Write down what you would put into an alert or dashboard.

Use the template: [06-notes-template.md](06-notes-template.md)

## How to Detect You’re “Faking Progress”

Signs of shallow learning:

- You can follow steps but cannot explain the why.
- You can’t reproduce a lab after one week without rereading.
- You fix issues by random changes, not diagnosis.
- You can’t write a runbook that someone else can follow.

Fix:

- repeat only the verification and troubleshooting parts of labs
- time-box debugging and force hypothesis-driven diagnosis

## Constraints That Make You Better

When you are ready, add constraints:

- “No internet” practical exams
- “One restart only” rule during drills
- “Explain before fix” rule
- “Change log” requirement: every action logged with a reason
