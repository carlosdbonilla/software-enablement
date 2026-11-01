# 00 — Terraform on AWS: Learning Path

> **Main reference:** HashiCorp's official [Get Started – AWS](https://developer.hashicorp.com/terraform/tutorials/aws-get-started) tutorials
> **Official example code:** [hashicorp-education/learn-terraform-get-started-aws](https://github.com/hashicorp-education/learn-terraform-get-started-aws)

---

## The whole thing in one sentence

**Terraform lets you write your AWS setup as code, preview the changes, and then build, change or delete it automatically.**

## What you'll build

One small project, grown step by step, exactly like the official tutorials:

```
Step 1: one EC2 server            Step 2: + variables & outputs       Step 3: + a VPC network (module)
┌──────────────┐                  ┌──────────────┐                    ┌──────── VPC 10.0.0.0/16 ────────┐
│ EC2 instance │       ──▶        │ EC2 instance │          ──▶       │ public subnet · private subnets │
│ (Ubuntu)     │                  │ var.* / out  │                    │        ┌──────────────┐         │
└──────────────┘                  └──────────────┘                    │        │ EC2 instance │         │
                                                                      └────────┴──────────────┴─────────┘
Step 4: destroy it                Step 5: store state in HCP Terraform (team-ready)
```

## The path

Each lesson matches an official tutorial (📘). Do them in order.

| Step | Lesson | Official tutorial | Time |
|---|---|---|---|
| 1 | [01 — What is IaC?](01-what-is-iac.md) | 📘 What is IaC with Terraform? | 15 min |
| 2 | [02 — Install & AWS Setup](02-install-and-aws-setup.md) | 📘 Install Terraform | 30 min |
| 3 | [03 — Create Infrastructure](03-create-infrastructure.md) | 📘 Create infrastructure | 45 min |
| 4 | [04 — Terraform State](04-terraform-state.md) | 📘 Create infrastructure (state section) | 20 min |
| 5 | [05 — Change Infrastructure](05-change-infrastructure.md) | 📘 Manage infrastructure | 20 min |
| 6 | [06 — Variables & Outputs](06-variables-and-outputs.md) | 📘 Manage infrastructure | 30 min |
| 7 | [07 — Modules & the VPC](07-modules-and-vpc.md) | 📘 Manage infrastructure | 40 min |
| 8 | [08 — Destroy Infrastructure](08-destroy-infrastructure.md) | 📘 Destroy infrastructure | 15 min |
| 9 | [09 — HCP Terraform](09-hcp-terraform.md) | 📘 Collaborate using HCP Terraform | 40 min |
| 10 | [10 — Next Steps](10-next-steps.md) | Beyond the tutorials | 30 min |
| 11 | [11 — Review Sheet](11-review-sheet.md) | — | 15 min |

**Total: about 5 hours**, including hands-on time.

---

## ⚠️ Before you start: cost

These tutorials create **real AWS resources**. They're small and cheap (cents per hour), but:
- **Finish a session with `terraform destroy`** ([Lesson 08](08-destroy-infrastructure.md)).
- Set an **AWS Budget alert** (e.g., $5) in the Billing console.

## How to study (so it sticks)

Every lesson has the same shape:

1. **🎯 Goal** — what you're learning
2. **💡 The Idea** — plain English + an analogy
3. **🧪 Do It** — the hands-on step from the official tutorial
4. **🔑 Key Terms** — words you must know
5. **⚠️ Watch Out** — common mistakes
6. **🧠 Recall** — answer *without looking*, then check

**Three habits**
- **Type the code. Don't copy-paste it.**
- **Answer the Recall questions out loud before opening the answers.**
- **Explain each lesson in 30 seconds** as if to a customer. That's what an interview tests.

## Review schedule

| When | What |
|---|---|
| Same day | Re-answer that lesson's Recall questions |
| Next day | [Review Sheet](11-review-sheet.md) flashcards |
| 1 week later | Flashcards + rebuild the whole project from memory, then destroy it |
| Before an interview | Review Sheet + "explain it" lines |

## Progress tracker

- [ ] 01 What is IaC
- [ ] 02 Install & AWS setup
- [ ] 03 Create infrastructure
- [ ] 04 State
- [ ] 05 Change infrastructure
- [ ] 06 Variables & outputs
- [ ] 07 Modules & VPC
- [ ] 08 Destroy (and checked the AWS console!)
- [ ] 09 HCP Terraform
- [ ] 10 Next steps
- [ ] 11 Review — next day
- [ ] 11 Review — 1 week (rebuilt from memory)

---

[Start → 01 What is IaC?](01-what-is-iac.md)
