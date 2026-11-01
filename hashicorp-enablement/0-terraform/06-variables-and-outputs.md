# 06 — Variables & Outputs

[← Prev](05-change-infrastructure.md) · [Index](00-index.md) · [Next: Modules & the VPC →](07-modules-and-vpc.md)

📘 Official tutorial: [Manage infrastructure → variables and outputs](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-manage)

---

## 🎯 Goal
Stop hard-coding values. Make the configuration flexible with **input variables**, and expose useful results with **outputs**.

## 💡 The Idea

| Type | Direction | Analogy |
|---|---|---|
| **Input variable** | Values going **in** | A form you fill out |
| **Output** | Values coming **out** | The receipt you get back |
| **Local value** (bonus) | Used **inside** only | A sticky note to avoid repeating yourself |

Right now `"t4g.nano"` and `"ExampleAppServerInstance"` are hard-coded in `main.tf`. To use a bigger server in production, you'd have to edit the code. Variables fix that.

## 🧪 Do It

### Step 1 — Create `variables.tf`
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
The defaults equal today's values, so **nothing changes yet**.

### Step 2 — Use the variables in `main.tf`
```hcl
resource "aws_instance" "app_server" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = var.instance_type

  tags = {
    Name = var.instance_name
  }
}
```
**`var.NAME`** reads a variable.

### Step 3 — Preview a different value (without applying)
```bash
terraform plan -var instance_type=t4g.small
```
The plan shows the instance type would change. Nothing is applied.

### Step 4 — Create `outputs.tf`
```hcl
output "instance_hostname" {
  description = "Private DNS name of the EC2 instance."
  value       = aws_instance.app_server.private_dns
}
```
```bash
terraform apply     # only adds the output; type yes
terraform output
```
```
instance_hostname = "ip-172-31-xx-xx.us-west-2.compute.internal"
```

---

## Ways to set a variable

| Method | Example |
|---|---|
| **Command line** | `terraform apply -var instance_type=t4g.small` |
| **`terraform.tfvars` file** (loaded automatically) | `instance_type = "t4g.small"` |
| **`*.auto.tfvars` file** (loaded automatically) | `prod.auto.tfvars` |
| **Var file flag** | `terraform apply -var-file="prod.tfvars"` |
| **Environment variable** | `export TF_VAR_instance_type=t4g.small` |
| **Default** | `default = "t4g.nano"` in the variable block |
| **Prompt** | No default and no value → Terraform asks you |

### Which one wins? (strongest first)
```
1. -var / -var-file on the command line   ← strongest
2. *.auto.tfvars files
3. terraform.tfvars
4. TF_VAR_ environment variables
5. default                                ← weakest
```
**Memory trick:** the closer to the moment you run the command, the stronger it is.

## Variable options

| Option | Purpose |
|---|---|
| `type` | `string`, `number`, `bool`, `list(string)`, `map(string)`, `object({...})` |
| `default` | Fallback value. **No default = required** |
| `description` | Documentation |
| `sensitive = true` | Hide the value in plan/apply output (still stored in state!) |
| `validation {}` | Custom rule, e.g., only allow certain instance types |

```hcl
variable "instance_type" {
  type    = string
  default = "t4g.nano"

  validation {
    condition     = startswith(var.instance_type, "t4g.")
    error_message = "Use a t4g (ARM) instance type to match the arm64 AMI."
  }
}
```

## Output commands
```bash
terraform output                         # all outputs
terraform output instance_hostname       # one output
terraform output -raw instance_hostname  # no quotes, good for scripts
terraform output -json                   # machine-readable
```

## Bonus — local values
```hcl
locals {
  common_tags = {
    Project   = "learn-terraform"
    ManagedBy = "terraform"
  }
}
# use: tags = merge(local.common_tags, { Name = var.instance_name })
```
Declare with `locals` (plural), use with `local.` (singular).

## 🔑 Key Terms

| Term | Meaning |
|---|---|
| `variable` / `var.NAME` | Declare / use an input |
| `output` | A value displayed after apply |
| `terraform.tfvars` | File of variable values, loaded automatically |
| `TF_VAR_name` | Environment variable that sets a Terraform variable |
| `locals` / `local.NAME` | Internal named values |
| `sensitive` | Hides a value on screen (not in state) |

## ⚠️ Watch Out
- **Don't commit secrets** in `.tfvars`. Use `TF_VAR_` environment variables, HCP Terraform sensitive variables ([Lesson 09](09-hcp-terraform.md)) or **Vault**.
- Outputs referencing a resource break if you delete that resource. You'll see this in [Lesson 08](08-destroy-infrastructure.md).
- `private_dns` is a **private** address. It's only reachable inside the VPC.

## 🧠 Recall
1. How do you use a variable called `instance_type`?
2. Default `"a"`, tfvars `"b"`, `-var` `"c"`: which wins?
3. What happens if a variable has no default and no value?
4. How do you print just one output with no quotes?
5. Does `sensitive = true` keep a value out of state?

<details><summary>Answers</summary>

1. `var.instance_type`
2. `"c"` — the command line is strongest.
3. Terraform prompts you for it.
4. `terraform output -raw NAME`
5. No, it only hides it on screen.
</details>

## 📌 Remember
- **Variables in**, **outputs out**, **locals inside**
- `var.` · `local.` · `terraform output`
- Command line > auto.tfvars > tfvars > TF_VAR_ > default

---

[← Prev](05-change-infrastructure.md) · [Index](00-index.md) · [Next: Modules & the VPC →](07-modules-and-vpc.md)
