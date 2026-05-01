---
title: 'JWT Signing and Validation'
tags:
  - lab
  - identity
  - jwt
module: "22"
---

# Lab 01 — JWT Signing and Validation (RSA + exp/aud/iss)

## Goal

Build a minimal JWT issuing + verification workflow and practice common failure modes:

- generate an RSA keypair
- issue a JWT (header.payload.signature)
- verify signature with a public key
- enforce basic claims (exp, aud, iss)

## Prereqs

- `python3`
- `openssl`

## Setup

```bash
mkdir -p ~/work/devops-labs/mod22-jwt
cd ~/work/devops-labs/mod22-jwt
mkdir -p keys
```

Generate keys:

```bash
openssl genrsa -out keys/private.pem 2048
openssl rsa -in keys/private.pem -pubout -out keys/public.pem
```

Create a JWT tool (issues and verifies using openssl for RSA signature verification):

```bash
cat > jwt_tool.py <<'EOF'
import argparse
import base64
import json
import subprocess
import tempfile
import time

def b64url(data: bytes) -> str:
    return base64.urlsafe_b64encode(data).decode("utf-8").rstrip("=")

def b64url_decode(s: str) -> bytes:
    pad = "=" * ((4 - (len(s) % 4)) % 4)
    return base64.urlsafe_b64decode((s + pad).encode("utf-8"))

def sign_rs256(private_key_path: str, signing_input: bytes) -> bytes:
    p = subprocess.run(
        ["openssl", "dgst", "-sha256", "-sign", private_key_path],
        input=signing_input,
        stdout=subprocess.PIPE,
        stderr=subprocess.PIPE,
        check=True,
    )
    return p.stdout

def verify_rs256(public_key_path: str, signing_input: bytes, signature: bytes) -> bool:
    with tempfile.NamedTemporaryFile() as sigf:
        sigf.write(signature)
        sigf.flush()
        p = subprocess.run(
            ["openssl", "dgst", "-sha256", "-verify", public_key_path, "-signature", sigf.name],
            input=signing_input,
            stdout=subprocess.PIPE,
            stderr=subprocess.PIPE,
            check=False,
        )
        return p.returncode == 0

def main():
    ap = argparse.ArgumentParser()
    sub = ap.add_subparsers(dest="cmd", required=True)

    issue = sub.add_parser("issue")
    issue.add_argument("--private-key", required=True)
    issue.add_argument("--iss", default="mod22-issuer")
    issue.add_argument("--aud", default="mod22-audience")
    issue.add_argument("--sub", default="user-123")
    issue.add_argument("--ttl-s", type=int, default=60)

    verify = sub.add_parser("verify")
    verify.add_argument("--public-key", required=True)
    verify.add_argument("--token", required=True)
    verify.add_argument("--iss", default="mod22-issuer")
    verify.add_argument("--aud", default="mod22-audience")

    args = ap.parse_args()

    if args.cmd == "issue":
        header = {"alg": "RS256", "typ": "JWT"}
        now = int(time.time())
        payload = {"iss": args.iss, "aud": args.aud, "sub": args.sub, "iat": now, "exp": now + args.ttl_s}
        h = b64url(json.dumps(header, separators=(",", ":"), sort_keys=True).encode("utf-8"))
        p = b64url(json.dumps(payload, separators=(",", ":"), sort_keys=True).encode("utf-8"))
        signing_input = f"{h}.{p}".encode("utf-8")
        sig = sign_rs256(args.private_key, signing_input)
        token = f"{h}.{p}.{b64url(sig)}"
        print(token)
        return 0

    if args.cmd == "verify":
        parts = args.token.split(".")
        if len(parts) != 3:
            raise SystemExit("bad_token_format")
        h_b, p_b, s_b = parts
        signing_input = f"{h_b}.{p_b}".encode("utf-8")
        signature = b64url_decode(s_b)

        header = json.loads(b64url_decode(h_b))
        payload = json.loads(b64url_decode(p_b))

        if header.get("alg") != "RS256":
            raise SystemExit("unsupported_alg")

        if payload.get("iss") != args.iss:
            raise SystemExit("bad_iss")
        if payload.get("aud") != args.aud:
            raise SystemExit("bad_aud")

        now = int(time.time())
        exp = int(payload.get("exp", 0))
        if now >= exp:
            raise SystemExit("expired")

        ok = verify_rs256(args.public_key, signing_input, signature)
        if not ok:
            raise SystemExit("bad_signature")
        print("ok")
        return 0

if __name__ == "__main__":
    raise SystemExit(main())
EOF
```

## Verify

## Steps

### 1) Issue a Token

```bash
token="$(python3 jwt_tool.py issue --private-key keys/private.pem --iss mod22-issuer --aud mod22-audience --sub user-1 --ttl-s 30)"
printf "%s\n" "$token" > token.jwt
wc -c token.jwt
```

### 2) Verify Token (Signature + Claims)

```bash
python3 jwt_tool.py verify --public-key keys/public.pem --token "$token" --iss mod22-issuer --aud mod22-audience
```

Expected:

- prints `ok`

### 3) Failure: Tamper Payload

```bash
python3 - <<'PY'
import base64, json
tok = open("token.jwt","r",encoding="utf-8").read().strip()
h,p,s = tok.split(".")
pad = "=" * ((4 - (len(p) % 4)) % 4)
payload = json.loads(base64.urlsafe_b64decode((p+pad).encode()))
payload["sub"] = "attacker"
newp = base64.urlsafe_b64encode(json.dumps(payload, separators=(",",":"), sort_keys=True).encode()).decode().rstrip("=")
print(h + "." + newp + "." + s)
PY > tampered.jwt
python3 jwt_tool.py verify --public-key keys/public.pem --token "$(cat tampered.jwt)" --iss mod22-issuer --aud mod22-audience || true
```

Expected:

- fails with `bad_signature`

### 4) Failure: Expired Token

```bash
short="$(python3 jwt_tool.py issue --private-key keys/private.pem --iss mod22-issuer --aud mod22-audience --sub user-1 --ttl-s 1)"
sleep 2
python3 jwt_tool.py verify --public-key keys/public.pem --token "$short" --iss mod22-issuer --aud mod22-audience || true
```

Expected:

- fails with `expired`

## Verify

```bash
python3 jwt_tool.py verify --public-key keys/public.pem --token "$(cat token.jwt)" --iss mod22-issuer --aud mod22-audience
```

Expected signals:

- prints `ok`

## Cleanup

```bash
cd ~
rm -rf ~/work/devops-labs/mod22-jwt
```

## Troubleshooting

### Symptom: verify fails with bad_signature unexpectedly

Fix:

- ensure you are verifying the exact token you issued (no whitespace/newlines)
- ensure you are using the matching public key
