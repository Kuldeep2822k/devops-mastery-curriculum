---
title: TCP/UDP Basics and Failure Patterns
tags:
  - networking
  - tcp
  - udp
module: "15"
---

# TCP/UDP Basics and Failure Patterns

## TCP (Connection-Oriented)

TCP provides:

- connection setup (3-way handshake)
- ordered delivery
- congestion control

Common failure signals:

- connection refused: no listener or actively rejected
- timeout: packet drop or firewall block
- reset: connection aborted (proxy/upstream crash, middlebox behavior)

Operator commands:

- `ss -tulpn`
- `curl -v`
- `nc -vz host port`

## UDP (Connectionless)

UDP provides:

- no built-in connection state
- no delivery guarantees

Failure patterns:

- timeouts are common because there is no handshake
- application-level retries are required

Operator habit:

- verify whether protocol is UDP or TCP before diagnosing “port open”

## Interpreting “Refused vs Timeout”

- refused usually means local reachability but service not listening or rejecting
- timeout usually means routing/firewall drops or upstream hang

This distinction often isolates the layer quickly.
