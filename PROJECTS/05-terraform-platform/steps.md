---
title: '05-terraform-platform: Steps'
tags:
  - project
---

# 05-terraform-platform — Steps

## Prereqs

- Terraform (or OpenTofu)

## Setup

```bash
mkdir -p ~/work/devops-labs/05-terraform-platform
cd ~/work/devops-labs/05-terraform-platform
```

Create Terraform config (local provider):

```bash
cat > main.tf <<'EOF'
terraform {
  required_providers {
    local = {
      source = "hashicorp/local"
      version = "~> 2.5"
    }
  }
}

provider "local" {}

resource "local_file" "app" {
  filename = "${path.module}/app.conf"
  content  = "version=1
"
}
EOF
terraform init
```

## Steps

### 1) Apply and Verify State

```bash
terraform apply -auto-approve
cat app.conf
terraform state list
```

### 2) Drift Drill

```bash
printf 'version=DRIFT
' > app.conf
terraform plan | tee plan.txt
```

Recover:

```bash
terraform apply -auto-approve
cat app.conf
```

### 3) Safe Refactor (Avoid Replacement)

Rename resource in config:

```bash
perl -pi -e 's/local_file" "app/local_file" "main/' main.tf
terraform plan || true
terraform state mv local_file.app local_file.main
terraform plan
```

Expected:

- after state mv, plan is non-destructive

## Verify

```bash
terraform plan
cat app.conf | grep -q '^version=1$'
```

## Cleanup

```bash
terraform destroy -auto-approve || true
cd ~
rm -rf ~/work/devops-labs/05-terraform-platform
```

## Troubleshooting

- If provider download fails, verify network access and retry `terraform init`.
