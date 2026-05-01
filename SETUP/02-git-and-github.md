---
title: Git and GitHub Setup
tags:
  - setup
  - git
  - collaboration
---

# Git and GitHub Setup

## Goal

Set up Git for safe, high-signal collaboration: clean commits, signed history (optional), correct identity, and reliable authentication without unsafe practices.

## Prereqs

- Git installed (verify with `git --version`)
- A Git hosting account (GitHub recommended for the CI module later)

## Setup

### Configure Identity

Use the correct name/email for your work and learning repos.

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

Verify:

```bash
git config --global --get user.name
git config --global --get user.email
```

### Configure Default Branch Name (Optional)

```bash
git config --global init.defaultBranch main
```

### Configure Editor and Diff Tools

Set an editor you can operate under pressure:

```bash
git config --global core.editor "vim"
```

Optional readability helpers:

```bash
git config --global pager.branch false
git config --global diff.algorithm histogram
git config --global merge.conflictstyle zdiff3
```

### Configure Safer Pull Strategy

Choose one and be consistent:

- merge-based (default): fewer surprises, noisier history
- rebase-based: cleaner history, requires more discipline

Recommended for learning operational clarity:

```bash
git config --global pull.rebase false
```

If you prefer rebase:

```bash
git config --global pull.rebase true
git config --global rebase.autoStash true
```

### Authentication (Do Not Use Passwords)

Preferred:

- SSH keys
- or GitHub CLI auth

SSH baseline (high level):

- create a key (do not paste private keys anywhere)
- add public key to GitHub
- use agent forwarding carefully

Verify SSH connectivity:

```bash
ssh -T git@github.com
```

Expected signals:

- a successful auth message (no shell access is fine)

### Commit Signing (Optional, Advanced)

Signing improves provenance but adds operational overhead. If you enable signing:

- ensure you can sign from both your local machine and CI (later)
- keep keys safe and revocable

Do not block yourself early; enable when you can debug failures quickly.

## Verification

Create a test repo and validate push:

```bash
mkdir -p ~/work/devops-labs/git-setup-test
cd ~/work/devops-labs/git-setup-test
git init
echo "ok" > README.md
git add README.md
git commit -m "chore: init"
```

Expected signals:

- commit created without identity errors

## Troubleshooting

### Symptom: “Please tell me who you are”

Fix:

- set `user.name` and `user.email` as above

### Symptom: push prompts for password or fails

Diagnosis:

- verify remote URL: `git remote -v`
- verify SSH: `ssh -T git@github.com`

Fix:

- switch remote to SSH
- use token-based auth via GitHub CLI if required

### Symptom: rebase conflicts often

Fix:

- consider merge-based pulls until you build conflict muscle memory
- practice conflict resolution in Module 03 Git

## Why This Matters in Production

- Authentication failures cause blocked hotfixes during incidents.
- Inconsistent history makes incident forensics harder.
- A predictable Git workflow reduces cognitive load when you’re on-call.
