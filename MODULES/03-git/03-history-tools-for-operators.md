---
title: History Tools for Operators (Reflog, Bisect, Revert)
tags:
  - git
  - operations
  - debugging
module: "03"
---

# History Tools for Operators (Reflog, Bisect, Revert)

## Operator Goals

During incidents, Git is used to answer:

- what changed?
- when did it change?
- which change caused the regression?
- how do we rollback safely?

## Reflog: Your “Undo” for Mistakes

Reflog tracks where HEAD and branch refs have been:

```bash
git reflog
```

Use cases:

- recover after hard reset
- recover a deleted branch tip (if not garbage collected)
- find the commit you were on before a failed rebase

Practice rule:

- do not panic; check reflog first

## Revert vs Reset vs Rebase (Operational Safety)

- revert: creates a new commit that undoes changes; safe for shared history
- reset: moves branch pointer; risky if branch is shared
- rebase: rewrites commits; powerful but coordination-heavy in shared branches

Rule of thumb:

- on shared branches, prefer revert (or a new “fix forward” commit)
- in private/local branches, rebase and reset are fine if you understand consequences

## Bisect: Find the Regression Fast

Bisect is a binary search over commits.

You need:

- a known good commit
- a known bad commit
- a deterministic test to classify a commit as good/bad

Commands:

```bash
git bisect start
git bisect bad
git bisect good <good_commit>
```

Then run a test each step and mark good/bad.

Staff-level nuance:

- flaky tests break bisect; you need a more stable signal
- environment changes can create false regressions; control the environment

## Blame (Use Carefully)

`git blame` is useful, but it can mislead:

- code moved or refactored
- commits squashed or rewritten
- the “blamed” commit may not contain the true causal decision

Use it to find context and owners, not to assign blame.

## Tags and Release Forensics

Tags let you answer:

- what version is deployed?
- which commit corresponds to that release?

Prefer annotated tags for releases.
