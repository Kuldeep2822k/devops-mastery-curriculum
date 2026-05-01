---
title: Practical Exam
tags:
  - exam
  - git
module: "03"
---

# Practical Exam — Module 03 Git

## Rules

- Time-box: 90 minutes.
- No internet.
- Submit evidence: commands + expected signals + reasoning.
- Do not paste secrets in commits or logs.

## Tasks

### Task 1: Conflict and Merge Under Constraints

Requirements:

- Create two branches that modify the same lines.
- Merge them and resolve conflict by intent.
- Verify the final file content and history.

Grading criteria:

- conflict resolved correctly
- verification includes `git log --graph` and file content

### Task 2: Recovery Drill (Reflog)

Requirements:

- Perform a destructive local operation (hard reset or branch delete).
- Use reflog to recover the lost commit or branch tip.

Grading criteria:

- recovery is successful and explained
- evidence includes reflog entry and final state

### Task 3: Regression Hunt (Bisect)

Requirements:

- Create a small history with one regression commit.
- Use `git bisect` with a deterministic test (or script) to find first bad commit.

Grading criteria:

- correct good/bad boundaries
- bisect identifies the correct commit

### Task 4: Safe Rollback (Revert) + Tag

Requirements:

- Revert the regression commit (shared-history safe).
- Create an annotated tag for the “recovered” state.

Grading criteria:

- test passes after revert
- tag exists and points to the expected commit

### Task 5: Runbook + ADR

Requirements:

- Runbook: “Regression after merge” with bisect + revert flow and verification.
- ADR: choose merge policy (merge commits vs squash vs rebase) for a repo and justify.

Grading criteria:

- runbook has commands and expected signals
- ADR captures constraints, options, decision, risks, mitigations
