---
title: 'Security + Supply Chain Incidents'
tags:
  - catalog
  - security
  - identity
  - supply-chain
  - incident
---

# Security + Supply Chain Incidents — Troubleshooting Scenarios

Use this when auth breaks, secrets leak, builds are compromised, or policy gates block releases.

## Scenarios

### Scenario 01 — Sudden spike in 401/403 after deploy

- Symptoms: Auth failures for many users/services immediately after release.
- Constraints: Avoid disabling auth globally as a quick fix.
- Diagnosis commands:
  - Compare deploy time vs error spike.
  - Inspect auth middleware logs for reasons (token invalid, audience mismatch).
- Root cause: Misconfigured issuer/audience, wrong JWKS URL, clock skew.
- Fix: Roll back; fix config; validate OIDC settings.
- Verify: 401/403 rate returns to baseline; requests succeed.
- Prevention: Config validation tests; canary; time sync monitoring.

### Scenario 02 — Token validation failures after key rotation

- Symptoms: “kid not found”, “signature invalid”, intermittent auth.
- Diagnosis commands:
  - Fetch JWKS and confirm expected `kid`.
  - Check cache TTLs for JWKS.
- Root cause: Stale JWKS cache, rotation without overlap.
- Fix: Reduce cache TTL; ensure overlap period; trigger cache refresh.
- Verify: All services accept tokens consistently.
- Prevention: Rotation runbook, staged rollout, monitoring.

### Scenario 03 — RBAC denies in Kubernetes after policy change

- Symptoms: `Forbidden` errors; controllers fail.
- Diagnosis commands:
  - `kubectl auth can-i <verb> <resource> --as=<sa>`
  - `kubectl -n <ns> describe rolebinding <rb> || true`
- Root cause: Over-tightened Role/ClusterRole, missing verb/resource.
- Fix: Restore least-privilege policy with required verbs; roll back unsafe change.
- Verify: Controllers recover; forbidden errors stop.
- Prevention: RBAC tests; policy PR review; least-privilege rubric.

### Scenario 04 — Secret accidentally printed to logs

- Symptoms: Secret appears in CI logs or application logs.
- Constraints: Treat as incident; do not copy secret further.
- Diagnosis commands:
  - Identify where logs are stored and who has access.
  - Locate commit/job that printed it.
- Root cause: Debug logging, `set -x`, printing env vars, unsafe scripts.
- Fix: Rotate secret; revoke tokens; scrub logs if policy allows; patch pipeline/app.
- Verify: New secret works; leak vector removed.
- Prevention: Secret scanning, redaction, lint rules, “no echo secrets” policy.

### Scenario 05 — Build provenance missing / unsigned artifacts rejected

- Symptoms: Release gate blocks; “signature missing” / “attestation missing”.
- Diagnosis commands:
  - Confirm what metadata is required (SBOM, signature, provenance).
  - Inspect pipeline steps for signing/attestation.
- Root cause: Skipped step, missing permissions, wrong keyless identity.
- Fix: Restore signing/attestation step; re-run build; promote by digest.
- Verify: Gate passes with expected metadata.
- Prevention: Required checks in CI; template pipeline.

### Scenario 06 — Dependency supply-chain incident (malicious package)

- Symptoms: Security advisory; suspicious outbound traffic after update.
- Constraints: Minimize blast radius; preserve forensics.
- Diagnosis commands:
  - Identify introduced dependency versions.
  - Review SBOM (if available) and lockfiles.
- Root cause: Compromised dependency or typo-squatting.
- Fix: Pin safe version; rollback; block package; rotate credentials if suspected exfil.
- Verify: No suspicious traffic; builds use pinned version.
- Prevention: Allowlist, lockfiles, SBOM + signing, review gates.
