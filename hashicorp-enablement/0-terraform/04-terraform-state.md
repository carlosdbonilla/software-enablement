# 04 — Terraform State

[← Prev](03-create-infrastructure.md) · [Index](00-index.md) · [Next: Change Infrastructure →](05-change-infrastructure.md)

📘 Official tutorial: [Create infrastructure → Inspect state](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/aws-create)

---

## 🎯 Goal
Understand Terraform's "memory," the most important concept to get right.

## 💡 The Idea
After `apply`, Terraform saves what it built in **`terraform.tfstate`**.

> **Analogy:** State is Terraform's **inventory list**. Your code says what *should* exist. The inventory says what Terraform *already built*. Terraform compares the two to decide what to do.

```
  YOUR CODE                 STATE FILE                        AWS
  aws_instance.app_server ←→ app_server = i-0abc123... ←→ the real EC2 instance
```

### Why Terraform needs state
- **Maps names to real IDs:** `aws_instance.app_server` means nothing to AWS; `i-0abc123` does.
- **Knows what it owns:** so it updates instead of creating duplicates.
- **Knows what to delete:** on `destroy`, and in what order.

### What `plan` and `apply` actually do
```
1. Read the state file
2. Refresh: ask AWS what really exists now
3. Compare with your code
4. Show the differences → (apply) make them → save new state
```

## 🧪 Do It
With your EC2 instance from Lesson 03 still running:

```bash
terraform state list
```
```
data.aws_ami.ubuntu
aws_instance.app_server
```

```bash
terraform show
```
Prints everything Terraform knows: instance ID, IP addresses, AMI, tags and more.

```bash
terraform state show aws_instance.app_server
```
Details for just that one resource.

**Try drift:** in the EC2 console, change the instance's **Name tag** by hand. Then run:
```bash
terraform plan
```
Terraform notices and plans to change the tag back. **That's drift detection.**

## Local vs. remote state

| | **Local** (default) | **Remote** (best practice) |
|---|---|---|
| Where | `terraform.tfstate` on your laptop | **HCP Terraform** ([Lesson 09](09-hcp-terraform.md)) or an **S3 bucket** |
| Team can share? | ❌ | ✅ |
| Locking (stops two people applying at once)? | ❌ | ✅ |
| Encrypted & backed up? | ❌ | ✅ |
| Good for | Learning | Real work |

## 🔑 Key Terms

| Term | Meaning |
|---|---|
| **State** | `terraform.tfstate`: Terraform's record of what it built |
| **Refresh** | Checking AWS for the current real status |
| **Drift** | Reality changed outside Terraform |
| **Backend** | Where state is stored (local, HCP Terraform, S3…) |
| **Locking** | Prevents two simultaneous applies |

## ⚠️ Watch Out
- **State can contain secrets** (passwords, keys) in plain text. **Never commit it to Git.**
- **Never edit the state file by hand.** Use `terraform state` commands.
- **Lose the state = Terraform forgets what it built.** The resources keep running (and billing) but Terraform can't manage them. Remote state prevents this.

### `.gitignore` for every Terraform project
```gitignore
**/.terraform/*
*.tfstate
*.tfstate.*
crash.log
*.tfplan
```

## 🧠 Recall
1. What is the state file, in one sentence?
2. Which command lists everything in state?
3. What happens when you change a tag in the console and run `plan`?
4. Why must state never go to GitHub?
5. Name two benefits of remote state.

<details><summary>Answers</summary>

1. Terraform's record mapping your code to the real resources it built.
2. `terraform state list`
3. Terraform detects the drift and plans to change it back to match the code.
4. It can contain secrets in plain text.
5. Team sharing, locking, encryption, backups (any two).
</details>

## 📌 Remember
- State = **inventory list**
- `state list` · `show` · `state show`
- **Secrets inside** → never commit; use **remote state** for real work

---

[← Prev](03-create-infrastructure.md) · [Index](00-index.md) · [Next: Change Infrastructure →](05-change-infrastructure.md)
