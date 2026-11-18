# 🔐 HashiCorp Vault — Beginner Learning Path

> My notes for learning Vault from scratch, the same way I learned Terraform:
> short modules, plain-English explanations, hands-on commands, and a quick quiz at the end of each one.
>
> **Main reference:** HashiCorp's official Vault tutorials → https://developer.hashicorp.com/vault/tutorials
> **Lab setup:** Vault dev server on my laptop + AWS for the dynamic-secrets module

---

## 🗺️ The Path

| # | Module | What I'll be able to do |
|---|--------|-------------------------|
| 01 | [What is Vault?](01-intro.md) | Explain the problem Vault solves in one sentence |
| 02 | [Install & Dev Server](02-install-and-dev-server.md) | Get Vault running locally in 2 minutes |
| 03 | [My First Secret (KV)](03-first-secret-kv.md) | Store, read, version, and delete a secret |
| 04 | [Secrets Engines](04-secrets-engines.md) | Understand "mounts" and enable new engines |
| 05 | [Dynamic Secrets (AWS)](05-dynamic-secrets-aws.md) | Have Vault create short-lived AWS credentials on demand |
| 06 | [Authentication](06-authentication.md) | Log in as a human (userpass) and as an app (AppRole) |
| 07 | [Policies](07-policies.md) | Write HCL policies that control who can do what |
| 08 | [Tokens & Leases](08-tokens-and-leases.md) | Understand TTLs, renewals, revocation, and token types |
| 09 | [Architecture, Seal & Unseal](09-architecture-seal-unseal.md) | Explain how Vault protects data and run a "real" server |
| 10 | [UI & API](10-ui-and-api.md) | Do everything from the browser and from `curl` |
| 11 | [Transit (Encryption as a Service)](11-transit-encryption.md) | Encrypt data without storing it in Vault |
| 12 | [Vault + Terraform](12-vault-and-terraform.md) | Connect what I already know (Terraform) to Vault |
| 13 | [Explain It Like an SE](13-explain-it-like-an-se.md) | Pitch Vault to a customer in plain business terms |
| 14 | [Cheat Sheet](14-cheat-sheet.md) | One page of every command I actually use |

---

## 🧠 How to use these notes

1. **Go in order.** Each module builds on the one before it.
2. **Type the commands yourself.** Don't copy/paste everything; typing makes it stick.
3. **Do the "Check yourself" quiz** at the end of each module before moving on.
4. **Restart the dev server whenever you want a clean slate.** It forgets everything when it stops (that's a feature).

---

## 🔗 Terraform → Vault cheat translation

Since I learned Terraform first, here's how the concepts map:

| Terraform idea | Closest Vault idea |
|---|---|
| Provider (AWS, Azure…) | **Secrets engine** (AWS, database, KV…) |
| `terraform.tfstate` holds sensitive info | Vault is *where* sensitive info should live |
| HCL config files | Vault **policies** and **server config** are also HCL |
| `terraform plan/apply` workflow | `vault write` / `vault read` workflow |
| IAM user access keys in `~/.aws` | Vault-generated, **auto-expiring** AWS keys |

---

## 📌 Version notes (as of Nov 2026)

- Vault jumped from **1.21 → 2.0** in April 2026. This was mostly a **versioning/support-model change** after IBM acquired HashiCorp, not a rewrite. The basics in these notes work the same.
- The **Vault Associate (003)** exam is a 1-hour multiple-choice exam. Check the official page for the current tested version and objectives: https://developer.hashicorp.com/certifications/security-automation
