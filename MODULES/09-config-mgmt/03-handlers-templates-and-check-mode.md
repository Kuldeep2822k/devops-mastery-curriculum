---
title: Handlers, Templates, and Check Mode
tags:
  - ansible
  - handlers
  - templates
module: "09"
---

# Handlers, Templates, and Check Mode

## Templates (Jinja2)

Templates are how you:

- manage config files declaratively
- inject environment-specific values safely

Operator habit:

- always validate config syntax before restarting services

## Handlers

Handlers run only when notified.

Use them for:

- service restarts
- reloads

This prevents unnecessary disruption.

## Check Mode and Diff

Use:

- `--check` to preview changes
- `--diff` to see config diffs (be careful with secrets)

Workflow:

1) `ansible-playbook --check --diff ...`  
2) review intended changes  
3) `ansible-playbook ...`  

## Anti-Patterns

- restarting on every run
- templating secret values into files without controls
- using check-mode as a substitute for real verification
