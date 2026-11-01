# 05 — Change Infrastructure

[← Prev](04-terraform-state.md) · [Index](00-index.md) · [Next: Variables & Outputs →](06-variables-and-outputs.md)

📘 Official tutorial: [Manage infrastructure](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-manage)

---

## 🎯 Goal
Change existing infrastructure safely and read a plan with confidence.

## 💡 The Idea
Infrastructure changes constantly. With Terraform you **edit the code**, **plan**, then **apply**. You never click in the console.

Some changes happen **in place**; others force Terraform to **destroy and recreate** the resource.

> **Analogy:** Repainting a room = **in-place update** (the house stays). Moving the house to a new lot = **replacement** (tear down, rebuild).

## Reading a plan — the symbols

| Symbol | Meaning | Example |
|---|---|---|
| `+` | **create** | New resource |
| `~` | **update in place** | Change a tag |
| `-` | **destroy** | Resource removed from code |
| `-/+` | **destroy, then recreate** | A change AWS can't make in place |
| `<=` | **read** | A data source being read |

Every plan ends with a summary:
```
Plan: 1 to add, 1 to change, 1 to destroy.
```

> 🔎 Look for **`# forces replacement`** next to an argument. That change will **delete and recreate** the resource. Any data on it is lost.

## 🧪 Do It

### Change 1 — Update a tag (in place)
In `main.tf`, change the tag:
```hcl
  tags = {
    Name = "LearnTerraformServer"
  }
```
```bash
terraform plan
```
You'll see `~` and `Plan: 0 to add, 1 to change, 0 to destroy.` A tag can be changed in place.

```bash
terraform apply     # type yes
```
Check the console: the name changed and the instance ID didn't. **Then change the tag back to `"ExampleAppServerInstance"` and apply again**, so your code matches the official tutorial for the next lesson.

### Change 2 — Preview a bigger change without applying
The official tutorial previews a size change with a command-line value. Once you've added the variables in [Lesson 06](06-variables-and-outputs.md), you'll run:
```bash
terraform plan -var instance_type=t4g.small
```
`plan` changes **nothing**. It's always safe to run.

### Change 3 — A change that forces replacement
In [Lesson 07](07-modules-and-vpc.md) you'll move the instance into a new VPC. AWS can't move an existing server between networks, so Terraform plans **`-/+` (replace)**.

## Useful extras

```bash
terraform plan -out=tfplan     # save the plan
terraform apply tfplan         # apply exactly what you reviewed
terraform apply -replace=aws_instance.app_server   # force a rebuild of one resource
```

## 🔑 Key Terms

| Term | Meaning |
|---|---|
| **In-place update** | Changed without being recreated (`~`) |
| **Replacement** | Destroyed and recreated (`-/+`) |
| **Forces replacement** | Plan note showing which argument causes a replacement |
| **Saved plan** | A plan file you apply exactly as reviewed |

## ⚠️ Watch Out
- **Read every plan before typing `yes`**, especially the `-` and `-/+` lines.
- **Replacement loses data** stored on the old resource and usually gives it a **new ID and IP**.
- **Don't fix things in the console.** Change the code, then apply.

## 🧠 Recall
1. What do `+`, `~`, `-` and `-/+` mean?
2. What does `# forces replacement` warn you about?
3. Does `terraform plan` change anything?
4. How do you apply exactly the plan you reviewed?

<details><summary>Answers</summary>

1. Create, update in place, destroy, destroy-and-recreate.
2. That argument's change will delete and recreate the resource.
3. No, it's only a preview.
4. `terraform plan -out=tfplan`, then `terraform apply tfplan`.
</details>

## 📌 Remember
- Edit code → **plan** → **apply**
- `~` is safe-ish; **`-/+` deletes and rebuilds**
- When unsure, run `plan`

---

[← Prev](04-terraform-state.md) · [Index](00-index.md) · [Next: Variables & Outputs →](06-variables-and-outputs.md)
