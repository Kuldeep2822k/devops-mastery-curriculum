---
title: Practical Exam
tags:
  - exam
  - networking
module: "15"
---

# Practical Exam — Module 15 Networking Protocols

## Rules

- Time-box: 120 minutes.
- No internet (except to reach a public host you choose for TLS tests).
- Submit evidence: commands + outputs + reasoning.

## Tasks

### Task 1: Failure Classification Drill

Requirements:

- produce one refused example, one timeout example, one TLS failure example (if possible)
- explain what each implies and next steps

Grading criteria:

- correct classification and evidence-based reasoning

### Task 2: DNS Debug

Requirements:

- resolve a hostname and explain caching/TTL
- distinguish resolve vs connect failure

Grading criteria:

- uses dig/nslookup and /etc/resolv.conf reasoning

### Task 3: TLS Debug

Requirements:

- use openssl s_client to inspect handshake and certificate
- explain common failure causes and safe fixes

Grading criteria:

- does not disable verification as fix

### Task 4: Runbook + ADR

Requirements:

- Runbook: “service unreachable” that includes DNS, connect, TLS, and HTTP checks.
- ADR: choose timeout/retry policy and justify tradeoffs.
