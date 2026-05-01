---
title: "Module 14: Web Proxy"
tags:
  - module
  - web-proxy
  - nginx
  - load-balancing
module: "14"
---

# Module 14 — Web Proxy (Reverse Proxies and Load Balancers)

## Outcomes

You can:

- Explain reverse proxy responsibilities: routing, TLS termination, buffering, timeouts, retries.
- Configure a basic reverse proxy safely (headers, health checks, upstreams).
- Diagnose common production failures: 502/503/504, header issues, websocket upgrades, timeouts, connection reuse.
- Understand proxy-related security edges: host header injection, auth forwarding, request smuggling basics.
- Write runbooks and ADRs for proxy policies (timeouts, buffering, header forwarding).

## Prereqs

- Linux networking basics: [Module 02](../02-linux/00-overview.md)
- Delivery basics: [Module 07](../07-delivery/00-overview.md)

## Module Map

- Concepts:
  - [01-reverse-proxy-mental-model.md](01-reverse-proxy-mental-model.md)
  - [02-timeouts-buffering-and-retries.md](02-timeouts-buffering-and-retries.md)
  - [03-headers-auth-and-client-ip.md](03-headers-auth-and-client-ip.md)
  - [04-troubleshooting-502-503-504.md](04-troubleshooting-502-503-504.md)
- Deep dives:
  - [deep-dive-01.md](deep-dive-01.md)
  - [deep-dive-02.md](deep-dive-02.md)
- Labs:
  - [lab-01-nginx-reverse-proxy-local.md](lab-01-nginx-reverse-proxy-local.md)
  - [lab-02-break-fix-timeouts-and-headers.md](lab-02-break-fix-timeouts-and-headers.md)
- Cloud extension:
  - [cloud-extension-lab.md](cloud-extension-lab.md)
- Assessment and practice:
  - [checklist.md](checklist.md)
  - [rubric.md](rubric.md)
  - [review-questions.md](review-questions.md)
  - [exam.md](exam.md)
  - [common-mistakes.md](common-mistakes.md)
  - [troubleshooting.md](troubleshooting.md)
  - [troubleshooting-lab.md](troubleshooting-lab.md)
- Writing templates:
  - [runbook-template.md](runbook-template.md)
  - [decision-record-template.md](decision-record-template.md)
