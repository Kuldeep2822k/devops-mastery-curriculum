---
title: '01-linux-admin-lab: Steps'
tags:
  - project
---

# 01-linux-admin-lab — Steps

## Prereqs

- Linux host with systemd user sessions (or adapt to your init system)
- `curl`, `journalctl`, `ss`, `df`, `du`

## Setup

```bash
mkdir -p ~/work/devops-labs/01-linux-admin-lab/evidence
cd ~/work/devops-labs/01-linux-admin-lab
```

Create a tiny local HTTP service:

```bash
cat > server.py <<'EOF'
from http.server import BaseHTTPRequestHandler, HTTPServer

class H(BaseHTTPRequestHandler):
    def do_GET(self):
        if self.path == "/healthz":
            self.send_response(200)
            self.end_headers()
            self.wfile.write(b"ok")
            return
        self.send_response(404)
        self.end_headers()
        self.wfile.write(b"not found")
    def log_message(self, format, *args):
        return

HTTPServer(("127.0.0.1", 18070), H).serve_forever()
EOF
```

Create a user systemd unit:

```bash
mkdir -p ~/.config/systemd/user
cat > ~/.config/systemd/user/proj01.service <<'EOF'
[Unit]
Description=proj01 demo service

[Service]
ExecStart=/usr/bin/python3 %h/work/devops-labs/01-linux-admin-lab/server.py
Restart=on-failure
RestartSec=1

[Install]
WantedBy=default.target
EOF
systemctl --user daemon-reload
systemctl --user enable --now proj01.service
```

## Steps

### 1) Verify Healthy Service

```bash
systemctl --user status proj01.service --no-pager
curl -fsS http://127.0.0.1:18070/healthz
ss -tulpn | grep -E ':(18070)\\b' || true
```

Save evidence:

```bash
systemctl --user status proj01.service --no-pager > evidence/service_status.txt
journalctl --user -u proj01.service -n 80 --no-pager > evidence/service_logs.txt
```

### 2) Break/Fix: Bad ExecStart

Break it:

```bash
perl -pi -e 's|ExecStart=.*|ExecStart=/usr/bin/python3 /does/not/exist.py|' ~/.config/systemd/user/proj01.service
systemctl --user daemon-reload
systemctl --user restart proj01.service || true
systemctl --user status proj01.service --no-pager || true
```

Fix it:

```bash
perl -pi -e 's|ExecStart=.*|ExecStart=/usr/bin/python3 %h/work/devops-labs/01-linux-admin-lab/server.py|' ~/.config/systemd/user/proj01.service
systemctl --user daemon-reload
systemctl --user restart proj01.service
curl -fsS http://127.0.0.1:18070/healthz
```

### 3) Break/Fix: Disk Pressure (Sandbox)

Simulate:

```bash
mkdir -p sandbox
dd if=/dev/zero of=sandbox/bigfile.bin bs=10M count=50 status=none
du -h sandbox | sort -h | tail -n 5
```

Recover:

```bash
rm -f sandbox/bigfile.bin
du -h sandbox | sort -h | tail -n 5
```

### 4) Write Runbook Updates

- Add a section for: service down, bad ExecStart, and disk pressure.
- Include the exact commands you used above and the expected signals.

## Verify

```bash
curl -fsS http://127.0.0.1:18070/healthz
systemctl --user is-active proj01.service
```

Expected:

- health returns `ok`
- service is active

## Cleanup

```bash
systemctl --user disable --now proj01.service || true
rm -f ~/.config/systemd/user/proj01.service
systemctl --user daemon-reload
cd ~
rm -rf ~/work/devops-labs/01-linux-admin-lab
```

## Troubleshooting

### Symptom: systemctl --user fails

Fix:

- ensure you are in a user session with systemd user services enabled
- adapt to a foreground process + tmux if needed
