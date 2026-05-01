---
title: Security Tooling (DevSecOps)
tags:
  - setup
  - security
  - supply-chain
---

# Security Tooling (DevSecOps)

## Goal

Install and validate security tooling needed for secure defaults, supply-chain practices, and later pipeline labs. The focus is operational usability: you must be able to run these tools repeatedly and interpret results.

## Principles

- Prefer tools you can automate (CI-friendly).
- Prefer tools that produce machine-readable output.
- Treat security findings like reliability issues: triage, prioritize, prevent recurrence.
- Never print or store secrets in plaintext.

## Baseline Tool Categories

### Secret Scanning (Pre-Commit and CI)

Goal: catch accidental secret commits early.

Verification goal:

- you can scan a repo and see findings without leaking secrets into logs

### Dependency and Vulnerability Scanning

Goal: understand risk in images and dependencies.

Verification goal:

- you can scan an image and interpret severity and exploitability

### SBOM Generation

Goal: produce a software bill of materials for artifacts.

Verification goal:

- you can generate an SBOM and store it alongside build artifacts

### Signing and Provenance (Later Modules)

Goal: sign artifacts and/or attest builds.

Verification goal:

- you can validate signatures and tie artifacts to source inputs

## Install Guidance

Install tools using your OS package manager when available, otherwise use official binaries.

Do not install tools by piping remote scripts to shell without understanding what they do.

## Verification (Generic Pattern)

For each security tool you install, record:

- `tool --version`
- one “scan a known-safe target” execution
- expected output shape and where results are stored

Example verification shape:

```bash
<tool> --version
<tool> scan <target> --output <file>
```

## Troubleshooting

### Symptom: scanners are slow or fail behind proxy

Diagnosis:

- check proxy variables and DNS
- check whether tool needs a custom CA bundle

Fix:

- configure proxy and CA in a way that does not disable TLS validation

### Symptom: too many findings, no prioritization

Fix:

- define a policy: what blocks builds, what creates tickets, what is informational
- start with a small enforced baseline, then raise standards

## Why This Matters in Production

- Secret leaks are high-severity security incidents.
- Vulnerable base images and dependencies are common breach paths.
- SBOM and signing enable faster response during supply-chain events.
