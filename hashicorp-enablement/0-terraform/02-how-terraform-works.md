# 02 — How Terraform Works

[← Prev](01-what-is-iac.md) · [Index](00-index.md) · [Next: Setup →](03-setup-and-first-project.md)

---

## 🎯 Goal
Get the mental model of what Terraform does when you run it.

## 💡 The Idea

**Terraform** is a free tool by HashiCorp. You describe infrastructure in `.tf` files, and Terraform builds it.

Three things to know about it:

1. **It uses HCL** (HashiCorp Configuration Language) — a simple, readable language.
2. **It's declarative** — you say **what** you want, not **how** to build it.
3. **It manages the whole lifecycle** — create, update and delete.

> **Analogy — declarative:** Ordering at a restaurant. You say *"I want a burger"* (what). You don't tell the chef how to cook it (how). Terraform is the chef.

### What happens behind the scenes

```
 Your .tf files ──▶  Terraform  ──▶  Provider (plugin)  ──▶  Cloud API (AWS, Azure…)
  "what I want"     compares want       translates to          builds it
                    vs. what exists     API calls
                          │
                    State file
                 "what I already built"
```

### The 3-step workflow: Write → Plan → Apply

```
  WRITE  ───▶  PLAN  ───▶  APPLY
  your code    preview     make it real
```

| Step | What happens | Analogy |
|---|---|---|
| **Write** | You describe resources in `.tf` files | Draw the blueprint |
| **Plan** | Terraform shows what it *would* change. Nothing changes yet. | Contractor gives you a quote |
| **Apply** | Terraform makes the changes | Contractor builds it |

You'll learn the exact commands in [Lesson 05](05-workflow-commands.md).

### Declarative in action
Say your code asks for **3 servers**:

- First `apply` → Terraform creates 3.
- Run `apply` again → **nothing happens** (you already have 3).
- Change the code to 5 → Terraform creates **only 2 more**.

Terraform always works out the difference between **what you want** and **what exists**.

## 🔑 Key Terms

| Term | Meaning |
|---|---|
| **HCL** | The language Terraform files are written in |
| **Declarative** | Describe the end result, not the steps |
| **Desired state** | What your code says should exist |
| **Provider** | Plugin that lets Terraform talk to a platform (AWS, Azure…) |
| **Plan** | A preview of changes |
| **Apply** | Actually making the changes |

## ⚠️ Watch Out
`plan` is **safe** — it never changes anything. When in doubt, run `plan`.

## 🧠 Recall
1. What does "declarative" mean?
2. What are the 3 steps of the Terraform workflow?
3. Your code says 3 servers, and 3 exist. What does `apply` do?
4. What is a provider?

<details><summary>Answers</summary>

1. You describe what you want; Terraform figures out how.
2. Write → Plan → Apply.
3. Nothing — reality already matches the code.
4. A plugin that lets Terraform talk to a specific platform's API.
</details>

## 📌 Remember
- You write **what** you want, Terraform figures out **how**
- **Write → Plan → Apply**
- Plan = preview, Apply = build

---

[← Prev](01-what-is-iac.md) · [Index](00-index.md) · [Next: Setup →](03-setup-and-first-project.md)
