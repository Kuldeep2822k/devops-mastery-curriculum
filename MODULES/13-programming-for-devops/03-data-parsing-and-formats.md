---
title: Data Parsing and Formats (JSON/YAML/CSV)
tags:
  - programming
  - data
module: "13"
---

# Data Parsing and Formats (JSON/YAML/CSV)

## Prefer Structured Data Over Grep Pipelines

Many operational failures come from:

- brittle regex parsing
- whitespace and formatting changes

Prefer:

- `jq` for JSON (if available)
- Python for parsing and generating structured output

## JSON as an Ops Contract

Benefits:

- stable schemas
- easy to validate and transform
- integrates well with APIs

Operator habit:

- define the schema you emit (fields and meaning)
- version schemas when evolving

## YAML Caveats

YAML is human-friendly but has pitfalls:

- implicit typing surprises
- indentation mistakes

Use it for configs, but treat it carefully.

## Anti-Patterns

- parsing JSON by grep
- logging entire JSON payloads containing secrets
- ad hoc CSV parsing without quoting rules
