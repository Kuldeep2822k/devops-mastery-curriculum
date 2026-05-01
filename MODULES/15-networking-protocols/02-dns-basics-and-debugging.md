---
title: DNS Basics and Debugging
tags:
  - networking
  - dns
module: "15"
---

# DNS Basics and Debugging

## What DNS Does

DNS maps names to records:

- A/AAAA: IP addresses
- CNAME: alias
- TXT: metadata
- SRV: service location

## Common DNS Failure Patterns

- NXDOMAIN: name does not exist
- SERVFAIL: resolver failure (upstream or configuration)
- slow resolution: timeouts and retries
- split-horizon: internal vs external resolution differs

## TTL and Caching

Caching causes:

- changes to take time to propagate
- “works for me” due to different cache state

## Operator Debug Commands

- `dig <name> +short`
- `dig <name> +trace` (when allowed)
- `nslookup <name>`
- check local resolver config: `cat /etc/resolv.conf`

## Operator Habits

- test resolution from the same network environment as the failing workload
- distinguish DNS failure from connectivity failure (resolve vs connect)
