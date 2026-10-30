# 05 — The Workflow Commands

[← Prev](04-providers-and-resources.md) · [Index](00-index.md) · [Next: State →](06-state.md)

---

## 🎯 Goal
Know the 7 commands you'll use every day — and read a plan with confidence.

## 💡 The Idea
Remember **Write → Plan → Apply**? These commands are how you do it.

```
init  →  fmt  →  validate  →  plan  →  apply   ...later...  destroy
setup    tidy    check         preview  build                 clean up
```

## The 7 commands

| Command | What it does | When |
|---|---|---|
| `terraform init` | Downloads providers, sets up the folder | First time in a folder, or after adding a provider/module |
| `terraform fmt` | Auto-formats your code neatly | Before saving/committing |
| `terraform validate` | Checks for syntax mistakes | Before planning |
| `terraform plan` | Shows what *would* change. **Changes nothing.** | Every time before apply |
| `terraform apply` | Shows the plan, asks "yes", then builds | When the plan looks right |
| `terraform destroy` | Deletes everything this project created | When you're done (especially on AWS!) |
| `terraform output` | Shows output values | After apply ([Lesson 07](07-variables-outputs-locals.md)) |

> **Memory trick:** the core four are like a construction job — **init** (get the tools), **plan** (get the quote), **apply** (build it), **destroy** (tear it down).

## Reading a plan

Plans use symbols:

| Symbol | Meaning |
|---|---|
| `+` | create |
| `~` | update in place |
| `-` | destroy |
| `-/+` | **destroy then recreate** (replacement) |

Every plan ends with a summary line:
```
Plan: 2 to add, 1 to change, 0 to destroy.
```

> ⚠️ Pay extra attention to `-` and `-/+`. Those **delete** things. Look for the words `forces replacement` to see which setting caused it.

## Handy extras

```bash
terraform apply -auto-approve      # skip the "yes" prompt (be careful)
terraform state list               # list everything Terraform is managing
terraform console                  # try out expressions interactively
```

## What Terraform adds to your folder

```
my-project/
├── main.tf
├── .terraform/               ← downloaded providers (don't commit to Git)
├── .terraform.lock.hcl       ← locks provider versions (commit this)
├── terraform.tfstate         ← the state file (never commit — Lesson 06)
└── terraform.tfstate.backup  ← previous state (never commit)
```

## 🧪 Try It
In your practice folder:
1. Mess up the spacing in `main.tf`, run `terraform fmt`, and watch it fix it.
2. Misspell `content` as `contnet`, run `terraform validate`, and read the error.
3. Fix it, change the content, run `terraform plan`, and find the `-/+` symbol.
4. Run `terraform state list`.

## ⚠️ Watch Out
- Forgot `init`? You'll get a "provider not installed" error. Run `terraform init`.
- **On AWS, always `terraform destroy` when you finish practicing**, or you'll get a bill.

## 🧠 Recall
1. Which command downloads providers?
2. Which command is a safe preview?
3. What does `-/+` mean in a plan?
4. What should you always do after an AWS practice session?

<details><summary>Answers</summary>

1. `terraform init`
2. `terraform plan`
3. Destroy and recreate (replace).
4. `terraform destroy`
</details>

## 📌 Remember
- `init` → `plan` → `apply` → `destroy`
- `+` create, `~` update, `-` destroy, `-/+` replace
- Read every plan before typing "yes"

---

[← Prev](04-providers-and-resources.md) · [Index](00-index.md) · [Next: State →](06-state.md)
