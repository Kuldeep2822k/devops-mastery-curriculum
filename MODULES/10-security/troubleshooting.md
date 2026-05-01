---
title: Troubleshooting Guide
tags:
  - troubleshooting
  - security
module: "10"
---

# Troubleshooting — Module 10 Security

## Fast Classification

- Secret leak?
- Unauthorized access?
- Suspicious CI behavior?
- Dependency/supply-chain concern?
- Misconfiguration exposure (public port, permissive policy)?

## Secret Leak Response (Checklist)

1) assume compromise  
2) revoke/rotate secret  
3) remove secret from code/config  
4) search for other exposures (logs, artifacts, forks)  
5) add guardrails to prevent recurrence  

## CI Suspicion Response (Checklist)

- stop releases/deploys temporarily (containment)
- review recent changes and PRs
- rotate CI tokens if exposure possible
- verify artifact identities and provenance metadata

## Supply Chain Incident Response (Checklist)

- identify affected versions (dependency manifest / lockfile)
- assess exploitability and exposure
- roll back to known-good versions if needed
- patch and redeploy with pinned versions

## Useful Local Commands

Search for obvious secret patterns:

```bash
grep -R -n -E '(AKIA[0-9A-Z]{16})|(BEGIN (RSA|EC|OPENSSH) PRIVATE KEY)|(password\\s*=)|(token\\s*=)' . | head -n 50 || true
```

Find Dockerfiles using :latest:

```bash
grep -R -n -E '^FROM .+:latest\\b' . || true
```
