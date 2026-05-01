---
title: Troubleshooting Guide
tags:
  - troubleshooting
  - git
module: "03"
---

# Troubleshooting — Module 03 Git

Use the global framework: [Troubleshooting Framework](../../00-HOW-TO-USE/05-troubleshooting-framework.md)

## Fast Triage (Git Incidents)

- What is broken?
  - wrong content merged?
  - lost commits?
  - cannot push?
  - conflict storm?
- What is the blast radius?
  - local only vs shared remote branch
- What is the safest containment?
  - stop pushing
  - create a backup branch
  - revert a commit/merge

## Core Commands

State:

```bash
git status
git branch -vv
git log --oneline --decorate --graph -n 20
git remote -v
```

Recover:

```bash
git reflog -n 50
git fsck --lost-found || true
```

Remote sync:

```bash
git fetch --all --prune
git log --oneline --decorate --graph --all -n 30
```

## Common Failures

### Merge Conflict

Signals:

- `git status` shows unmerged paths

Fix pattern:

- decide intended final file state
- edit file(s)
- `git add` resolved files
- `git commit`
- verify with tests/smoke checks

### “I lost commits” after reset/rebase

Fix pattern:

- `git reflog` to find old HEAD
- restore via `git reset --hard <hash>` (local only)
- or create a recovery branch: `git branch recover/<name> <hash>`

### “non-fast-forward” push rejected

Interpretation:

- remote has commits you don’t have or history diverged

Safer fix:

- `git fetch`
- inspect divergence
- merge or rebase (policy dependent)
- avoid force push on shared branches

### Detached HEAD confusion

Fix pattern:

- if you need to keep work, create a branch:

```bash
git switch -c recover/work
```
