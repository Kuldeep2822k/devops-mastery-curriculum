---
title: Troubleshooting Lab (Scenarios)
tags:
  - troubleshooting-lab
  - git
  - oncall
module: "03"
---

# Troubleshooting Lab — Module 03 Git

## Instructions

- Do 12–20 scenarios.
- Time-box each: 10–20 minutes.
- Produce: symptoms, constraints, hints, diagnosis commands, root cause, fix, verification, prevention.

Use incident log template: [notes template](../../00-HOW-TO-USE/06-notes-template.md)

## Scenarios

### Scenario 01 — “Detached HEAD” After Checking a Commit

- Symptoms: `git status` shows detached HEAD; commits made but not on a branch.
- Constraints: must not lose work.
- Hints: create a branch pointer.
- Diagnosis commands:
  - `git status`
  - `git log --oneline -n 5`
- Root cause: checked out a commit directly.
- Fix: `git switch -c recover/detached-work`.
- Verification: `git branch -vv` shows branch at expected commit.
- Prevention: create topic branches before committing; understand what HEAD points to.

### Scenario 02 — Local Hard Reset Lost a Commit

- Symptoms: last commit missing.
- Constraints: no remote yet.
- Hints: reflog.
- Diagnosis commands:
  - `git reflog -n 20`
- Root cause: `git reset --hard` moved branch pointer.
- Fix: `git reset --hard <hash-from-reflog>`.
- Verification: `git log --oneline -n 5` shows restored commit.
- Prevention: prefer `git revert` for shared branches; create backup branch before risky ops.

### Scenario 03 — Rebase Gone Wrong

- Symptoms: conflicts; history looks wrong; you want to abort.
- Constraints: do not force-push.
- Hints: `git rebase --abort` and reflog.
- Diagnosis commands:
  - `git status`
  - `git reflog -n 20`
- Root cause: rebase conflict or wrong base.
- Fix: `git rebase --abort` or reset to pre-rebase reflog entry.
- Verification: branch returns to previous commit and working tree clean.
- Prevention: rebase private branches; coordinate before rewriting shared history.

### Scenario 04 — Push Rejected (Non-Fast-Forward)

- Symptoms: push fails with non-fast-forward.
- Constraints: main branch is shared; no force push.
- Hints: fetch and inspect divergence.
- Diagnosis commands:
  - `git fetch --all`
  - `git log --oneline --decorate --graph --all -n 20`
- Root cause: remote has commits not in local branch (or divergent history).
- Fix: merge or rebase per policy, then push.
- Verification: push succeeds; history matches expectations.
- Prevention: pull/fetch before pushing; avoid long-lived branches.

### Scenario 05 — Accidentally Committed to main

- Symptoms: changes landed on main locally but should be in feature branch.
- Constraints: main is protected on remote.
- Hints: move commits to a branch.
- Diagnosis commands:
  - `git log --oneline -n 5`
- Root cause: work done on wrong branch.
- Fix pattern:
  - create branch at current commit: `git switch -c feature/fixup`
  - reset main back if not pushed: `git switch main && git reset --hard <previous>`
- Verification: feature branch contains commits; main restored.
- Prevention: set branch protections and local pre-check: confirm branch before commit.

### Scenario 06 — Wrong Files Included in Commit

- Symptoms: secrets or large files staged accidentally (do not print them).
- Constraints: do not expose secrets; assume not pushed yet.
- Hints: unstage safely.
- Diagnosis commands:
  - `git status`
  - `git diff --staged`
- Root cause: `git add .` staged too much.
- Fix:
  - `git restore --staged <path>`
  - amend commit if already committed: `git commit --amend` (local only)
- Verification: staged set contains only intended files.
- Prevention: use `.gitignore`; review staged diff before commit.

### Scenario 07 — Merge Conflict in a Config File

- Symptoms: conflict markers in config.
- Constraints: must choose correct final config for target environment.
- Hints: decide intent; don’t guess.
- Diagnosis commands:
  - `git status`
  - open conflicted file
- Root cause: parallel edits to same lines.
- Fix: resolve to correct final state; `git add`; `git commit`.
- Verification: config file content correct; tests/smoke checks pass.
- Prevention: reduce conflict surface; use structured config patterns and shorter branches.

### Scenario 08 — Revert of Merge Commit Confusion

- Symptoms: reverting a merge commit fails or reverts wrong changes.
- Constraints: need a safe rollback.
- Hints: merge revert requires selecting mainline parent.
- Diagnosis commands:
  - `git show <merge_commit>`
  - `git log --oneline --decorate --graph -n 20`
- Root cause: merge commits have multiple parents; revert needs `-m`.
- Fix: revert with correct mainline parent: `git revert -m 1 <merge_commit>`.
- Verification: diff reflects rollback; tests/smoke checks pass.
- Prevention: document rollback approach (revert PR merge) in runbook.

### Scenario 09 — Tag Points to Wrong Commit

- Symptoms: release tag does not match deployed artifact.
- Constraints: published tags should be treated as immutable.
- Hints: create a new tag rather than moving old.
- Diagnosis commands:
  - `git show <tag>`
  - `git rev-parse <tag>`
- Root cause: tag created at wrong commit.
- Fix: create a new corrected tag (do not move published tag); update release notes.
- Verification: new tag points to correct commit.
- Prevention: tag from CI using commit SHA; verify tag targets before publishing.

### Scenario 10 — “git pull” Creates Unexpected Merge

- Symptoms: unexpected merge commit on branch.
- Constraints: maintain policy (merge-based or rebase-based).
- Hints: set pull strategy explicitly.
- Diagnosis commands:
  - `git config --get pull.rebase || true`
  - `git log --oneline --graph -n 20`
- Root cause: default pull behavior differs from expectation.
- Fix: adjust config; if needed, reset branch locally and reapply safely (avoid rewriting shared history).
- Verification: future pulls behave as expected.
- Prevention: document repo workflow and enforce via tooling.

### Scenario 11 — “Everything is Modified” After Branch Switch

- Symptoms: switching branches shows many modified files.
- Constraints: don’t lose work; don’t commit noise.
- Hints: uncommitted changes and line endings.
- Diagnosis commands:
  - `git status`
  - `git diff --stat`
- Root cause: uncommitted changes carry over or autocrlf/line ending issues.
- Fix: stash changes or commit them; normalize line endings with `.gitattributes` (later).
- Verification: clean status after resolving.
- Prevention: keep working tree clean; standardize line endings.

### Scenario 12 — Bisect Gives Inconsistent Results

- Symptoms: bisect points to different commits across runs.
- Constraints: do not “average” results.
- Hints: test must be deterministic.
- Diagnosis commands:
  - rerun the test at a fixed commit multiple times
  - inspect environment assumptions
- Root cause: flaky test or environment-dependent behavior.
- Fix: define a more stable signal; isolate environment; rerun bisect.
- Verification: same commit identified consistently.
- Prevention: invest in deterministic tests and stable build environments (Module 04 CI).

### Scenario 13 — “git blame” Points to Refactor, Not Root Cause

- Symptoms: blame shows a refactor commit as last modifier.
- Constraints: find true causal decision.
- Hints: follow history across renames.
- Diagnosis commands:
  - `git log --follow <file>`
  - `git blame -w <file>`
- Root cause: code moved or reformatted.
- Fix: trace earlier commits; find the change that introduced behavior.
- Verification: you can identify the change by intent, not just last touch.
- Prevention: keep refactors separate from behavior changes; use meaningful commit messages.

### Scenario 14 — Wrong Remote URL / Auth Failures

- Symptoms: push/pull fails; auth prompts.
- Constraints: no password-based auth; avoid printing tokens.
- Hints: check remote URL and SSH setup.
- Diagnosis commands:
  - `git remote -v`
  - `ssh -T git@github.com`
- Root cause: wrong remote or auth not configured.
- Fix: set correct remote URL; configure SSH or CLI auth.
- Verification: fetch/push works.
- Prevention: standardize auth method; document in setup.

## Time-Boxed On-Call Drill (30–60 minutes)

Pick one scenario that simulates a production regression:

- constraints: no force-push to main, must preserve audit trail
- objective: identify regression commit and rollback via revert

Deliverables:

- a timeline with commands and reasoning
- a rollback commit/tag
- prevention actions (tests, policy, runbook updates)
