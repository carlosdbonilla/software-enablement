# 04 — Providers & Resources

[← Prev](03-setup-and-first-project.md) · [Index](00-index.md) · [Next: Workflow Commands →](05-workflow-commands.md)

---

## 🎯 Goal
Read and write the two most important building blocks in Terraform.

## 💡 The Idea

| | What it is | Analogy |
|---|---|---|
| **Provider** | A plugin that lets Terraform talk to a platform's API | A **translator** between Terraform and AWS |
| **Resource** | One actual piece of infrastructure (a file, server, bucket, database) | The **thing** you want built |

Terraform by itself doesn't know anything about AWS. The **AWS provider** teaches it.

---

## Providers

Providers come from the **[Terraform Registry](https://registry.terraform.io)** — an app store for providers.

You declare them in a `terraform` block:
```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"   # where to download it from
      version = "~> 5.0"          # which version
    }
  }
}
```

Some providers need settings, like which AWS region to use:
```hcl
provider "aws" {
  region = "us-east-1"
}
```

`terraform init` downloads the providers you listed.

> 💡 **`~> 5.0` means** "any 5.x version, but not 6.0." You get bug fixes without surprise breaking changes.

---

## Resources

### The pattern — memorize this
```hcl
resource "TYPE" "NAME" {
  argument = value
}
```

### Real example
```hcl
resource "aws_instance" "web_server" {
  ami           = "ami-0e86e20dae9224db8"
  instance_type = "t2.micro"
}
```

Breaking it down:

| Part | Example | Meaning |
|---|---|---|
| `resource` | `resource` | "I want to create something" |
| **Type** | `aws_instance` | What kind of thing. The first word (`aws`) is the provider. |
| **Name** | `web_server` | Your nickname for it, used inside Terraform |
| **Arguments** | `instance_type = "t2.micro"` | The settings |

### Referencing one resource from another
Format: `TYPE.NAME.ATTRIBUTE`

```hcl
resource "aws_security_group" "web_sg" {
  name = "web-sg"
}

resource "aws_instance" "web_server" {
  ami                    = "ami-0e86e20dae9224db8"
  instance_type          = "t2.micro"
  vpc_security_group_ids = [aws_security_group.web_sg.id]   # ← reference
}
```

Because the server references the security group, **Terraform automatically creates the security group first.** You don't have to tell it the order.

### Where do I find the arguments?
Search the resource on the Registry (e.g. "aws_instance terraform"). Every resource page lists:
- **Arguments** — settings you can give it
- **Attributes** — values you can read after it's created (like `id` or `public_ip`)

You will live on these pages. That's normal — nobody memorizes them.

## 🧪 Try It
In your lesson 3 folder, add a second resource and reference the first:
```hcl
resource "local_file" "copy" {
  filename = "${path.module}/copy.txt"
  content  = local_file.hello.content
}
```
Run `terraform apply`. Then open `copy.txt` — it has the same text as `hello.txt`.

## 🔑 Key Terms

| Term | Meaning |
|---|---|
| **Provider** | Plugin that connects Terraform to a platform |
| **Registry** | Where providers (and modules) are published |
| **Resource** | A piece of infrastructure Terraform manages |
| **Argument** | A setting you give a resource |
| **Attribute** | A value you can read from a resource after it exists |
| **Reference** | `TYPE.NAME.ATTRIBUTE` — using one resource's value in another |

## ⚠️ Watch Out
- The **name** (`web_server`) is only for Terraform. It's *not* the name you'll see in AWS. For that, use tags: `tags = { Name = "web-server" }`.
- Two resources of the same type can't share a name.
- **Never put passwords or AWS keys in `.tf` files** — they'll end up on GitHub.

## 🧠 Recall
1. What's the difference between a provider and a resource?
2. In `resource "aws_s3_bucket" "logs"`, what is the type and what is the name?
3. How do you reference that bucket's `id`?
4. How does Terraform know which resource to create first?

<details><summary>Answers</summary>

1. A provider is the plugin that talks to the platform; a resource is the thing being built.
2. Type: `aws_s3_bucket`. Name: `logs`.
3. `aws_s3_bucket.logs.id`
4. From references — if A uses B's value, B is created first.
</details>

## 📌 Remember
- **Provider** = translator · **Resource** = the thing
- `resource "TYPE" "NAME" { ... }`
- Reference = `TYPE.NAME.ATTRIBUTE`

---

[← Prev](03-setup-and-first-project.md) · [Index](00-index.md) · [Next: Workflow Commands →](05-workflow-commands.md)
