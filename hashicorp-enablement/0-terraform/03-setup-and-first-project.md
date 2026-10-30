# 03 — Setup & Your First Project

[← Prev](02-how-terraform-works.md) · [Index](00-index.md) · [Next: Providers & Resources →](04-providers-and-resources.md)

---

## 🎯 Goal
Install Terraform and create something with it in under 30 minutes — **no cloud account needed**.

## 💡 The Idea
The course starts with the **`local` provider**, which creates files on your own computer. It's free and fast, so you can learn Terraform before spending money on AWS.

## Step 1 — Install Terraform

**Mac**
```bash
brew tap hashicorp/tap
brew install hashicorp/tap/terraform
```

**Windows**
```powershell
winget install Hashicorp.Terraform
```
(Or download the zip from the [install page](https://developer.hashicorp.com/terraform/install), unzip `terraform.exe`, and add its folder to your PATH.)

**Check it worked**
```bash
terraform -version
```

## Step 2 — Set up your editor
In **VS Code**, install the **HashiCorp Terraform** extension. It gives you colors, autocomplete and formatting.

## Step 3 — Your first project

Create a folder and a file:
```
my-first-terraform/
└── main.tf
```

**`main.tf`** — type this in yourself:
```hcl
terraform {
  required_providers {
    local = {
      source  = "hashicorp/local"
      version = "2.5.1"
    }
  }
}

resource "local_file" "hello" {
  filename = "${path.module}/hello.txt"
  content  = "Hello from Terraform!"
}
```

Run these commands in that folder:
```bash
terraform init      # download the local provider
terraform plan      # preview: "1 to add"
terraform apply     # type: yes
```

✅ A file called `hello.txt` now exists. **Terraform built it from your code.**

## 🧪 Try It — learn by breaking things

Do each one and watch what Terraform says:

| Experiment | What you'll see | Lesson learned |
|---|---|---|
| Run `terraform apply` again | "No changes" | Terraform only acts on differences |
| Change `content` to something new, then `plan` | It will replace the file | Code changes → infra changes |
| Delete `hello.txt` by hand, then `plan` | It plans to create it again | Terraform notices drift and fixes it |
| Run `terraform destroy` | The file is deleted | Terraform can clean up everything it made |

## 🔑 Key Terms

| Term | Meaning |
|---|---|
| `main.tf` | A Terraform file (any name ending in `.tf` works) |
| `local` provider | Creates files on your computer — great for practice |
| `path.module` | "The folder this file is in" |

## ⚠️ Watch Out
- Always run commands **inside the project folder**.
- Every new project folder needs `terraform init` first.
- Terraform reads **all** `.tf` files in a folder together, so file names are just for you.

## 🧠 Recall
1. Why does the course start with the `local` provider?
2. What 3 commands did you run to create the file?
3. What happened when you deleted the file and ran `plan`?

<details><summary>Answers</summary>

1. It's free and quick — you learn Terraform without needing AWS.
2. `terraform init`, `terraform plan`, `terraform apply`.
3. Terraform noticed it was missing and planned to create it again.
</details>

## 📌 Remember
- Install → `terraform -version`
- New folder → `terraform init`
- `plan` to preview, `apply` to build, `destroy` to clean up

---

[← Prev](02-how-terraform-works.md) · [Index](00-index.md) · [Next: Providers & Resources →](04-providers-and-resources.md)
