---
title: 'mTLS Between Two Local Services'
tags:
  - lab
  - mtls
  - service-mesh
module: "18"
---

# Lab 01 — mTLS Between Two Local Services (Local TLS + Client Certs)

## Goal

Build a local “service-to-service” HTTPS setup with mutual TLS (mTLS) and practice:

- generating a CA and leaf certificates
- running a server that requires client certs
- proving success and common failures (missing client cert, wrong CA)
- recovering safely without disabling TLS

## Prereqs

- `python3`
- `openssl`
- `curl`

## Setup

```bash
mkdir -p ~/work/devops-labs/mod18-mtls
cd ~/work/devops-labs/mod18-mtls
mkdir -p certs
```

Generate a local CA:

```bash
openssl genrsa -out certs/ca.key 2048
openssl req -x509 -new -nodes -key certs/ca.key -sha256 -days 365 \
  -subj "/CN=mod18-local-ca" \
  -out certs/ca.crt
```

Generate a server certificate:

```bash
openssl genrsa -out certs/server.key 2048
openssl req -new -key certs/server.key -subj "/CN=127.0.0.1" -out certs/server.csr
openssl x509 -req -in certs/server.csr -CA certs/ca.crt -CAkey certs/ca.key -CAcreateserial \
  -out certs/server.crt -days 365 -sha256
```

Generate a client certificate:

```bash
openssl genrsa -out certs/client.key 2048
openssl req -new -key certs/client.key -subj "/CN=mod18-client" -out certs/client.csr
openssl x509 -req -in certs/client.csr -CA certs/ca.crt -CAkey certs/ca.key -CAcreateserial \
  -out certs/client.crt -days 365 -sha256
```

Create a small HTTPS server that requires client certs:

```bash
cat > server.py <<'EOF'
import json
import ssl
from http.server import BaseHTTPRequestHandler, HTTPServer

class H(BaseHTTPRequestHandler):
    def do_GET(self):
        if self.path != "/healthz":
            self.send_response(404)
            self.end_headers()
            self.wfile.write(b"not found")
            return
        peer = None
        if hasattr(self.connection, "getpeercert"):
            peer = self.connection.getpeercert()
        self.send_response(200)
        self.send_header("Content-Type", "application/json")
        self.end_headers()
        self.wfile.write(json.dumps({"ok": True, "peer_cert_subject": peer.get("subject") if peer else None}).encode("utf-8"))

    def log_message(self, format, *args):
        return

def main():
    httpd = HTTPServer(("127.0.0.1", 18443), H)
    ctx = ssl.create_default_context(ssl.Purpose.CLIENT_AUTH)
    ctx.load_cert_chain(certfile="certs/server.crt", keyfile="certs/server.key")
    ctx.load_verify_locations(cafile="certs/ca.crt")
    ctx.verify_mode = ssl.CERT_REQUIRED
    httpd.socket = ctx.wrap_socket(httpd.socket, server_side=True)
    httpd.serve_forever()

if __name__ == "__main__":
    main()
EOF
```

## Steps

### 1) Start the mTLS Server

```bash
python3 server.py
```

### 2) Successful Call With Client Certificate

In another terminal:

```bash
curl -fsS --cacert certs/ca.crt --cert certs/client.crt --key certs/client.key https://127.0.0.1:18443/healthz
```

Expected signals:

- HTTP 200
- JSON response includes a non-null `peer_cert_subject`

### 3) Failure: Missing Client Certificate

```bash
curl -v --cacert certs/ca.crt https://127.0.0.1:18443/healthz 2>&1 | head -n 40 || true
```

Expected:

- TLS handshake fails (server requires a client cert)

### 4) Failure: Wrong CA Trust

Create a different CA (simulating a mismatched trust bundle):

```bash
openssl genrsa -out certs/wrong-ca.key 2048
openssl req -x509 -new -nodes -key certs/wrong-ca.key -sha256 -days 365 \
  -subj "/CN=wrong-ca" \
  -out certs/wrong-ca.crt
curl -v --cacert certs/wrong-ca.crt --cert certs/client.crt --key certs/client.key https://127.0.0.1:18443/healthz 2>&1 | head -n 60 || true
```

Expected:

- client rejects server certificate (unknown CA)

### 5) Recovery (Correct Trust + Correct Client Identity)

```bash
curl -fsS --cacert certs/ca.crt --cert certs/client.crt --key certs/client.key https://127.0.0.1:18443/healthz
```

## Verify

```bash
openssl x509 -in certs/server.crt -noout -subject -issuer -dates
openssl x509 -in certs/client.crt -noout -subject -issuer -dates
curl -fsS --cacert certs/ca.crt --cert certs/client.crt --key certs/client.key https://127.0.0.1:18443/healthz | grep -q 'peer_cert_subject' && echo "ok: mtls works"
```

Expected signals:

- both certs are issued by `mod18-local-ca`
- the final line prints `ok: mtls works`

## Cleanup

Stop the server (Ctrl+C), then:

```bash
cd ~
rm -rf ~/work/devops-labs/mod18-mtls
```

## Troubleshooting

### Symptom: curl says certificate verify failed

Fix:

- ensure you use `--cacert certs/ca.crt` (correct trust bundle)
- ensure server cert CN/SAN matches the host you call (`127.0.0.1` in this lab)

### Symptom: handshake fails even with client cert

Fix:

- confirm server is running and listening:

```bash
ss -tulpn | grep -E ':(18443)\b' || true
```

- confirm client cert is issued by the server’s CA
