# 01 — What is Vault?

## 🎯 One-sentence answer

**Vault is a secure, central place to store, generate, and control access to secrets — and it logs every time anyone touches one.**

A *secret* is anything you'd be in trouble for leaking: passwords, API keys, database credentials, AWS access keys, TLS certificates, encryption keys.

---

## 😬 The problem: "secret sprawl"

Without Vault, secrets end up everywhere:

- Hardcoded in application code
- Sitting in `.env` files and config files
- Pasted into Slack, Jira tickets, and wikis
- Committed to GitHub by accident
- Shared AWS keys that **never expire** and nobody remembers who has

Ask a company "who has access to the production database password?" and the honest answer is usually *"…no idea."*

That's secret sprawl. It's a security risk **and** an audit/compliance headache.

---

## ✅ What Vault does about it

| Capability | Plain English |
|---|---|
| **Secure storage** | Secrets are encrypted *before* they're written to disk |
| **Dynamic secrets** | Vault *creates* credentials on demand (e.g., an AWS key) and deletes them automatically later |
| **Identity-based access** | You prove who you are (login), then policies decide what you can touch |
| **Leasing & revocation** | Every dynamic secret has an expiration date; you can kill them early |
| **Encryption as a service** | Apps can ask Vault to encrypt/decrypt data without managing keys themselves |
| **Audit logging** | Every request is logged — who, what, when |

### 🏨 Analogy: the hotel front desk

- **Old way:** one master key copied and handed to everybody. Lose one, and you have to change every lock.
- **Vault way:** you show ID at the front desk (**authentication**), they check what you're allowed into (**policy**), and give you a keycard that **stops working at checkout** (**lease/TTL**). The desk logs every card issued (**audit**).

---

## 🧩 The core vocabulary (learn these 6 words)

| Term | Meaning |
|---|---|
| **Secret** | Sensitive data you want to protect |
| **Secrets engine** | A plugin that stores or generates a type of secret (KV, AWS, database, transit…) |
| **Auth method** | How you log in (token, username/password, AppRole, AWS IAM, OIDC, Kubernetes…) |
| **Policy** | Rules (written in HCL) about which paths you can read/write |
| **Token** | What you get after logging in. You send it with every request. |
| **Lease** | The "expiration timer" attached to tokens and dynamic secrets |

> 🔑 **Everything in Vault is a path.** Secrets, auth methods, policies, system settings — all live at paths like `secret/myapp`, `auth/userpass/`, `sys/policies/`. Policies work by allowing or denying paths.

---

## 🏷️ Flavors of Vault

| Edition | What it is |
|---|---|
| **Community Edition** | Free, source-available (Business Source License since 2023). What we use for learning. |
| **Enterprise** | Paid, self-managed. Adds things like namespaces, replication, HSM support, and more. |
| **HCP Vault Dedicated** | HashiCorp-managed Vault in the cloud (AWS/Azure). You use it; they run it. |

> 📝 Side note: the 2023 license change led the community to fork Vault into **OpenBao**. Good to know it exists if a customer mentions it.

---

## ✍️ Check yourself

1. What is "secret sprawl" and why is it a problem?
2. What's the difference between a **static** secret and a **dynamic** secret?
3. Name the 6 core Vault vocabulary words.
4. Finish the sentence: "In Vault, everything is a ______."

<details>
<summary>Answers</summary>

1. Secrets scattered across code, files, chats, and repos, so you can't tell who has access or rotate them easily.
2. Static = you put it in and it stays the same (e.g., a password you store in KV). Dynamic = Vault generates it on request and it auto-expires.
3. Secret, secrets engine, auth method, policy, token, lease.
4. Path.

</details>
