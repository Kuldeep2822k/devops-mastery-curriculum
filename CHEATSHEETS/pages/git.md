---
title: 'Git'
tags:
  - cheatsheet
  - git
---

# Git Cheatsheet (Operator-Focused)

## Fast Triage

- What changed?
  - `git status -sb`
  - `git diff`
  - `git diff --staged`
- What commit introduced this?
  - `git log --oneline --decorate -n 20`
  - `git log -p -n 1 <sha>`

## Branching + Safety

- Update your branch safely:
  - `git fetch --prune`
  - `git rebase origin/<base-branch>` (or merge, depending on team policy)
- Abort a bad rebase:
  - `git rebase --abort`
- Recover lost work:
  - `git reflog -n 50`
  - `git checkout -b rescue <sha>`

## Debugging History

- Find when a line changed:
  - `git blame -w <file>`
- Search history for a string:
  - `git log -S 'string' -- <path>`
- Bisect a regression:
  - `git bisect start`
  - `git bisect bad`
  - `git bisect good <sha>`
  - `git bisect run <your-test-command>`
  - `git bisect reset`

## Undo Patterns (Pick the Right One)

- Undo local edits (danger):
  - `git restore <file>`
- Unstage:
  - `git restore --staged <file>`
- Create a revert commit (safe for shared branches):
  - `git revert <sha>`
- Reset local branch (danger on shared branches):
  - `git reset --hard <sha>`

