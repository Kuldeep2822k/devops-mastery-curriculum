---
title: Rubric (0–4)
tags:
  - rubric
  - git
module: "03"
---

# Rubric — Module 03 Git

Use global rubric meanings: [Evidence and Rubrics](../../00-HOW-TO-USE/04-evidence-rubrics.md)

## Skill 1: Collaboration Workflow (Branches, Merges, Conflicts)

- 0: Cannot branch/merge without step-by-step help; conflicts cause panic.
- 1: Can complete workflows but struggles to resolve conflicts correctly.
- 2: Can resolve conflicts and verify outcomes reliably.
- 3: Can choose merge strategy based on risk; keeps history understandable for forensics.
- 4: Sets team policy, reduces merge pain, and mentors others effectively.

## Skill 2: Recovery and Forensics (Reflog, Revert, Tags)

- 0: Loses work after mistakes; no recovery strategy.
- 1: Can view reflog but uncertain how to restore.
- 2: Can recover from common mistakes and rollback via revert safely.
- 3: Uses tags/releases for traceability; chooses revert vs rewrite appropriately.
- 4: Designs repo practices that improve incident response and auditability.

## Skill 3: Regression Hunting (Bisect + Deterministic Signals)

- 0: Randomly guesses causal commits.
- 1: Can bisect with help but makes mistakes in good/bad boundaries.
- 2: Can bisect with a deterministic test and identify first-bad commit.
- 3: Can design stable “good/bad” signals even when tests are flaky.
- 4: Automates regression detection and makes bisecting consistently fast.

## Skill 4: Operational Writing (Runbook + ADR)

- 0: No usable docs.
- 1: Docs exist but lack verification signals and decision flow.
- 2: Runbook is executable; ADR captures tradeoffs.
- 3: Docs are concise, safe, and reduce MTTR; includes rollback triggers.
- 4: Docs become organizational assets; prevention and policies are clear.
