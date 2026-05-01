---
title: "Lab 01: systemd Service Debug (Local)"
tags:
  - lab
  - linux
  - systemd
module: "02"
---

# Lab 01 — systemd Service Debug (Local)

## Goal

Create a small local systemd-managed service, then break and fix it using operational playbooks:

- validate unit behavior (user, ExecStart, environment)
- inspect status and logs via journald
- diagnose common failure modes (bad path, permission, port in use)
- write runbook steps and verification signals

## Prereqs

- A Linux system with systemd (or a Linux VM).
- `systemctl` and `journalctl` available.
- `python3` installed.

## Setup

Create a working directory:

```bash
mkdir -p ~/work/devops-labs/mod02-systemd
cd ~/work/devops-labs/mod02-systemd
```

Create a simple service script:

```bash
cat > app.py <<'EOF'
import os
import socket
import time

HOST = "0.0.0.0"
PORT = int(os.getenv("PORT", "9099"))

def main():
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    s.bind((HOST, PORT))
    s.listen(16)
    print(f"listening port={PORT}", flush=True)
    while True:
        conn, _ = s.accept()
        conn.sendall(b"ok\n")
        conn.close()

if __name__ == "__main__":
    main()
EOF
```

Create a systemd unit file in your user systemd directory:

```bash
mkdir -p ~/.config/systemd/user
cat > ~/.config/systemd/user/mod02-demo.service <<'EOF'
[Unit]
Description=mod02 demo service

[Service]
Type=simple
ExecStart=/usr/bin/python3 %h/work/devops-labs/mod02-systemd/app.py
Environment=PORT=9099
Restart=on-failure
RestartSec=1

[Install]
WantedBy=default.target
EOF
```

Reload user systemd:

```bash
systemctl --user daemon-reload
```

## Steps

### 1) Start and Verify Service

```bash
systemctl --user start mod02-demo.service
systemctl --user status mod02-demo.service --no-pager
```

Verify listener:

```bash
ss -tulpn | grep -E ':(9099)\b' || true
```

Verify connectivity:

```bash
nc -vz 127.0.0.1 9099
printf "ping\n" | nc 127.0.0.1 9099
```

Expected signals:

- service status shows active/running
- port 9099 is listening
- `nc` connects successfully and returns `ok`

### 2) Read Logs via journald

```bash
journalctl --user -u mod02-demo.service --since "5 min ago" --no-pager
```

Expected signals:

- a line containing `listening port=9099`

### 3) Break It: Wrong ExecStart Path

Edit the unit file to point to a non-existent path (simulate a bad deploy):

```bash
sed -i 's#app.py#missing.py#g' ~/.config/systemd/user/mod02-demo.service
systemctl --user daemon-reload
systemctl --user restart mod02-demo.service || true
systemctl --user status mod02-demo.service --no-pager
```

### 4) Diagnose and Fix

Diagnosis:

```bash
systemctl --user status mod02-demo.service --no-pager
journalctl --user -u mod02-demo.service --since "5 min ago" --no-pager | tail -n 50
systemctl --user cat mod02-demo.service
```

Root cause:

- ExecStart points to a missing file.

Fix:

```bash
sed -i 's#missing.py#app.py#g' ~/.config/systemd/user/mod02-demo.service
systemctl --user daemon-reload
systemctl --user restart mod02-demo.service
```

### 5) Break It: Port Already in Use

Create a conflicting listener:

```bash
python3 -c 'import socket; s=socket.socket(); s.bind(("0.0.0.0", 9099)); s.listen(1); print("conflict listening"); import time; time.sleep(600)'
```

In another terminal, restart the service:

```bash
systemctl --user restart mod02-demo.service || true
systemctl --user status mod02-demo.service --no-pager
journalctl --user -u mod02-demo.service --since "5 min ago" --no-pager | tail -n 50
```

Diagnosis commands:

```bash
ss -tulpn | grep -E ':(9099)\b' || true
```

Fix:

- stop the conflicting process, then:

```bash
systemctl --user restart mod02-demo.service
```

## Verify

```bash
systemctl --user is-active mod02-demo.service
printf "ping\n" | nc 127.0.0.1 9099
```

Expected signals:

- `active`
- response `ok`

## Cleanup

Stop and disable the user service:

```bash
systemctl --user stop mod02-demo.service || true
systemctl --user disable mod02-demo.service || true
rm -f ~/.config/systemd/user/mod02-demo.service
systemctl --user daemon-reload
```

Verify cleanup:

```bash
systemctl --user status mod02-demo.service --no-pager || true
ss -tulpn | grep -E ':(9099)\b' || true
```

Expected signals:

- service not found / inactive
- nothing listening on 9099

## Troubleshooting

### Symptom: `systemctl --user` not available

Diagnosis:

- you may be on a system without user systemd sessions

Fix:

- run this lab inside a Linux VM with systemd
- or adapt to system-level unit (requires admin privileges)

### Symptom: logs not showing up

Diagnosis:

- check journald:

```bash
journalctl --user --since "5 min ago" --no-pager | tail -n 50
```

Fix:

- ensure the service prints to stdout/stderr

## Why This Matters in Production

- Most “service down” incidents are debugged via supervisor state + logs + sockets.
- Bad paths, missing configs, and port collisions are common after deployments and restarts.

## What to Write in a Runbook

- Status commands and what “active” vs “failed” means.
- How to read the last 50 lines of service logs.
- How to identify port collisions and fix safely.

## Definition of Done

- You can intentionally break a unit and fix it using status/log/port evidence.
- You can explain the root cause and prevention for each break.
