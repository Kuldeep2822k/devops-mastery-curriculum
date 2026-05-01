---
title: Modules and Composition
tags:
  - terraform
  - modules
  - composition
module: "08"
---

# Modules and Composition

## Modules Are Boundaries

Modules exist to:

- encapsulate complexity
- create reuse with consistent defaults
- create contracts (inputs/outputs)

Bad modules create hidden coupling and make changes risky.

## Module Design Principles

- keep inputs explicit and minimal
- provide safe defaults
- expose outputs that are operationally useful (IDs, addresses)
- avoid overly generic “kitchen sink” modules

## Composition Patterns

Common structure:

```
terraform/
  envs/
    dev/
    prod/
  modules/
    network/
    service/
```

Environments reference modules and provide configuration:

- envs should be thin
- modules contain implementation

## Workspaces (Use Carefully)

Workspaces can separate state, but:

- they can hide which environment you are targeting
- they can lead to accidental prod changes

Safer for learning and many teams:

- separate directories and separate state backends per env

## Anti-Patterns

- environments encoded as workspaces only, with no directory separation
- modules that mutate global state or rely on implicit provider behavior
- outputting secrets to stdout or logging
