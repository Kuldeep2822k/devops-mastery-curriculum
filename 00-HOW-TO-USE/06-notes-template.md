---
title: Notes Template
tags:
  - template
  - notes
---

# Notes Template

Copy this into your evidence repo for modules, labs, drills, and exams.

## Module Notes (Template)

---
title: "Module <ID>: <Name> Notes"
date: "<YYYY-MM-DD>"
tags:
  - module
  - devops-staff-guide
module: "<ID>"
---

### Outcomes

- 

### Mental Model (Explain Like You’re On-Call)

- Components:
- Inputs/outputs:
- Invariants (what must always be true):
- Failure modes:
- Signals (how you know it’s broken):

### Tradeoffs (What You’d Put in an ADR)

- Option A:
- Option B:
- Decision:
- Risks:
- Mitigations:

### Commands and Signals Cheat List

- Command:
  - Why:
  - Expected signal:

## Lab Report (Template)

---
title: "Lab: <Module ID> / <Lab Name>"
date: "<YYYY-MM-DD>"
tags:
  - lab
  - devops-staff-guide
module: "<ID>"
---

### Goal

### Setup Notes

### Steps (High Level)

1.

### Verification Signals

- Command:
  - Expected:
  - Actual:

### Cleanup Verification

- Command:
  - Expected:
  - Actual:

### What Broke / What I Learned

- Symptom:
- Diagnosis commands:
- Root cause:
- Fix:
- Prevention:

## Incident Log (Time-Boxed Drill) Template

---
title: "Incident Drill: <Scenario>"
date: "<YYYY-MM-DD>"
tags:
  - incident
  - drill
module: "<ID>"
---

### Summary

- Impact:
- Start time:
- End time:
- Severity:

### Timeline (Actions + Reasons)

- <time>:
  - action:
  - reason:
  - command(s):
  - result:

### Root Cause

### Fix

### Verification

### Prevention / Follow-Ups

## Exam Submission Template

---
title: "Exam: <Module ID>"
date: "<YYYY-MM-DD>"
tags:
  - exam
module: "<ID>"
---

### Constraints

### Tasks Completed

### Evidence

- Commands run (redacted):
- Expected signals:
- Actual signals:

### Reasoning

### Prevention
