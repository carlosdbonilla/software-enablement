# 03 — Create Infrastructure (Your First EC2 Server)

[← Prev](02-install-and-aws-setup.md) · [Index](00-index.md) · [Next: Terraform State →](04-terraform-state.md)

📘 Official tutorial: [Create infrastructure](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-create)

---

## 🎯 Goal
Write your first Terraform configuration and launch a real **EC2 instance** on AWS.

## 💡 The Idea
You'll write two files. Terraform reads **every `.tf` file in the folder** together, so the names are just for humans.

| File | Holds |
|---|---|
| `terraform.tf` | **Settings**: which providers and versions to use |
| `main.tf` | **What to build**: provider config, data sources, resources |

## 🧪 Do It

### Step 1 — Make a project folder
```bash
mkdir learn-terraform-get-started-aws
cd learn-terraform-get-started-aws
```

### Step 2 — `terraform.tf` (settings)
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

### Step 3 — `main.tf` (what to build)
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

resource "aws_instance" "app_server" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t4g.nano"

  tags = {
    Name = "ExampleAppServerInstance"
  }
}
```

### Step 4 — Run the workflow
```bash
terraform fmt         # tidy formatting
terraform init        # download the AWS provider
terraform validate    # check for mistakes
terraform apply       # show the plan → type yes → build
```

You'll see a plan ending in:
```
Plan: 1 to add, 0 to change, 0 to destroy.
```
Type **`yes`**. After a minute or two:
```
Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

### Step 5 — Check it
Open **AWS console → EC2 → Instances** (region **us-west-2 / Oregon**). You'll see `ExampleAppServerInstance` running. **Terraform built it from your code.**

---

## Reading the code, block by block

### 1. The `terraform` block — settings
| Line | Meaning |
|---|---|
| `required_providers { aws = {...} }` | "This project needs the AWS provider" |
| `source = "hashicorp/aws"` | Download it from the Terraform Registry |
| `version = "~> 5.92"` | Any **5.x version from 5.92 up**, but not 6.0. Avoids surprise breaking changes |
| `required_version = ">= 1.2"` | Terraform itself must be 1.2 or newer |

### 2. The `provider` block — configure AWS
`region = "us-west-2"` → build everything in Oregon. Credentials come from your environment ([Lesson 02](02-install-and-aws-setup.md)), **not** from the code.

### 3. The `data` block — look something up
A **data source reads** information that already exists; it doesn't create anything.
- It asks AWS: *"Find the most recent Ubuntu 24.04 image published by Canonical (owner `099720109477`)."*
- **Why:** AMI IDs differ by region and change with every update. Looking one up beats hard-coding an ID.
- Referenced as: `data.aws_ami.ubuntu.id`

### 4. The `resource` block — build something
```
resource "aws_instance" "app_server" {
         └─── TYPE ───┘ └── NAME ──┘
```
| Part | Meaning |
|---|---|
| `aws_instance` | **Type**: an EC2 server (the `aws_` prefix = AWS provider) |
| `app_server` | **Name**: your label inside Terraform |
| `ami` | Which machine image (from the data source) |
| `instance_type` | Server size: `t4g.nano` (tiny, ARM-based, cheap) |
| `tags` | Labels. `Name` is what shows in the console |

**Address** of this resource: `aws_instance.app_server`

### The reference pattern (memorize)
```
resource:     TYPE.NAME.ATTRIBUTE         aws_instance.app_server.id
data source:  data.TYPE.NAME.ATTRIBUTE    data.aws_ami.ubuntu.id
```
Because the instance references the data source, Terraform **looks up the AMI first, automatically**.

---

## What each command did

| Command | What happened |
|---|---|
| `terraform fmt` | Fixed spacing and alignment |
| `terraform init` | Downloaded the AWS provider into `.terraform/` and created `.terraform.lock.hcl` |
| `terraform validate` | Checked syntax and references (no AWS calls) |
| `terraform apply` | Planned → asked for approval → called the AWS API → saved the result in state |

### Files Terraform created
```
learn-terraform-get-started-aws/
├── terraform.tf
├── main.tf
├── .terraform/             ← provider plugin (DON'T commit)
├── .terraform.lock.hcl     ← exact provider version (DO commit)
└── terraform.tfstate       ← what Terraform built (NEVER commit) → Lesson 04
```

## 🔑 Key Terms

| Term | Meaning |
|---|---|
| **AMI** | Amazon Machine Image: the OS template for a server |
| **EC2 instance** | A virtual server in AWS |
| **Instance type** | The server's size (CPU/memory) |
| **Data source** | Reads existing info; doesn't create |
| **Resource address** | `TYPE.NAME`, e.g., `aws_instance.app_server` |
| **Lock file** | `.terraform.lock.hcl`: pins exact provider versions |

## ⚠️ Watch Out
- **Region mismatch:** if you don't see the instance, switch the console to **us-west-2 (Oregon)**.
- **`t4g` = ARM (Graviton).** That's why the AMI filter says `arm64`. To use an Intel type like `t3.micro`, change the filter to `amd64`. Mismatched architecture = error.
- `No valid credential sources found` → re-export your AWS keys or run `aws configure`.
- `terraform apply` costs real (small) money. **Finish with `terraform destroy`** if you stop here.

## 🧠 Recall
1. What goes in `terraform.tf` vs. `main.tf`?
2. What does `~> 5.92` allow?
3. What's the difference between a `data` block and a `resource` block?
4. Why look up the AMI instead of hard-coding it?
5. Write the address of the EC2 resource.
6. Which file should you commit: `.terraform.lock.hcl` or `terraform.tfstate`?

<details><summary>Answers</summary>

1. `terraform.tf`: settings (providers, versions). `main.tf`: provider config, data sources, resources.
2. Any 5.x version ≥ 5.92, but not 6.0.
3. `data` reads something that exists; `resource` creates and manages something.
4. AMI IDs differ by region and change over time; the lookup always finds the latest.
5. `aws_instance.app_server`
6. Commit `.terraform.lock.hcl`. Never commit state.
</details>

## 📌 Remember
- **`terraform` block** = settings · **`provider`** = AWS config · **`data`** = look up · **`resource`** = build
- `fmt → init → validate → apply`
- Reference: `TYPE.NAME.ATTRIBUTE`

---

[← Prev](02-install-and-aws-setup.md) · [Index](00-index.md) · [Next: Terraform State →](04-terraform-state.md)
