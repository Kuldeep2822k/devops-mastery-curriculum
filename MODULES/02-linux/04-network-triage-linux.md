---
title: Network Triage on Linux
tags:
  - linux
  - networking
  - triage
module: "02"
---

# Network Triage on Linux

## Operator Goal

Answer quickly:

- is the service listening?
- is it reachable from where it should be reachable?
- is DNS resolving correctly?
- are connections being accepted/refused/timing out?

## Sockets and Listeners

```bash
ss -tulpn
ss -tn state established | head
```

Common interpretations:

- LISTEN exists: process is bound
- no LISTEN: service not bound or bound to a different interface/port

## Quick Connectivity Checks

From the same host:

```bash
curl -v http://localhost:<port>/
nc -vz 127.0.0.1 <port>
```

DNS:

```bash
dig <name> +short || nslookup <name>
```

TLS inspection:

```bash
openssl s_client -connect <host>:443 -servername <host>
```

## “Connection Refused” vs “Timeout”

- refused: target reachable, no listener or firewall rejects
- timeout: path blocked, packet drop, routing issue, service hung, or security group/firewall

## Anti-Patterns

- assuming “network issue” without checking `ss` and local curl first
- disabling TLS verification to “make it work”
- ignoring DNS and thinking only about IP reachability
