---
title: Common Mistakes (Wrong vs Right)
tags:
  - mistakes
  - git
module: "03"
---

# Common Mistakes — Module 03 Git

## 1) Wrong: Force-Push as Default

Wrong pattern:

- rewrite history and force-push because “it’s cleaner”

Right pattern:

- preserve shared history; use revert for rollback
- only rewrite private branches with coordination rules

## 2) Wrong: Conflict Resolution by Guessing

Wrong pattern:

- “take ours” or “take theirs” without understanding intent

Right pattern:

- understand both changes
- choose a correct final state
- verify via tests/smoke checks and file content

## 3) Wrong: Treat bisect as Magic

Wrong pattern:

- bisect with flaky tests or unstable signals

Right pattern:

- define deterministic good/bad signal (test, script, log pattern)
- confirm the boundaries are correct before starting

## 4) Wrong: Blame People

Wrong pattern:

- use `git blame` to assign fault

Right pattern:

- use blame to find context and owners
- focus on root cause and prevention

## 5) Wrong: Untagged Releases

Wrong pattern:

- deploy “main” without a tag or artifact identifier

Right pattern:

- tag releases; map artifacts to commits; keep rollback targets explicit
