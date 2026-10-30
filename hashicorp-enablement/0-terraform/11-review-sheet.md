# 11 — Review Sheet & Flashcards

[← Prev](10-aws-project.md) · [Index](00-index.md)

---

Use this page for your spaced reviews (next day → 1 week → 1 month).
**Cover the answers, say them out loud, then check.**

## The big picture on one screen

```
  WRITE .tf files  ──▶  terraform init  ──▶  terraform plan  ──▶  terraform apply
   (HCL, declarative)    (get providers)      (preview diff)       (build + update state)
                                                                          │
                                       terraform destroy  ◀───── when done ┘
```

| Concept | One-line definition | Analogy |
|---|---|---|
| IaC | Infrastructure written as code | Blueprint |
| Declarative | Say *what*, not *how* | Ordering at a restaurant |
| Provider | Plugin that talks to a platform's API | Translator |
| Resource | One piece of infrastructure | The thing being built |
| State | Record of what Terraform built | Inventory list |
| Drift | Reality changed outside Terraform | Someone rearranged the furniture |
| Variable / Output / Local | In / out / inside | Form / receipt / sticky note |
| Module | Reusable folder of code | Recipe |

## Syntax at a glance

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = var.region
}

variable "region" {
  type    = string
  default = "us-east-1"
}

locals {
  app_name = "my-app"
}

resource "aws_s3_bucket" "files" {
  bucket = "${local.app_name}-files-123"
}

module "web" {
  source = "./modules/web"
}

output "bucket_name" {
  value = aws_s3_bucket.files.bucket
}
```

**How to reference things**

| You want | Write |
|---|---|
| A variable | `var.NAME` |
| A local | `local.NAME` |
| A resource's value | `TYPE.NAME.ATTRIBUTE` |
| A module's output | `module.NAME.OUTPUT` |
| This folder | `path.module` |
| Current copy number | `count.index` |
| Current item | `each.key` |

## Commands

| Command | Does |
|---|---|
| `terraform init` | Set up folder, download providers |
| `terraform fmt` | Tidy code |
| `terraform validate` | Check for mistakes |
| `terraform plan` | Preview (safe) |
| `terraform apply` | Build |
| `terraform destroy` | Delete everything |
| `terraform output` | Show outputs |
| `terraform state list` | List what's managed |

**Plan symbols:** `+` create · `~` update · `-` destroy · `-/+` replace

**Variable precedence (strongest first):** `-var` → `*.auto.tfvars` → `terraform.tfvars` → `TF_VAR_` → default

## Golden rules

1. Always read the **plan** before typing `yes`.
2. **Never** commit `terraform.tfstate` or secrets to Git.
3. Use a **remote backend** (like S3) for real projects.
4. Don't change Terraform-managed things **by hand** (that's drift).
5. Prefer **`for_each`** over `count` for named things.
6. **Pin versions** of providers and modules.
7. **`terraform destroy`** after practicing on the cloud.

## `.gitignore` for every Terraform repo

```gitignore
.terraform/
*.tfstate
*.tfstate.*
*.tfplan
crash.log
```
(Do commit `.terraform.lock.hcl`.)

---

## 🧠 Flashcards

<details><summary>1. What problem does IaC solve?</summary>Manual, slow, error-prone setup and configuration drift between environments.</details>

<details><summary>2. What does "declarative" mean?</summary>You describe the end result; Terraform figures out the steps.</details>

<details><summary>3. What are the 3 workflow steps?</summary>Write → Plan → Apply.</details>

<details><summary>4. What is a provider?</summary>A plugin that lets Terraform talk to a platform's API (AWS, Azure, local files…).</details>

<details><summary>5. Write the resource pattern.</summary><code>resource "TYPE" "NAME" { argument = value }</code></details>

<details><summary>6. How do you reference a resource's attribute?</summary><code>TYPE.NAME.ATTRIBUTE</code> — e.g. <code>aws_instance.web.public_ip</code></details>

<details><summary>7. What does terraform init do?</summary>Sets up the folder and downloads providers and modules.</details>

<details><summary>8. Does terraform plan change anything?</summary>No — it's only a preview.</details>

<details><summary>9. What does -/+ mean in a plan?</summary>The resource will be destroyed and recreated.</details>

<details><summary>10. What is the state file?</summary>Terraform's record that maps your code to the real resources it built.</details>

<details><summary>11. Why never commit state to Git?</summary>It can contain secrets in plain text.</details>

<details><summary>12. Why use a remote backend?</summary>Team sharing, locking, encryption and backups.</details>

<details><summary>13. What is drift?</summary>Real infrastructure changed outside Terraform so it no longer matches the code.</details>

<details><summary>14. Default "a", tfvars "b", -var "c" — which wins?</summary>"c" — the command line is strongest.</details>

<details><summary>15. How do you set a variable with an environment variable?</summary><code>export TF_VAR_name="value"</code></details>

<details><summary>16. Variable vs local?</summary>Variables are inputs that can change from outside; locals are internal calculated values.</details>

<details><summary>17. Does sensitive = true remove a value from state?</summary>No — it only hides it on screen.</details>

<details><summary>18. count vs for_each?</summary>count = N numbered copies; for_each = one per named item. Prefer for_each for named things because removing one doesn't shift the others.</details>

<details><summary>19. When do you need depends_on?</summary>Only for hidden dependencies Terraform can't see from references.</details>

<details><summary>20. What is a module?</summary>A reusable folder of Terraform code — inputs (variables), steps (resources), outputs.</details>

<details><summary>21. How do you read a module's output?</summary><code>module.MODULE_NAME.OUTPUT_NAME</code></details>

<details><summary>22. What protects a resource from being deleted?</summary><code>lifecycle { prevent_destroy = true }</code></details>

<details><summary>23. In the AWS project, why is RDS built before EC2?</summary>EC2's startup script references RDS values, so Terraform builds RDS first.</details>

<details><summary>24. What's the last thing you do after a cloud practice session?</summary><code>terraform destroy</code></details>

---

## 🏁 Final challenge (do this at the 1-month review)

Without looking at your notes:
1. Create a folder with a `local_file` resource that uses a **variable** for the filename.
2. Make 3 files using **`for_each`**.
3. Add an **output** listing the file paths.
4. Move the resource into a **module** and call it from the root.
5. `init`, `plan`, `apply`, `destroy`.

If you can do this, you've got the fundamentals. 🎉

---

[← Prev](10-aws-project.md) · [Index](00-index.md)
