# 12 — Vault + Terraform

## 🎯 Goal

Connect my Terraform knowledge to Vault. There are **two directions**:

| Direction | What it means |
|---|---|
| **Terraform → configures Vault** | Use Terraform to create mounts, policies, auth methods (Vault config as code) |
| **Vault → feeds Terraform** | Terraform pulls secrets/credentials from Vault instead of hardcoding them |

---

## 1️⃣ Terraform configures Vault

Same workflow I already know: `init` → `plan` → `apply`.

```hcl
terraform {
  required_providers {
    vault = {
      source = "hashicorp/vault"
    }
  }
}

# Reads VAULT_ADDR and VAULT_TOKEN from env vars
provider "vault" {}

# A KV v2 mount
resource "vault_mount" "app" {
  path        = "app-secrets"
  type        = "kv"
  options     = { version = "2" }
  description = "Managed by Terraform"
}

# A policy
resource "vault_policy" "app_read" {
  name   = "app-read"
  policy = <<EOT
path "app-secrets/data/*" {
  capabilities = ["read"]
}
EOT
}

# Userpass auth method
resource "vault_auth_backend" "userpass" {
  type = "userpass"
}
```

```bash
terraform init
terraform plan
terraform apply
vault secrets list   # app-secrets/ is there
vault policy list    # app-read is there
```

> 💡 This is how real teams run Vault: **policies and mounts in Git, reviewed in PRs, applied by Terraform.** No clicking around in the UI.

---

## 2️⃣ Vault feeds Terraform: dynamic AWS creds

Remember module 05? Instead of putting long-lived AWS keys on my laptop, Terraform can ask Vault for short-lived ones:

```hcl
provider "vault" {}

data "vault_aws_access_credentials" "creds" {
  backend = "aws"
  role    = "my-ec2-role"
}

provider "aws" {
  region     = "us-east-1"
  access_key = data.vault_aws_access_credentials.creds.access_key
  secret_key = data.vault_aws_access_credentials.creds.secret_key
}
```

Each `terraform apply` gets **fresh, expiring** AWS credentials. Nothing to leak in `~/.aws`.

> ℹ️ Newly created IAM keys can take a few seconds to work in AWS. If the first apply fails with an auth error, retry.

---

## ⚠️ The state file problem

Anything Terraform reads with a normal `data` source **ends up in `terraform.tfstate` in plain text.** Remember: state can contain sensitive info.

Ways to deal with it:
- Keep state in a **secure remote backend** (encrypted S3 bucket, HCP Terraform) with tight access
- Prefer **short-lived dynamic creds** (they're expired by the time anyone reads the state)
- Newer Terraform versions support **ephemeral resources** and **write-only arguments**, which keep secrets **out of state entirely**. Check the Vault provider docs for which ephemeral resources are available in your version: https://registry.terraform.io/providers/hashicorp/vault/latest/docs

---

## 🧠 The HashiCorp story (good for interviews)

| Tool | Job |
|---|---|
| **Terraform** | *Provision* infrastructure |
| **Vault** | *Secure* it: secrets, identity, encryption |
| Together | Infrastructure gets built **without** anyone handling long-lived credentials |

---

## ✍️ Check yourself

1. Name the two ways Terraform and Vault work together.
2. Which Terraform resource creates a Vault policy?
3. Why is reading Vault secrets with a regular `data` source a risk?
4. What Terraform features help keep secrets out of state?

<details>
<summary>Answers</summary>

1. Terraform configures Vault (config as code), and Vault provides secrets/creds to Terraform.
2. `vault_policy`
3. The values get saved in plain text in the state file.
4. Ephemeral resources and write-only arguments (plus a secure remote backend and short-lived creds).

</details>
