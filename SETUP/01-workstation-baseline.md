---
title: Workstation Baseline
tags:
  - setup
  - workstation
  - security
---

# Workstation Baseline

## Goal

Build a stable operator workstation baseline: consistent versions, safe shell defaults, and predictable environment behavior.

## Prereqs

- A Linux/macOS workstation or a Linux VM.
- Admin rights to install packages.

## Baseline Checklist (What “Good” Looks Like)

- OS up to date, rebooted, disk space available.
- Shell configured with safe history behavior (no accidental secrets).
- Time sync enabled (TLS and distributed debugging depend on correct clocks).
- SSH configured safely (agent use, keys, no password reuse).
- Basic triage tools installed (process/network/disk inspection).
- A dedicated workspace directory for labs.

## Setup

### Create a Lab Workspace Directory

Pick a dedicated directory to reduce accidental damage:

```bash
mkdir -p ~/work/devops-labs ~/work/devops-evidence
```

### Configure Safer Shell History

Goals:

- avoid storing obvious secret patterns in history
- keep enough history for debugging what you did

Recommended approach:

- use per-session history
- use a reasonable history size
- avoid commands that print secrets in the first place

If you use bash, consider setting:

```bash
export HISTSIZE=50000
export HISTFILESIZE=100000
export HISTCONTROL=ignoreboth
shopt -s histappend
```

If you use zsh:

```bash
export HISTSIZE=50000
export SAVEHIST=100000
setopt hist_ignore_space
setopt hist_reduce_blanks
setopt inc_append_history
```

Do not rely on history settings as “security”. The real rule is: never print secrets.

### Time Sync

Correct time is required for:

- TLS validation
- log correlation
- token expiration debugging

Verify:

```bash
date
```

Linux (systemd) check:

```bash
timedatectl status
```

Expected signals:

- “System clock synchronized: yes” (or equivalent)

### Basic Packages (Linux)

Install baseline utilities (choose your distro package manager):

Core triage:

- `curl`, `wget`
- `jq`
- `git`
- `make`
- `openssl`
- `tcpdump` (optional; requires privileges)
- `lsof`, `ps`, `top/htop`
- `dig`/`nslookup` (often in `dnsutils`/`bind-utils`)
- `netcat` (`nc`)

Verify:

```bash
curl --version
jq --version
git --version
openssl version
```

Expected signals:

- commands exist and return version output

## Operational Hygiene

### Resource Monitoring Baseline

You need to notice when your local lab is the problem:

```bash
df -h
free -m || vm_stat
uptime
```

### Local DNS and Proxy Awareness

Corporate networks may:

- intercept TLS
- force proxies
- block registries

Record:

- whether you are behind a proxy
- whether DNS is split-horizon
- whether you need custom CA certs

These will become real troubleshooting inputs later.

## Troubleshooting

### Symptom: TLS errors everywhere

Diagnosis:

- check time: `date`
- check system CAs and proxy environment variables

Fix:

- correct time sync
- install the correct enterprise CA if required (do not bypass TLS verification as a default)

### Symptom: “command not found”

Diagnosis:

- confirm install location
- confirm PATH

Fix:

- install via package manager
- add the correct PATH export in your shell profile

## Why This Matters in Production

- Broken time sync causes cascading auth and TLS failures that look like “random outages”.
- Missing core tools causes slow triage and higher MTTR.
- Unsafe shell habits leak secrets; that turns incidents into security events.
