# 09 — Collaborate Using HCP Terraform

[← Prev](08-destroy-infrastructure.md) · [Index](00-index.md) · [Next: Next Steps →](10-next-steps.md)

📘 Official tutorial: [Collaborate using HCP Terraform](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-hcp-terraform)

---

## 🎯 Goal
Move your state off your laptop and run Terraform in **HCP Terraform**, the way teams work.

## 💡 The Idea
So far, state lives in `terraform.tfstate` **on your laptop**. That breaks down for teams:
- Teammates can't see it.
- Two people could apply at the same time and corrupt it.
- Lose your laptop, lose the state.

**HCP Terraform** (formerly *Terraform Cloud*) is HashiCorp's SaaS. It **stores state remotely**, **locks** it during runs, and can **run** `plan`/`apply` for you.

> **Analogy:** Local state is a **notebook in your backpack**. HCP Terraform is a **shared, locked office filing cabinet** with a log of who opened it and when.

### Local vs. HCP Terraform

| | Local | **HCP Terraform** |
|---|---|---|
| State storage | Your laptop | Remote, encrypted |
| Locking | ❌ | ✅ |
| Where runs happen | Your laptop | HCP Terraform's servers (output streams to your terminal) |
| AWS credentials | Your environment | Stored as **sensitive workspace variables** |
| Run history and audit | ❌ | ✅ |
| Team access controls | ❌ | ✅ |

### Key HCP Terraform concepts

| Term | Meaning |
|---|---|
| **Organization** | Your company or team account |
| **Project** | A folder grouping related workspaces |
| **Workspace** | One state + its variables + run history (≈ one environment, e.g., `app-dev`) |
| **Run** | One plan/apply execution |
| **Workspace variables** | Terraform or environment variables stored in HCP Terraform; can be marked **sensitive** |

## 🧪 Do It

### Step 1 — Recreate the infrastructure
You destroyed it in Lesson 08, so rebuild it:
```bash
terraform apply     # type yes
```

### Step 2 — Create an HCP Terraform account
Sign up at **app.terraform.io**, confirm your email, and **create an organization**. Note its exact name.

### Step 3 — Log in from the CLI
```bash
terraform login
```
Type `yes`. A browser opens to create an **API token**. Copy it and paste it into the terminal. It's saved on your computer for future runs.

### Step 4 — Add a `cloud` block to `terraform.tf`
```hcl
terraform {
  cloud {
    organization = "your-organization-name"

    workspaces {
      project = "Learn Terraform"
      name    = "learn-terraform-aws-get-started"
    }
  }

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.92"
    }
  }

  required_version = ">= 1.2"
}
```
Replace `your-organization-name` with yours.

### Step 5 — Migrate state
```bash
terraform init
```
Terraform asks whether to **copy your existing local state to HCP Terraform**. Type `yes`. It creates the project and workspace if they don't exist.

Your local `terraform.tfstate` can now be deleted. **The state lives in HCP Terraform.**

### Step 6 — Give the workspace your AWS credentials
Runs now happen on HCP Terraform's servers, which **don't have your laptop's credentials**. In the workspace → **Variables** → add **environment variables**:

| Key | Value | Sensitive |
|---|---|---|
| `AWS_ACCESS_KEY_ID` | your key ID | ✅ |
| `AWS_SECRET_ACCESS_KEY` | your secret | ✅ |

### Step 7 — Make a change and apply remotely
Uncomment the `aws_instance` in `main.tf` and the output in `outputs.tf` (commented out in Lesson 08). Then:
```bash
terraform apply
```
The plan runs **in HCP Terraform** and streams to your terminal, with a link to view the run in the browser. Type `yes`. The output prints `instance_hostname`.

### Step 8 — Clean up
```bash
terraform destroy    # type yes
```
Then delete the workspace in **Settings → Destruction and Deletion**, and optionally the project.

---

## Remote state options (beyond HCP Terraform)

| Option | How |
|---|---|
| **HCP Terraform** | `cloud {}` block (this lesson) |
| **S3 backend** | `backend "s3" {}` with encryption and locking |
| **Terraform Enterprise** | Self-hosted version of HCP Terraform |

```hcl
# S3 alternative (bucket must already exist)
terraform {
  backend "s3" {
    bucket       = "my-tf-state-bucket"
    key          = "learn-terraform/terraform.tfstate"
    region       = "us-west-2"
    encrypt      = true
    use_lockfile = true
  }
}
```

## 🔑 Key Terms

| Term | Meaning |
|---|---|
| **HCP Terraform** | HashiCorp's SaaS for remote state and runs (formerly Terraform Cloud) |
| **`cloud` block** | Connects a configuration to HCP Terraform |
| **`terraform login`** | Saves an API token so the CLI can use HCP Terraform |
| **Workspace** | A state + variables + run history |
| **State migration** | Moving existing state to a new backend (`terraform init`) |
| **Sensitive variable** | Hidden value, write-only in the UI |

## ⚠️ Watch Out
- **Organization name must match exactly** or `init` fails.
- `No valid credential sources found` during a remote run → the workspace is missing the AWS **environment variables** (Step 6).
- **Don't mark them as "Terraform variables."** AWS keys must be **environment variables**.
- In production, prefer **dynamic credentials** (OIDC between HCP Terraform and AWS, or **Vault**) over long-lived keys.

## 🧠 Recall
1. Three problems with local state for teams?
2. What block connects a project to HCP Terraform?
3. Which command saves your API token?
4. Which command migrates local state, and what do you answer?
5. Why must you add AWS credentials to the workspace?

<details><summary>Answers</summary>

1. Teammates can't access it, no locking (risk of corruption), and it's lost if your laptop is.
2. The `cloud {}` block inside `terraform {}`.
3. `terraform login`
4. `terraform init`; answer `yes` to copy existing state.
5. Runs execute on HCP Terraform's servers, which don't have your local credentials.
</details>

## 📌 Remember
- **Local notebook → shared locked cabinet**
- `terraform login` → `cloud {}` block → `terraform init` (migrate) → add **sensitive env vars**
- Workspace = **state + variables + run history**

---

[← Prev](08-destroy-infrastructure.md) · [Index](00-index.md) · [Next: Next Steps →](10-next-steps.md)
