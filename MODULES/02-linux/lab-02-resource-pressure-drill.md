---
title: "Lab 02: Resource Pressure Drill (CPU/Mem/Disk)"
tags:
  - lab
  - linux
  - performance
  - incident
module: "02"
---

# Lab 02 — Resource Pressure Drill (CPU/Mem/Disk)

## Goal

Practice diagnosing and recovering from resource pressure:

- detect CPU saturation vs memory pressure vs disk full
- use command evidence rather than guesses
- apply minimal reversible fixes
- produce an incident timeline and prevention steps

## Prereqs

- Linux shell access
- `top` (or `htop`), `df`, `du`, `ss`, `ps`
- Docker installed is optional; not required

## Setup

Create a sandbox directory:

```bash
mkdir -p ~/work/devops-labs/mod02-pressure
cd ~/work/devops-labs/mod02-pressure
```

Create a basic “probe script” for repeated checks:

```bash
cat > probe.sh <<'EOF'
set -eu
echo "ts=$(date +%s)"
uptime || true
df -h | head -n 20 || true
free -m 2>/dev/null || true
ps -eo pid,cmd,%cpu,%mem --sort=-%cpu | head -n 10 || true
EOF
chmod +x probe.sh
```

## Steps

### 1) Baseline (Ground Truth)

```bash
./probe.sh
```

Record baseline notes: load average, free memory, disk usage %.

### 2) CPU Pressure Injection

Start a CPU burner:

```bash
python3 -c 'import time\nx=0\nwhile True:\n  x=(x+1)%10000000'
```

In another terminal, observe:

```bash
./probe.sh
top -b -n 1 | head -n 20 || true
```

Expected signals:

- elevated load average
- a python process near 100% CPU (or high)
- your system becomes slower

Containment and fix:

- stop the burner process (Ctrl+C)

Verification:

```bash
./probe.sh
```

Expected signals:

- load average begins to fall
- top CPU process is no longer the burner

### 3) Disk Pressure Injection (Safe, Bounded)

Create a bounded large file (adjust size if needed):

```bash
dd if=/dev/zero of=bigfile.bin bs=1M count=256 status=none
```

Observe disk usage:

```bash
df -h | head -n 20
ls -lh bigfile.bin
```

Expected signals:

- disk usage increases

Fix:

```bash
rm -f bigfile.bin
```

Verify:

```bash
df -h | head -n 20
```

### 4) Memory Pressure Simulation (Non-Destructive)

Avoid OOM-ing your workstation. Use a bounded allocation:

```bash
python3 -c 'import time\nx=["x"*1024*1024 for _ in range(128)]\nprint("allocated_mb=128")\ntime.sleep(60)'
```

In another terminal:

```bash
free -m 2>/dev/null || true
ps -eo pid,cmd,%mem --sort=-%mem | head -n 10
```

Expected signals:

- a python process shows increased memory usage

Fix:

- wait for it to exit, or stop it (Ctrl+C)

Verify:

```bash
free -m 2>/dev/null || true
```

## Verify

You can distinguish these by evidence:

- CPU pressure: high load, high %CPU, latency increases, but memory/disk may be normal
- disk pressure: df high usage, write failures possible, logs may fail
- memory pressure: free memory low, high RSS, potential OOM messages

## Cleanup

Ensure no injection processes are running and no big files remain:

```bash
rm -f bigfile.bin
ps aux | grep -E 'python3 -c' | head -n 20 || true
```

## Troubleshooting

### Symptom: system becomes unstable

Fix:

- stop injections immediately
- do not push memory allocations further

### Symptom: `free` not available

Fix:

- use `vm_stat` on macOS, or rely on `top` output

## Why This Matters in Production

- resource saturation is a top contributor to incidents
- CPU and memory issues often present as latency and timeouts
- disk issues cause cascading failures and data risk

## What to Write in a Runbook

- a 5-minute triage command list for CPU/mem/disk
- how to identify the top offending process
- safe containment options (traffic reduction, restart budget, rollback)

## Definition of Done

- You can generate pressure safely and recognize it via signals.
- You can recover and verify that signals return toward baseline.
