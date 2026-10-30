# 07 — Variables, Outputs & Locals

[← Prev](06-state.md) · [Index](00-index.md) · [Next: Count & For Each →](08-count-and-for-each.md)

---

## 🎯 Goal
Stop hard-coding values. Make your code flexible and reusable.

## 💡 The Idea

| Type | Direction | Analogy |
|---|---|---|
| **Input variable** | Values going **in** | A form you fill out |
| **Output** | Values coming **out** | The receipt you get back |
| **Local** | Used **inside** only | A sticky note to avoid repeating yourself |

---

## 1. Input variables

**Declare it** (usually in `variables.tf`):
```hcl
variable "filename" {
  description = "Name of the file to create"
  type        = string
  default     = "example"
}
```

**Use it** with `var.`:
```hcl
resource "local_file" "example" {
  filename = "${path.module}/${var.filename}.txt"
  content  = "demo content"
}
```

Common types: `string`, `number`, `bool`, `list(string)`, `map(string)`.

> No `default`? Then the variable is **required**, and Terraform will ask you for it.

### Ways to set a variable

| Method | Example |
|---|---|
| Command line | `terraform apply -var="filename=from-cli"` |
| `terraform.tfvars` file (loaded automatically) | `filename = "from-tfvars"` |
| Environment variable | `export TF_VAR_filename="from-env"` |
| Default value | `default = "example"` in the variable block |

### Which one wins? (highest to lowest)
```
1. -var on the command line        ← strongest
2. *.auto.tfvars files
3. terraform.tfvars
4. TF_VAR_ environment variables
5. default value                    ← weakest
```

> **Memory trick:** The closer to the moment you run the command, the stronger it is. Typing it on the command line beats everything; the default written long ago loses to everything.

---

## 2. Outputs

Print useful values after `apply` — like a server's IP address.

```hcl
output "file_path" {
  value = local_file.example.filename
}

output "db_password" {
  value     = aws_db_instance.db.password
  sensitive = true     # hidden on screen (but still in state!)
}
```

```bash
terraform output              # show all
terraform output file_path    # show one
```

---

## 3. Locals

Name a value once, use it many times. Declare with `locals` (plural), use with `local.` (singular).

```hcl
locals {
  environment = "prod"
  upper_env   = upper(local.environment)    # "PROD"
}

resource "local_file" "config" {
  filename = "${path.module}/${local.upper_env}-config.txt"
  content  = "env = ${local.environment}"
}
```

### Variable or local?
- **Variable** → someone outside might want to change it (region, size, name)
- **Local** → it's calculated from other values, used internally

## 🧪 Try It — the precedence game
1. Create a variable `filename` with `default = "default-value"`. Apply. Check the file name.
2. Add `terraform.tfvars` with `filename = "from-tfvars"`. Apply. Name changes.
3. Run `terraform apply -var="filename=from-cli"`. Name changes again.

You've just proven the order of precedence yourself.

## 🔑 Key Terms

| Term | Meaning |
|---|---|
| `variable` | An input, used as `var.NAME` |
| `terraform.tfvars` | File of variable values, loaded automatically |
| `TF_VAR_name` | Environment variable that sets a Terraform variable |
| `output` | A value displayed after apply |
| `locals` / `local.` | Internal named values |
| `sensitive = true` | Hides a value on screen (not in state) |

## ⚠️ Watch Out
- Don't put secrets in `terraform.tfvars` if you commit it to Git. Use `TF_VAR_` environment variables instead.
- `locals` to declare, `local.` to use — a common typo.

## 🧠 Recall
1. How do you use a variable called `region`?
2. Default is "a", tfvars says "b", command line says "c". Which wins?
3. What's the difference between an output and a local?
4. Does `sensitive = true` remove a value from state?

<details><summary>Answers</summary>

1. `var.region`
2. "c" — the command line wins.
3. Outputs show values *outside* after apply; locals are used *inside* the code.
4. No, it only hides it on screen.
</details>

## 📌 Remember
- **Variables** in, **outputs** out, **locals** inside
- `var.` · `local.` · `output`
- Command line beats tfvars beats default

---

[← Prev](06-state.md) · [Index](00-index.md) · [Next: Count & For Each →](08-count-and-for-each.md)
