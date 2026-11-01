# 11 — Review Sheet & Flashcards

[← Prev](10-next-steps.md) · [Index](00-index.md)

---

Use this for spaced review and **before any interview**. Cover the answers, say them out loud, then check.

## The whole course on one screen

```
 WRITE .tf  ──▶  init  ──▶  fmt / validate  ──▶  plan  ──▶  apply  ──▶ (change code, repeat) ──▶  destroy
 (HCL)          (get        (tidy / check)       (preview)  (build +
                providers,                                   save state)
                modules)
```

| Concept | One line | Analogy |
|---|---|---|
| IaC | Infrastructure written as code | Blueprint |
| Declarative | Say *what*, not *how* | Restaurant order |
| Provider | Plugin that talks to AWS | Translator |
| Resource | Something Terraform builds | The room being built |
| Data source | Looks up something that exists | Checking a catalog |
| State | Record of what Terraform built | Inventory list |
| Variable / output / local | In / out / inside | Form / receipt / sticky note |
| Module | Reusable package of code | Recipe |
| HCP Terraform | Remote state + runs for teams | Shared locked filing cabinet |

## The final project code

**`terraform.tf`**
```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.92"
    }
  }

  required_version = ">= 1.2"
}
```

**`main.tf`**
```hcl
provider "aws" {
  region = "us-west-2"
}

data "aws_ami" "ubuntu" {
  most_recent = true

  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd-gp3/ubuntu-noble-24.04-arm64-server-*"]
  }

  owners = ["099720109477"] # Canonical
}

module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "5.19.0"

  name = "example-vpc"
  cidr = "10.0.0.0/16"

  azs             = ["us-west-2a", "us-west-2b", "us-west-2c"]
  private_subnets = ["10.0.1.0/24", "10.0.2.0/24"]
  public_subnets  = ["10.0.101.0/24"]

  enable_dns_hostnames = true
}

resource "aws_instance" "app_server" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = var.instance_type

  vpc_security_group_ids = [module.vpc.default_security_group_id]
  subnet_id              = module.vpc.private_subnets[0]

  tags = {
    Name = var.instance_name
  }
}
```

**`variables.tf`**
```hcl
variable "instance_name" {
  description = "Value of the EC2 instance's Name tag."
  type        = string
  default     = "ExampleAppServerInstance"
}

variable "instance_type" {
  description = "The EC2 instance's type."
  type        = string
  default     = "t4g.nano"
}
```

**`outputs.tf`**
```hcl
output "instance_hostname" {
  description = "Private DNS name of the EC2 instance."
  value       = aws_instance.app_server.private_dns
}
```

## References

| You want | Write |
|---|---|
| A variable | `var.instance_type` |
| A local | `local.common_tags` |
| A resource attribute | `aws_instance.app_server.private_dns` |
| A data source attribute | `data.aws_ami.ubuntu.id` |
| A module output | `module.vpc.private_subnets[0]` |

## Commands

| Command | Does |
|---|---|
| `terraform init` | Download providers/modules, set up backend |
| `terraform fmt` | Tidy formatting |
| `terraform validate` | Check for mistakes |
| `terraform plan` | Preview (safe) |
| `terraform apply` | Build/change |
| `terraform destroy` | Delete everything |
| `terraform output` | Show outputs |
| `terraform state list` / `show` | Inspect state |
| `terraform login` | Connect CLI to HCP Terraform |

**Plan symbols:** `+` create · `~` update · `-` destroy · `-/+` replace

**Variable precedence (strongest first):** `-var` → `*.auto.tfvars` → `terraform.tfvars` → `TF_VAR_` → default

## Golden rules
1. Read every **plan** before typing `yes`.
2. **Never** commit state, `.terraform/` or secrets. **Do** commit `.terraform.lock.hcl`.
3. **Remote state** (HCP Terraform / S3) for real work.
4. **No manual console changes** to managed resources.
5. **Pin** provider and module versions.
6. **`terraform destroy`** after every practice session.

## 🗣️ Explain it in 30 seconds (interview)
> "Terraform is HashiCorp's infrastructure-as-code tool. You declare the infrastructure you want in HCL, for example a VPC and an EC2 instance on AWS. Terraform uses providers to call the cloud APIs, compares your code against its state file to build a plan, and on apply makes only the changes needed. Because it's code, infrastructure becomes repeatable, reviewable in Git and reusable through modules. HCP Terraform adds remote state, locking and team workflows on top."

---

## 🧠 Flashcards

<details><summary>1. What is IaC?</summary>Managing infrastructure with code files instead of clicking in a console.</details>

<details><summary>2. What does "declarative" mean?</summary>You describe the end result; Terraform figures out the steps.</details>

<details><summary>3. What goes in the <code>terraform {}</code> block?</summary>Settings: required providers and versions, required Terraform version, and the backend/cloud block.</details>

<details><summary>4. What does <code>~> 5.92</code> mean?</summary>Any 5.x version from 5.92 up, but not 6.0.</details>

<details><summary>5. What does the <code>provider "aws"</code> block set in the tutorial?</summary>The region (<code>us-west-2</code>). Credentials come from the environment.</details>

<details><summary>6. Data source vs. resource?</summary>A data source reads something that exists; a resource creates and manages something.</details>

<details><summary>7. Why look up the AMI with a data source?</summary>AMI IDs vary by region and change over time; the lookup always gets the latest.</details>

<details><summary>8. Write the resource pattern.</summary><code>resource "TYPE" "NAME" { argument = value }</code></details>

<details><summary>9. What does <code>terraform init</code> do, and when do you run it?</summary>Downloads providers and modules and sets up the backend. Run it first and after adding a provider, module or backend.</details>

<details><summary>10. What's in <code>.terraform.lock.hcl</code>? Commit it?</summary>Exact provider versions and checksums. Yes, commit it.</details>

<details><summary>11. What is state, and why never commit it?</summary>Terraform's record of what it built. It can contain secrets in plain text.</details>

<details><summary>12. Which command lists resources in state?</summary><code>terraform state list</code></details>

<details><summary>13. What is drift?</summary>Real infrastructure changed outside Terraform so it no longer matches the code.</details>

<details><summary>14. What does <code>-/+</code> mean in a plan?</summary>Destroy and recreate (replacement).</details>

<details><summary>15. Why was the EC2 instance replaced when the VPC was added?</summary>AWS can't move an existing instance to a different VPC.</details>

<details><summary>16. How do you preview a change in instance type without editing files?</summary><code>terraform plan -var instance_type=t4g.small</code></details>

<details><summary>17. Default "a", tfvars "b", -var "c": which wins?</summary>"c": the command line is strongest.</details>

<details><summary>18. What does the tutorial's output show?</summary>The instance's private DNS name (<code>aws_instance.app_server.private_dns</code>).</details>

<details><summary>19. What is a module?</summary>A reusable package of Terraform code: inputs, resources, outputs.</details>

<details><summary>20. How do you read the VPC module's first private subnet?</summary><code>module.vpc.private_subnets[0]</code></details>

<details><summary>21. What prefix do module resources get in state?</summary><code>module.vpc.</code></details>

<details><summary>22. Two ways to remove infrastructure?</summary>Remove it from the code and apply (one resource), or <code>terraform destroy</code> (everything).</details>

<details><summary>23. In what order does destroy delete things?</summary>Reverse dependency order (subnets before the VPC).</details>

<details><summary>24. How do you connect a project to HCP Terraform?</summary><code>terraform login</code>, add a <code>cloud {}</code> block, run <code>terraform init</code> to migrate state.</details>

<details><summary>25. Why add AWS keys to the HCP Terraform workspace?</summary>Runs happen on HCP Terraform's servers, which don't have your local credentials. Add them as sensitive environment variables.</details>

<details><summary>26. count vs. for_each?</summary>count = N numbered copies; for_each = one per named item. Prefer for_each for named things.</details>

<details><summary>27. Which lifecycle setting protects a database?</summary><code>prevent_destroy = true</code></details>

<details><summary>28. Why does <code>t4g.nano</code> need an arm64 AMI?</summary>t4g instances use ARM (Graviton) processors.</details>

---

## 🏁 Final check (1 week later)
Without notes: rebuild the four files above from memory → `init` → `apply` → change the instance name with a `-var` → add an output → `destroy`. If you can, you've got the fundamentals. 🎉

---

[← Prev](10-next-steps.md) · [Index](00-index.md)
