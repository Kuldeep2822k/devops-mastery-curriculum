---
title: 'SMTP Session Drill'
tags:
  - lab
  - email
  - smtp
module: "19"
---

# Lab 01 — SMTP Session Drill (Local SMTP Server + Client)

## Goal

Practice the SMTP mental model by running a local SMTP server and sending mail via a real client:

- understand the envelope (MAIL FROM / RCPT TO) vs message content (DATA)
- capture the raw message as evidence
- distinguish connection failures vs SMTP rejections

## Prereqs

- `python3`

## Setup

```bash
mkdir -p ~/work/devops-labs/mod19-smtp
cd ~/work/devops-labs/mod19-smtp
mkdir -p out
```

Create a minimal SMTP server that stores messages to disk:

```bash
cat > smtp_server.py <<'EOF'
import argparse
import asyncio
import datetime
import os
import re

ADDR_RE = re.compile(r"<([^>]+)>")

def extract_addr(s):
    m = ADDR_RE.search(s)
    return m.group(1) if m else s.strip()

class Session:
    def __init__(self, out_dir):
        self.out_dir = out_dir
        self.mail_from = ""
        self.rcpt_to = []

async def handle_client(reader, writer, out_dir):
    s = Session(out_dir)
    peer = writer.get_extra_info("peername")
    writer.write(b"220 mod19.local ESMTP\r\n")
    await writer.drain()

    data_mode = False
    data_lines = []

    while True:
        line = await reader.readline()
        if not line:
            break
        raw = line.decode("utf-8", errors="replace").rstrip("\r\n")

        if data_mode:
            if raw == ".":
                ts = datetime.datetime.utcnow().strftime("%Y%m%dT%H%M%SZ")
                msg_path = os.path.join(out_dir, f"msg-{ts}.eml")
                with open(msg_path, "w", encoding="utf-8") as f:
                    f.write(f"X-Received-From: {peer}\n")
                    f.write(f"X-Envelope-From: {s.mail_from}\n")
                    f.write(f"X-Envelope-To: {','.join(s.rcpt_to)}\n")
                    f.write("\n".join(data_lines) + "\n")
                data_mode = False
                data_lines = []
                writer.write(b"250 2.0.0 OK queued\r\n")
                await writer.drain()
                continue
            data_lines.append(raw)
            continue

        cmd = raw.upper()
        if cmd.startswith("EHLO") or cmd.startswith("HELO"):
            writer.write(b"250-mod19.local\r\n250 SIZE 10485760\r\n")
            await writer.drain()
            continue
        if cmd.startswith("MAIL FROM:"):
            s.mail_from = extract_addr(raw[len("MAIL FROM:"):])
            writer.write(b"250 2.1.0 OK\r\n")
            await writer.drain()
            continue
        if cmd.startswith("RCPT TO:"):
            s.rcpt_to.append(extract_addr(raw[len("RCPT TO:"):]))
            writer.write(b"250 2.1.5 OK\r\n")
            await writer.drain()
            continue
        if cmd == "DATA":
            writer.write(b"354 End data with <CR><LF>.<CR><LF>\r\n")
            await writer.drain()
            data_mode = True
            data_lines = []
            continue
        if cmd == "RSET":
            s.mail_from = ""
            s.rcpt_to = []
            writer.write(b"250 2.0.0 OK\r\n")
            await writer.drain()
            continue
        if cmd == "QUIT":
            writer.write(b"221 2.0.0 Bye\r\n")
            await writer.drain()
            break
        writer.write(b"502 5.5.2 Command not recognized\r\n")
        await writer.drain()

    writer.close()
    await writer.wait_closed()

async def main():
    ap = argparse.ArgumentParser()
    ap.add_argument("--host", default="127.0.0.1")
    ap.add_argument("--port", type=int, default=1025)
    ap.add_argument("--out", default="out")
    args = ap.parse_args()
    os.makedirs(args.out, exist_ok=True)
    server = await asyncio.start_server(lambda r, w: handle_client(r, w, args.out), args.host, args.port)
    async with server:
        await server.serve_forever()

if __name__ == "__main__":
    asyncio.run(main())
EOF
```

Create a mail sender:

```bash
cat > send_mail.py <<'EOF'
import argparse
import smtplib
from email.message import EmailMessage

def main():
    ap = argparse.ArgumentParser()
    ap.add_argument("--host", default="127.0.0.1")
    ap.add_argument("--port", type=int, default=1025)
    ap.add_argument("--mail-from", default="sender@example.com")
    ap.add_argument("--rcpt-to", default="rcpt@example.com")
    ap.add_argument("--subject", default="mod19 test")
    ap.add_argument("--body", default="hello")
    args = ap.parse_args()

    msg = EmailMessage()
    msg["From"] = args.mail_from
    msg["To"] = args.rcpt_to
    msg["Subject"] = args.subject
    msg.set_content(args.body)

    s = smtplib.SMTP(args.host, args.port, timeout=3)
    s.set_debuglevel(1)
    s.send_message(msg)
    s.quit()

if __name__ == "__main__":
    main()
EOF
```

## Verify

## Steps

### 1) Start Local SMTP Server

```bash
python3 smtp_server.py --port 1025 --out out
```

### 2) Send a Message (Client → Server)

In another terminal:

```bash
python3 send_mail.py --host 127.0.0.1 --port 1025 --mail-from sender@example.com --rcpt-to rcpt@example.com --subject "hello" --body "test body"
```

Expected signals:

- client prints SMTP conversation (debug)
- server stores a message under `out/`

### 3) Inspect Envelope vs Message Data

```bash
ls -la out | tail -n 5
latest="$(ls -1t out/msg-*.eml | head -n 1)"
sed -n '1,60p' "$latest"
```

Expected:

- `X-Envelope-From` and `X-Envelope-To` exist
- the message headers/body are present

## Verify

```bash
test -n "$(ls -1 out/msg-*.eml 2>/dev/null | head -n 1)"
latest="$(ls -1t out/msg-*.eml | head -n 1)"
grep -q '^Subject: hello' "$latest"
grep -q '^X-Envelope-From: sender@example.com' "$latest"
```

Expected signals:

- message file exists
- subject and envelope fields match inputs

## Cleanup

Stop the server (Ctrl+C), then:

```bash
cd ~
rm -rf ~/work/devops-labs/mod19-smtp
```

## Troubleshooting

### Symptom: connection refused

Diagnosis:

- server not running or wrong port

Fix:

- start server and confirm listening port:

```bash
ss -tulpn | grep -E ':(1025)\b' || true
```

### Symptom: client hangs

Fix:

- ensure firewall/proxy not intercepting local connections
- use `--host 127.0.0.1` explicitly
