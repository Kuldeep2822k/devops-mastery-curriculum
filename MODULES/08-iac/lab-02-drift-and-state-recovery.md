---
title: "Lab 02: Drift and State Recovery (Local)"
tags:
  - lab
  - terraform
  - drift
  - state
module: "08"
---

# Lab 02 — Drift and State Recovery (Local)

## Goal

Practice:

- detecting drift
- understanding “state vs real world”
- safe state operations (`state show`, `state mv`, `state rm`)

Local-only lab (no cloud resources).

## Prereqs

- Terraform installed

## Setup

```bash
mkdir -p ~/work/devops-labs/mod08-tf-drift
cd ~/work/devops-labs/mod08-tf-drift
```

Create config:

```bash
cat > main.tf <<'EOF'
terraform {
  required_version = ">= 1.5.0"
  required_providers {
    local = {
      source  = "hashicorp/local"
      version = ">= 2.0.0"
    }
  }
}

provider "local" {}

resource "local_file" "a" {
  filename = "${path.module}/a.txt"
  content  = "A\n"
}

output "a_path" {
  value = local_file.a.filename
}
EOF
```

Init and apply:

```bash
terraform init
terraform apply -auto-approve
cat a.txt
```

## Steps

### 1) Simulate Drift

Modify the real file outside Terraform:

```bash
printf "DRIFT\n" > a.txt
cat a.txt
```

Now plan:

```bash
terraform plan
```

Expected signal:

- plan shows `local_file.a` will be updated to restore content to "A\n"

Apply to reconcile:

```bash
terraform apply -auto-approve
cat a.txt
```

Expected:

- file content restored to A

### 2) Practice state show/list

```bash
terraform state list
terraform state show local_file.a | head -n 80
```

### 3) Rename a Resource (state mv)

Update config to rename resource to `local_file.main`:

```bash
perl -0777 -pe 's/resource \"local_file\" \"a\"/resource \"local_file\" \"main\"/g; s/local_file\\.a/local_file.main/g' -i main.tf
terraform fmt
```

If you plan now without state mv, Terraform will want to delete/create. Avoid that by moving state:

```bash
terraform state mv local_file.a local_file.main
terraform plan
```

Expected:

- no destructive changes; state moved

### 4) State rm (Last Resort Pattern)

Simulate “stop managing” without deleting the real file:

```bash
terraform state rm local_file.main
terraform plan
```

Expected:

- plan wants to create resource again (because state no longer tracks it)

Undo by importing is provider-specific; in this lab, treat `state rm` as dangerous.

## Verify

### Verify Drift Detection and Reconciliation

```bash
printf "DRIFT\n" > a.txt
terraform plan
terraform apply -auto-approve
cat a.txt
```

Expected signals:

- plan indicates `local_file` will be updated to restore content
- after apply, `a.txt` content returns to `A`

### Verify Safe Refactor Using state mv

```bash
terraform state list
terraform state mv local_file.a local_file.main
terraform plan
```

Expected signals:

- no destructive delete/create due to rename

## Cleanup

```bash
terraform destroy -auto-approve || true
cd ~
rm -rf ~/work/devops-labs/mod08-tf-drift
```

## Troubleshooting

### Symptom: state mv causes unexpected changes

Diagnosis:

- moved wrong address or config differs

Fix:

- inspect `terraform state list`
- rerun move with correct addresses

### Symptom: drift keeps returning

Interpretation:

- something outside Terraform keeps changing resource (automation/manual edits)

Fix:

- fix the process; drift is a workflow issue

## Why This Matters in Production

- Drift and state mistakes are top causes of IaC outages and fear of Terraform.
- `state mv` is a safe way to refactor without recreating resources when done carefully.

## Definition of Done

- You can detect drift, reconcile it, and refactor a resource name safely using state mv.
