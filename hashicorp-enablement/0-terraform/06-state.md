# 06 — Terraform State

[← Prev](05-workflow-commands.md) · [Index](00-index.md) · [Next: Variables →](07-variables-outputs-locals.md)

---

## 🎯 Goal
Understand Terraform's "memory" — the most important concept in this course.

## 💡 The Idea

**`terraform.tfstate`** is a file where Terraform records **everything it has built**.

> **Analogy:** The state file is Terraform's **inventory list**. Your code says what *should* exist. The inventory says what Terraform *has already built*. Terraform compares the two to decide what to do.

```
  YOUR CODE            STATE FILE                   REAL WORLD
  "1 web server"  ←→   web_server = i-0abc123  ←→   actual server in AWS
```

### Why Terraform needs it
- To know what it already created (so it doesn't create duplicates)
- To match your nickname (`web_server`) to the real cloud ID (`i-0abc123`)
- To know what to delete on `destroy`

### What `plan` really does
```
1. Read the state file
2. Check the real world (refresh)
3. Compare with your code
4. Show the differences
```

---

## ⚠️ Secrets are visible in state

The course demonstrates this:
```hcl
resource "local_sensitive_file" "secret" {
  filename = "${path.module}/secret.txt"
  content  = "kunal123!"
}
```
The plan hides the value as `(sensitive value)`... but **open `terraform.tfstate` and the password is right there in plain text.**

**Rule:** Never commit the state file to GitHub.

---

## Where to keep state: backends

| | **Local** (default) | **Remote** (best practice) |
|---|---|---|
| Where | A file on your laptop | Shared storage like an AWS S3 bucket |
| Share with a team? | ❌ | ✅ |
| Locking (stops two people changing at once)? | ❌ | ✅ |
| Encrypted & backed up? | ❌ | ✅ |
| Good for | Learning | Real projects |

Remote backend example (S3):
```hcl
terraform {
  backend "s3" {
    bucket       = "my-terraform-state-bucket"   # bucket must already exist
    key          = "my-app/terraform.tfstate"
    region       = "us-east-1"
    encrypt      = true
    use_lockfile = true   # turns on locking
  }
}
```
After adding a backend, run `terraform init` again.

---

## Drift

**Drift** = someone changed something by hand (outside Terraform), so reality no longer matches the code.

How Terraform handles it:
1. You run `terraform plan`
2. Terraform checks reality and spots the difference
3. `terraform apply` puts it back the way the code says

To accept the manual change instead, update your code to match it.

## 🧪 Try It
1. Run `terraform apply` in your practice folder.
2. Open `terraform.tfstate` in VS Code and find your file's `content` and `filename`.
3. Edit `hello.txt` by hand. Run `terraform plan`. Terraform catches the drift.
4. Add the `local_sensitive_file` example above, apply, and find the "secret" in the state file.

## 🔑 Key Terms

| Term | Meaning |
|---|---|
| **State file** | `terraform.tfstate` — Terraform's record of what it built |
| **Backend** | Where the state file is stored |
| **Remote backend** | Shared storage (e.g. S3) — the best practice |
| **Locking** | Prevents two people running `apply` at the same time |
| **Drift** | Reality changed outside Terraform |

## ⚠️ Watch Out
- **Never edit the state file by hand.**
- **Never commit it to Git.**
- Hiding a value as "sensitive" does **not** remove it from state.

## 🧠 Recall
1. In one sentence, what is the state file?
2. Why should state never go to GitHub?
3. Name 2 reasons to use a remote backend.
4. What is drift, and how does Terraform detect it?

<details><summary>Answers</summary>

1. Terraform's record that maps your code to the real resources it built.
2. It can contain secrets in plain text.
3. Team sharing, locking, encryption, backups (any two).
4. A manual change outside Terraform; `terraform plan` compares reality to the code and shows it.
</details>

## 📌 Remember
- State = Terraform's **inventory**
- Contains **secrets** → never commit, store remotely
- `plan` detects **drift**

---

[← Prev](05-workflow-commands.md) · [Index](00-index.md) · [Next: Variables →](07-variables-outputs-locals.md)
