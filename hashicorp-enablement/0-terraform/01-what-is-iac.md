# 01 — What is Infrastructure as Code?

[← Index](00-index.md) · [Next: Install & AWS Setup →](02-install-and-aws-setup.md)

📘 Official tutorial: [What is Infrastructure as Code with Terraform?](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/infrastructure-as-code)

---

## 🎯 Goal
Understand **why** Terraform exists and the basic workflow before writing any code.

## 💡 The Idea

**Infrastructure as Code (IaC)** = managing servers, networks and databases with **code files** instead of clicking through the AWS console.

> **Analogy:** Clicking in the console is building a house **without a blueprint**. You can do it, but you can't build an identical second house, and nobody knows exactly what you did. **Terraform code is the blueprint.**

### Why IaC beats clicking

| Clicking in the console | Terraform code |
|---|---|
| Slow and manual | Automated and repeatable |
| Dev, test and prod slowly become different ("drift") | Same code = same environment |
| No history of who changed what | Stored in **Git**: history, reviews, rollback |
| Hard to delete everything cleanly | One command destroys it all |

### What makes Terraform special
- **Declarative:** you describe **what** you want ("one EC2 server"), and Terraform figures out **how**.
- **HCL:** a simple, readable language (files end in `.tf`).
- **Providers:** plugins that let Terraform manage almost anything with an API: **AWS**, Azure, GCP, Kubernetes, GitHub, Databricks, Elastic and thousands more.
- **State:** Terraform remembers what it built, so it can update and delete precisely.

> **Declarative analogy:** ordering at a restaurant. You say "I want a burger" (what). You don't tell the chef how to cook it (how).

### The core workflow

```
   WRITE  ──▶  PLAN  ──▶  APPLY
   code        preview    make it real
```

| Step | What happens | Analogy |
|---|---|---|
| **Write** | Describe resources in `.tf` files | Draw the blueprint |
| **Plan** | Terraform shows what it *would* change. Nothing changes yet | Contractor gives a quote |
| **Apply** | Terraform makes the changes through the AWS API | Contractor builds it |

## 🔑 Key Terms

| Term | Meaning |
|---|---|
| **IaC** | Managing infrastructure with code |
| **Declarative** | Describe the end result, not the steps |
| **HCL** | HashiCorp Configuration Language |
| **Provider** | Plugin connecting Terraform to a platform (e.g., AWS) |
| **Resource** | One piece of infrastructure (e.g., an EC2 instance) |
| **State** | Terraform's record of what it built |
| **Drift** | Real infrastructure no longer matches the code |

## ⚠️ Watch Out
Once Terraform manages something, **stop changing it by hand in the console**. Manual changes create drift.

## 🧠 Recall
1. What is IaC?
2. What does "declarative" mean?
3. What are the three workflow steps?
4. What is a provider?

<details><summary>Answers</summary>

1. Managing infrastructure with code files instead of manual clicking.
2. You describe what you want; Terraform figures out how.
3. Write → Plan → Apply.
4. A plugin that lets Terraform talk to a platform's API (like AWS).
</details>

## 📌 Remember
- IaC = **the blueprint**
- **Write → Plan → Apply**
- Declarative: **what, not how**

---

[← Index](00-index.md) · [Next: Install & AWS Setup →](02-install-and-aws-setup.md)
