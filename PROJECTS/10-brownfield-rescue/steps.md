---
title: '10-brownfield-rescue: Steps'
tags:
  - project
---

# 10-brownfield-rescue — Steps

## Prereqs

- `python3`, `curl`
- choose a target service you can run locally (recommended: Project 07 service or Module 11 Lab 01 service)

## Setup

```bash
mkdir -p ~/work/devops-labs/10-brownfield-rescue/evidence
cd ~/work/devops-labs/10-brownfield-rescue
```

## Steps

### 1) Baseline and Problem Statement

Write these files:

```bash
cat > evidence/problem.md <<'EOF'
# Problem Statement

- Symptoms:
- Impact:
- Baseline signals:
- Last known good:
EOF
```

### 2) Stabilize Delivery (Build + Artifact Identity)

Create a release manifest template:

```bash
cat > evidence/release_manifest.md <<'EOF'
# Release Manifest

- version:
- commit:
- artifact identity (digest/checksum):
- rollback target:
EOF
```

### 3) Observability + Runbook

```bash
cat > evidence/runbook.md <<'EOF'
# Runbook

## First 10 Minutes

## Commands

## Containment

## Rollback
EOF

cat > evidence/alerts.md <<'EOF'
# Alerts

- availability alert:
- latency alert:
EOF
```

### 4) Incident Drill + Postmortem

```bash
cat > evidence/postmortem.md <<'EOF'
# Postmortem

## Timeline

## Root Cause

## What changed

## Action Items
EOF
```

Run one drill using your target service:

- inject failures (e.g., /fail, /slow, wrong config)
- capture exact commands, observed signals, and rollback decision

## Verify

```bash
test -s evidence/problem.md
test -s evidence/runbook.md
test -s evidence/alerts.md
test -s evidence/postmortem.md
```

## Cleanup

```bash
cd ~
rm -rf ~/work/devops-labs/10-brownfield-rescue
```

## Troubleshooting

- If the “brownfield” target is too complex, use the Project 07 service as your system-under-test.
