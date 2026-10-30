# 09 — Modules

[← Prev](08-count-and-for-each.md) · [Index](00-index.md) · [Next: AWS Project →](10-aws-project.md)

---

## 🎯 Goal
Package Terraform code so you can reuse it.

## 💡 The Idea
A **module** is a folder of `.tf` files that work together.

> **Analogy:** A module is a **recipe**. Write it once, then cook it again and again with different ingredients (inputs). You get a dish back (outputs).

| Recipe | Module |
|---|---|
| Ingredients | Input variables |
| Steps | Resources |
| The dish | Outputs |

Every Terraform folder is already a module — you've been using them all along.

### Types of modules

| Type | What it is |
|---|---|
| **Root module** | The folder where you run `terraform` commands |
| **Child module** | A module that another module uses (usually a subfolder) |
| **Published module** | Shared on the Terraform Registry or GitHub for anyone to use |

### Why use them?
- **Reuse** — write once, use many times
- **Consistency** — dev and prod built from the same recipe
- **Simplicity** — hide complexity behind a few inputs
- **Teamwork** — share good patterns across a team

---

## Build one (from the course)

```
05_modules/
├── main.tf               ← ROOT: uses the module
└── file_creator/         ← CHILD: the module (recipe)
    ├── variables.tf      ← ingredients
    ├── main.tf           ← steps
    └── outputs.tf        ← the dish
```

**`file_creator/variables.tf`**
```hcl
variable "filename_1" {
  type    = string
  default = "file1.txt"
}
variable "file1_content" {
  type    = string
  default = "hello from file 1"
}
```

**`file_creator/main.tf`**
```hcl
resource "local_file" "file1" {
  filename = "${path.module}/${var.filename_1}"
  content  = var.file1_content
}
```

**`file_creator/outputs.tf`**
```hcl
output "file1_path" {
  value = local_file.file1.filename
}
```

## Use it

**`05_modules/main.tf`**
```hcl
module "create_files" {
  source = "./file_creator"            # where the module lives

  filename_1    = "my_first_file.txt"  # fill in the inputs
  file1_content = "from file1"
}

output "file_path" {
  value = module.create_files.file1_path   # read its output
}
```

```bash
terraform init    # init is needed whenever you add a module
terraform apply
```

**Format to remember:** `module.MODULE_NAME.OUTPUT_NAME`

---

## Using a published module

Someone else already wrote it — just fill in the inputs:
```hcl
module "vpc" {
  source  = "terraform-aws-modules/vpc/aws"
  version = "5.8.1"      # always pin a version

  name = "my-vpc"
  cidr = "10.0.0.0/16"
}
```
Find these on the [Terraform Registry](https://registry.terraform.io/browse/modules). Each one has an **Inputs** and **Outputs** tab.

## 🧪 Try It
Call the same module **twice** with different inputs:
```hcl
module "files_a" {
  source     = "./file_creator"
  filename_1 = "a.txt"
}

module "files_b" {
  source     = "./file_creator"
  filename_1 = "b.txt"
}
```
One recipe, two dishes. That's the power of modules.

## 🔑 Key Terms

| Term | Meaning |
|---|---|
| **Module** | A folder of `.tf` files used as one unit |
| **Root module** | Where you run commands |
| **Child module** | A module called by another |
| `source` | Where the module's code lives |
| `module.NAME.OUTPUT` | How to read a module's output |

## ⚠️ Watch Out
- You can only read what a module **outputs**. Its inner resources are private.
- Run `terraform init` after adding or changing a module.
- Always pin `version` on published modules.

## 🧠 Recall
1. Using the recipe analogy, what are inputs, resources and outputs?
2. What's the difference between root and child modules?
3. How do you read an output called `ip` from a module called `web`?
4. What command must you run after adding a module?

<details><summary>Answers</summary>

1. Inputs = ingredients, resources = steps, outputs = the dish.
2. Root = where you run commands; child = a module used by another module.
3. `module.web.ip`
4. `terraform init`
</details>

## 📌 Remember
- Module = **reusable recipe**
- Inputs → variables, outputs → `module.NAME.OUTPUT`
- `init` after adding one, pin versions on published ones

---

[← Prev](08-count-and-for-each.md) · [Index](00-index.md) · [Next: AWS Project →](10-aws-project.md)
