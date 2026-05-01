---
title: "Lab 01: Branching, Conflicts, and Recovery"
tags:
  - lab
  - git
  - conflicts
module: "03"
---

# Lab 01 — Branching, Conflicts, and Recovery

## Goal

Build practical Git collaboration muscle memory:

- create branches and merge changes
- intentionally create merge conflicts and resolve them correctly
- recover from common mistakes using reflog
- produce verification signals and an incident-style log of actions

## Prereqs

- Git installed and configured
- A workspace folder: `~/work/devops-labs`

## Setup

Create a repo:

```bash
mkdir -p ~/work/devops-labs/mod03-git-lab
cd ~/work/devops-labs/mod03-git-lab
git init
```

Create initial files:

```bash
cat > README.md <<'EOF'
# mod03 git lab
EOF

cat > app.conf <<'EOF'
port=8080
mode=dev
EOF

git add README.md app.conf
git commit -m "chore: init repo"
```

## Steps

### 1) Create Two Branches That Will Conflict

Branch A:

```bash
git switch -c feature/a
perl -0777 -pe 's/mode=dev/mode=prod/' -i app.conf
git add app.conf
git commit -m "feat: set mode to prod"
```

Branch B:

```bash
git switch -c feature/b main
perl -0777 -pe 's/mode=dev/mode=staging/' -i app.conf
git add app.conf
git commit -m "feat: set mode to staging"
```

### 2) Merge and Resolve Conflict

Merge B into main first:

```bash
git switch main
git merge feature/b
```

Now merge A into main (expect conflict):

```bash
git merge feature/a || true
git status
```

Resolve by choosing a final value (document why; pick `mode=prod` for this lab):

```bash
cat > app.conf <<'EOF'
port=8080
mode=prod
EOF
git add app.conf
git commit -m "merge: resolve mode conflict"
```

### 3) Verify History and Result

```bash
git log --oneline --decorate --graph -n 15
cat app.conf
```

Expected signals:

- merge commit or conflict resolution commit exists
- `app.conf` shows the intended final content

### 4) Create a Mistake: Accidental Hard Reset

Simulate a common incident:

```bash
git reset --hard HEAD~1
git log --oneline -n 5
```

Expected signal:

- the last commit disappears from history (but is recoverable)

### 5) Recover Using Reflog

```bash
git reflog -n 10
```

Find the commit before the reset and restore:

```bash
git reset --hard HEAD@{1}
git log --oneline -n 5
```

## Verify

- `git status` is clean
- `app.conf` matches expected content
- reflog-based recovery restored the lost commit

Verification commands:

```bash
git status
cat app.conf
git log --oneline --decorate -n 10
```

## Cleanup

Optional: remove the lab repo directory when done:

```bash
cd ~
rm -rf ~/work/devops-labs/mod03-git-lab
```

## Troubleshooting

### Symptom: merge conflict too confusing

Diagnosis:

- read `git status`
- open the conflicted file and understand markers

Fix:

- decide the intended final content (don’t guess)
- re-run `git add` and commit

### Symptom: reflog doesn’t show expected entry

Diagnosis:

- reflog is per-repo; ensure you are in the correct repo
- ensure GC has not removed objects (rare in this lab)

Fix:

- run `git reflog --all -n 50`

## Why This Matters in Production

- Merge conflicts happen during urgent hotfixes and incident-driven changes.
- reflog recovery prevents panic-driven “rebuild from scratch” mistakes.

## What to Write in a Runbook

- conflict resolution steps with verification commands
- reflog recovery procedure for common “oops” events

## Definition of Done

- You can create and resolve a merge conflict correctly.
- You can recover from a destructive local operation via reflog.
