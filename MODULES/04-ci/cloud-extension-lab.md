---
title: Cloud Extension Lab (Optional)
tags:
  - cloud
  - optional
  - ci
module: "04"
---

# Cloud Extension Lab (Optional) — CI Across Providers

Core labs use portable Make targets + GitHub Actions. This extension maps the same pipeline to other CI providers without changing the pipeline contract.

## Goal

Run the same pipeline contract on:

- a hosted CI system (provider of choice)
- with caching and artifact upload
- with environment separation concept (PR vs main vs release)

## Cost Control

- use free-tier minutes where possible
- avoid running large matrices
- delete unused runners and projects

## Steps (High Level)

1. Ensure the repo has a stable `make ci` contract.
2. Configure pipeline steps to run:
   - `make lint`
   - `make test`
   - `make smoke`
3. Configure caching:
   - dependency cache keyed by lockfile hash/tool versions
4. Configure artifacts:
   - upload `dist/`
5. Add a manual approval gate for “release” stage (if supported).

## Verification Signals

- pipeline triggers as expected
- cache hit rate improves over repeated runs without correctness issues
- artifacts downloadable and consistent

## Cleanup

- delete CI projects if not needed
- remove any credentials and revoke tokens
