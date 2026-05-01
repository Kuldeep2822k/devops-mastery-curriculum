---
title: 'Trace a Delivery Failure'
tags:
  - lab
  - email
  - debugging
module: "19"
---

# Lab 02 — Trace a Delivery Failure (421/451/550 + Retry Policy)

## Goal

Practice tracing delivery failures using evidence:

- connection failures (server down)
- permanent SMTP failures (550)
- transient SMTP failures (451) and safe retry behavior
- dead-lettering after retry budget exhausted

## Prereqs

- `python3`
- Lab 01 familiarity (local SMTP setup pattern)

## Setup

```bash
mkdir -p ~/work/devops-labs/mod19-trace
cd ~/work/devops-labs/mod19-trace
mkdir -p out dlq
```

Create an SMTP server with failure modes:

```bash
cat > smtp_server_policy.py <<'EOF'
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
    def __init__(self):
        self.mail_from = ""
        self.rcpt_to = []

async def handle(reader, writer, out_dir, mode):
    s = Session()
    writer.write(b"220 mod19.policy ESMTP\r\n")
    await writer.drain()

    data_mode = False
    data_lines = []

    while True:
        line = await reader.readline()
        if not line:
            break
        raw = line.decode("utf-8", errors="replace").rstrip("\r\n")
        cmd = raw.upper()

        if data_mode:
            if raw == ".":
                ts = datetime.datetime.utcnow().strftime("%Y%m%dT%H%M%SZ")
                msg_path = os.path.join(out_dir, f"msg-{ts}.eml")
                with open(msg_path, "w", encoding="utf-8") as f:
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

        if cmd.startswith("EHLO") or cmd.startswith("HELO"):
            writer.write(b"250-mod19.policy\r\n250 SIZE 10485760\r\n")
            await writer.drain()
            continue
        if cmd.startswith("MAIL FROM:"):
            s.mail_from = extract_addr(raw[len("MAIL FROM:"):])
            writer.write(b"250 2.1.0 OK\r\n")
            await writer.drain()
            continue
        if cmd.startswith("RCPT TO:"):
            rcpt = extract_addr(raw[len("RCPT TO:"):])
            s.rcpt_to.append(rcpt)
            if mode == "reject_550":
                writer.write(b"550 5.7.1 rejected\r\n")
                await writer.drain()
                continue
            if mode == "tempfail_451":
                writer.write(b"451 4.7.1 try again later\r\n")
                await writer.drain()
                continue
            writer.write(b"250 2.1.5 OK\r\n")
            await writer.drain()
            continue
        if cmd == "DATA":
            writer.write(b"354 End data with <CR><LF>.<CR><LF>\r\n")
            await writer.drain()
            data_mode = True
            data_lines = []
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
    ap.add_argument("--port", type=int, default=1026)
    ap.add_argument("--out", default="out")
    ap.add_argument("--mode", default="ok", choices=["ok","reject_550","tempfail_451"])
    args = ap.parse_args()
    os.makedirs(args.out, exist_ok=True)
    server = await asyncio.start_server(lambda r, w: handle(r, w, args.out, args.mode), args.host, args.port)
    async with server:
        await server.serve_forever()

if __name__ == "__main__":
    asyncio.run(main())
EOF
```

Create a retrying sender with a retry budget and DLQ:

```bash
cat > send_with_retry.py <<'EOF'
import argparse
import smtplib
import time
from email.message import EmailMessage

def main():
    ap = argparse.ArgumentParser()
    ap.add_argument("--host", default="127.0.0.1")
    ap.add_argument("--port", type=int, default=1026)
    ap.add_argument("--mail-from", default="sender@example.com")
    ap.add_argument("--rcpt-to", default="rcpt@example.com")
    ap.add_argument("--subject", default="mod19 trace")
    ap.add_argument("--body", default="hello")
    ap.add_argument("--retries", type=int, default=3)
    ap.add_argument("--backoff-ms", type=int, default=200)
    ap.add_argument("--dlq", default="dlq")
    args = ap.parse_args()

    msg = EmailMessage()
    msg["From"] = args.mail_from
    msg["To"] = args.rcpt_to
    msg["Subject"] = args.subject
    msg.set_content(args.body)

    last_err = None
    for attempt in range(1, args.retries + 2):
        try:
            s = smtplib.SMTP(args.host, args.port, timeout=3)
            s.set_debuglevel(1)
            s.send_message(msg)
            s.quit()
            print(f"sent attempt={attempt}")
            return 0
        except smtplib.SMTPResponseException as e:
            code = int(e.smtp_code)
            print(f"smtp_err attempt={attempt} code={code}")
            last_err = f"SMTP {code}"
            if 500 <= code <= 599:
                break
        except Exception as e:
            print(f"conn_err attempt={attempt} err={type(e).__name__}")
            last_err = type(e).__name__

        time.sleep(min(args.backoff_ms * (2 ** (attempt - 1)), 2000) / 1000.0)

    ts = int(time.time())
    import os
    os.makedirs(args.dlq, exist_ok=True)
    p = os.path.join(args.dlq, f"dlq-{ts}.txt")
    with open(p, "w", encoding="utf-8") as f:
        f.write(f"last_error={last_err}\n")
        f.write(f"to={args.rcpt_to}\n")
        f.write(f"subject={args.subject}\n")
    print(f"dead_lettered path={p}")
    return 2

if __name__ == "__main__":
    raise SystemExit(main())
EOF
```

## Verify

## Steps

### 1) Failure Mode: Server Down (Connection Failure)

Don’t start the server yet:

```bash
python3 send_with_retry.py --host 127.0.0.1 --port 1026 --retries 2 || true
ls -la dlq | tail -n 5
```

Expected signals:

- connection errors in client output
- DLQ file created

### 2) Failure Mode: Permanent Reject (550)

Start the policy server:

```bash
python3 smtp_server_policy.py --port 1026 --out out --mode reject_550
```

In another terminal:

```bash
python3 send_with_retry.py --host 127.0.0.1 --port 1026 --retries 2 || true
```

Expected:

- client sees SMTP 550
- it dead-letters without endless retries

### 3) Failure Mode: Temporary Fail (451) With Retry

Restart server with tempfail mode:

```bash
python3 smtp_server_policy.py --port 1026 --out out --mode tempfail_451
```

Send again:

```bash
python3 send_with_retry.py --host 127.0.0.1 --port 1026 --retries 2 || true
```

Expected:

- client retries a few times
- message ends up in DLQ after retry budget exhausted

### 4) Recovery: Healthy Server (OK)

Restart server with ok mode:

```bash
python3 smtp_server_policy.py --port 1026 --out out --mode ok
```

Send:

```bash
python3 send_with_retry.py --host 127.0.0.1 --port 1026 --retries 0
ls -la out | tail -n 5
```

Expected:

- client prints `sent attempt=1`
- message stored under `out/`

## Verify

```bash
test -n "$(ls -1 dlq/dlq-*.txt 2>/dev/null | head -n 1)"
test -n "$(ls -1 out/msg-*.eml 2>/dev/null | head -n 1)"
```

Expected signals:

- DLQ contains at least one failure record
- out/ contains at least one successfully accepted message

## Cleanup

Stop the server (Ctrl+C), then:

```bash
cd ~
rm -rf ~/work/devops-labs/mod19-trace
```

## Troubleshooting

### Symptom: you can’t distinguish 451 vs 550

Fix:

- look at SMTP status codes in client output; treat 4xx as retryable (bounded) and 5xx as permanent (no retry storm)

### Symptom: port already in use

Fix:

- choose a different local port and pass it to both server and client
