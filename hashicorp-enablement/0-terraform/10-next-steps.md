# 10 — Next Steps: Beyond the Tutorials

[← Prev](09-hcp-terraform.md) · [Index](00-index.md) · [Next: Review Sheet →](11-review-sheet.md)

---

## 🎯 Goal
Learn the handful of extra concepts you'll meet right after the Get Started tutorials, then prove your skills with a challenge.

## 1. Create many resources from one block

### `count` — N copies
```hcl
resource "aws_instance" "web" {
  count         = 3
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t4g.nano"

  tags = {
    Name = "web-${count.index}"     # web-0, web-1, web-2
  }
}
```
Refer to one: `aws_instance.web[0]` · all: `aws_instance.web[*].id`

**Handy trick, on/off switch:** `count = var.create_server ? 1 : 0`

### `for_each` — one per named item
```hcl
resource "aws_s3_bucket" "env" {
  for_each = toset(["dev", "test", "prod"])
  bucket   = "my-learn-tf-${each.key}-12345"   # must be globally unique
}
```
Refer to one: `aws_s3_bucket.env["dev"]`

| Use `count` when… | Use `for_each` when… |
|---|---|
| Copies are identical, or for on/off | Items have names; you may add or remove some later |

> With `count`, removing the middle item **shifts the numbers**, so later items get recreated. `for_each` uses names, so only that item is affected. **When in doubt, use `for_each`.**

## 2. Other meta-arguments

| Meta-argument | Purpose |
|---|---|
| `depends_on = [aws_s3_bucket.x]` | Force an order Terraform can't see from references |
| `provider = aws.east` | Use a second (aliased) provider config, e.g., another region |
| `lifecycle { prevent_destroy = true }` | Refuse to delete (protect databases) |
| `lifecycle { create_before_destroy = true }` | Build the replacement before deleting the old one |
| `lifecycle { ignore_changes = [tags] }` | Ignore outside changes to these arguments |

## 3. More commands you'll use

| Command | Use |
|---|---|
| `terraform console` | Try expressions and functions interactively |
| `terraform plan -refresh-only` | See drift without changing anything |
| `terraform import` / `import {}` block | Bring an existing AWS resource under Terraform |
| `terraform state mv` / `moved {}` block | Rename a resource without recreating it |
| `terraform init -upgrade` | Upgrade providers within version constraints |
| `TF_LOG=DEBUG terraform plan` | Detailed logs for troubleshooting |

## 4. Best practices (short list)

- **Remote state** (HCP Terraform or S3) for anything real
- **Never commit** state, `.terraform/` or secrets; **do commit** `.terraform.lock.hcl`
- **Pin** provider and module versions
- **Look up** AMIs and VPCs with data sources instead of hard-coding IDs
- **`fmt` + `validate`** before every commit; read every plan
- **Variables** for anything that changes per environment; **outputs** for what others need
- **Modules** for repeated patterns; separate folders or workspaces per environment
- **No manual console changes** to Terraform-managed resources
- **Secrets** from Vault or sensitive variables, never in code

## 5. Where Terraform fits (the HashiCorp picture)

| Product | Job |
|---|---|
| **Terraform** | Build infrastructure (this course) |
| **Vault** | Secrets: dynamic AWS credentials, DB passwords, certificates |
| **HCP Terraform** | Remote state, runs, policies, team access |
| **Packer** | Build machine images (AMIs) |
| **Nomad / Consul** | Run workloads / connect services |

**Terraform + Vault:** instead of long-lived AWS keys, Vault issues **short-lived AWS credentials** for each run, and apps pull secrets from Vault instead of `.tfvars`.

---

## 🏁 Challenge project (do this without the tutorials)

Build in a **new folder**, then destroy it:

1. `terraform.tf` with the AWS provider `~> 5.92`
2. A VPC with the **VPC module** (1 public subnet, 2 private)
3. A **security group** (`aws_security_group`) in that VPC allowing inbound **port 443** from anywhere
4. An EC2 instance in the **public** subnet using that security group, with `associate_public_ip_address = true`
5. An **S3 bucket** per environment using **`for_each`** over `["dev", "test"]`
6. **Variables** for instance type and project name; a **`terraform.tfvars`** to set them
7. **Outputs** for the instance's public IP and the bucket names
8. `fmt` → `init` → `validate` → `plan` → `apply` → check the console → **`destroy`**

If you get stuck, the [AWS provider docs](https://registry.terraform.io/providers/hashicorp/aws/latest/docs) and the [VPC module page](https://registry.terraform.io/modules/terraform-aws-modules/vpc/aws/latest) have everything you need. Reading Registry docs **is** the real skill.

## 📌 Remember
- `count` = N copies · `for_each` = named items (preferred)
- `lifecycle` protects important things
- The **Registry docs** are your daily reference

---

[← Prev](09-hcp-terraform.md) · [Index](00-index.md) · [Next: Review Sheet →](11-review-sheet.md)
