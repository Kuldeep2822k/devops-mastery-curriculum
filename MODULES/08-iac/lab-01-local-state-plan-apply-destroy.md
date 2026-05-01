---
title: "Lab 01: Local State (init/plan/apply/destroy)"
tags:
  - lab
  - terraform
  - iac
module: "08"
---

# Lab 01 — Local State (init/plan/apply/destroy)

## Goal

Practice the Terraform core loop locally with safe resources:

- init / fmt / validate / plan / apply / destroy
- understand state file behavior
- avoid committing state

This lab uses local-only providers (no cloud resources).

## Prereqs

- Terraform installed: `terraform version`

## Setup

Create a lab directory:

```bash
mkdir -p ~/work/devops-labs/mod08-tf-local
cd ~/work/devops-labs/mod08-tf-local
```

Create `main.tf`:

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

resource "local_file" "hello" {
  filename = "${path.module}/out.txt"
  content  = "hello from terraform\n"
}

output "file_path" {
  value = local_file.hello.filename
}
EOF
```

Create `.gitignore` (do not commit state):

```bash
cat > .gitignore <<'EOF'
.terraform/
.terraform.lock.hcl
terraform.tfstate
terraform.tfstate.backup
out.txt
EOF
```

## Steps

### 1) Initialize

```bash
terraform init
```

Expected signals:

- provider downloaded
- init succeeds

### 2) Format and Validate

```bash
terraform fmt
terraform validate
```

Expected:

- no errors

### 3) Plan

```bash
terraform plan
```

Expected:

- plan shows `local_file.hello` will be created

### 4) Apply

```bash
terraform apply -auto-approve
```

### 5) Destroy

```bash
terraform destroy -auto-approve
```

## Verify

### Verify Apply Results

```bash
cat out.txt
terraform output
ls -la terraform.tfstate
```

Expected signals:

- `out.txt` exists and has content
- `terraform.tfstate` exists

### Verify Destroy Results

```bash
test ! -f out.txt && echo "ok: out.txt removed"
terraform show
```

Expected signals:

- resource removed
- state shows no managed resources

## Cleanup

Remove the directory if desired:

```bash
cd ~
rm -rf ~/work/devops-labs/mod08-tf-local
```

## Troubleshooting

### Symptom: provider download fails

Diagnosis:

- proxy/DNS/TLS issues

Fix:

- confirm your workstation network/proxy configuration
- retry after clearing `.terraform/`

### Symptom: state file contains sensitive data

Interpretation:

- this is normal for many providers

Fix:

- treat state as sensitive and store it in a protected backend in real systems

## Why This Matters in Production

- Understanding state and the core loop prevents “apply and pray”.
- Safe defaults (gitignore, plan review) reduce catastrophic mistakes.

## Definition of Done

- You can run init→plan→apply→destroy locally and explain what state tracks.
