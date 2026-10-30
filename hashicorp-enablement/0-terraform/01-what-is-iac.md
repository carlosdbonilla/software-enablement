# 01 — What is Infrastructure as Code (IaC)?

[← Index](00-index.md) · [Next: How Terraform Works →](02-how-terraform-works.md)

---

## 🎯 Goal
Understand **why** Terraform exists before learning **how** to use it.

## 💡 The Idea

**Infrastructure** = everything an app needs to run: servers, networks, storage, databases, monitoring.

There are three eras of managing it:

| Era | How it worked | Problem |
|---|---|---|
| **1. On-prem (old way)** | Buy physical servers, set them up by hand | Slow, expensive, can't scale |
| **2. Cloud by clicking** | Click around the AWS console | Still manual, environments slowly become different |
| **3. Infrastructure as Code** | Write files that describe what you want; a tool builds it | ✅ Fast, repeatable, tracked |

> **Analogy:** Building in the console is like building a house without a blueprint — you can do it, but you can't build an identical second house. **IaC is the blueprint.** Hand it to the builder and you get the same house every time.

### The cloud's hidden problem: configuration drift
Dev, staging and prod *should* be identical. But someone changes a setting by hand in one of them, and nobody writes it down. Over time they **drift** apart, and things break in prod that worked in dev.

IaC fixes this: the code is the single source of truth.

### Why IaC is better

- **Consistent** — same code = same environment, every time
- **Automated** — no clicking, fewer human errors
- **Version controlled** — stored in Git, so you have history, reviews and rollbacks
- **Fast** — rebuild a whole environment in minutes

### Types of IaC tools

| Cloud-specific (one cloud only) | Cloud-agnostic (any cloud) |
|---|---|
| AWS CloudFormation, Azure ARM/Bicep, Google Cloud Deployment Manager | **Terraform**, OpenTofu, Pulumi |

## 🔑 Key Terms

| Term | Meaning |
|---|---|
| **Infrastructure** | Servers, networks, storage, databases an app runs on |
| **IaC** | Managing infrastructure with code files instead of clicking |
| **Configuration drift** | When environments become different because of untracked manual changes |
| **Cloud-agnostic** | Works with many cloud providers |

## ⚠️ Watch Out
IaC doesn't *remove* the cloud console — it means you **stop making changes there**. A manual change on something Terraform manages creates drift.

## 🧠 Recall
Answer from memory, then check.

1. What is "infrastructure"?
2. What is configuration drift?
3. Give 3 benefits of IaC.
4. Is Terraform cloud-specific or cloud-agnostic?

<details><summary>Answers</summary>

1. Everything an app needs to run: servers, networks, storage, databases, monitoring.
2. Environments slowly becoming different because of manual, untracked changes.
3. Consistency, automation, version control (also speed/repeatability).
4. Cloud-agnostic.
</details>

## 📌 Remember
- IaC = **infrastructure written as code**
- It solves **manual work** and **drift**
- Terraform works across **any cloud**

---

[← Index](00-index.md) · [Next: How Terraform Works →](02-how-terraform-works.md)
