---
title: Troubleshooting Guide
tags:
  - troubleshooting
  - networking
module: "15"
---

# Troubleshooting — Module 15 Networking Protocols

## Fast Triage Playbook (Copyable)

1) Listener:

```bash
ss -tulpn
```

2) DNS:

```bash
dig <name> +short || nslookup <name>
cat /etc/resolv.conf
```

3) Connect:

```bash
nc -vz <host> <port> || true
curl -m 3 -v http://<host>:<port>/ 2>&1 | head -n 40 || true
```

4) TLS:

```bash
openssl s_client -connect <host>:443 -servername <host> </dev/null | head -n 80
```

5) HTTP:

```bash
curl -m 5 -v https://<host>/ 2>&1 | head -n 80
```

## Symptom Classification

- refused: no listener or active reject
- timeout: routing/firewall drop or severe upstream slowness
- reset: connection aborted midstream
- TLS error: cert/hostname/time/protocol mismatch

## Containment

- rollback recent changes
- reduce retries and load
- route traffic away from failing dependency
