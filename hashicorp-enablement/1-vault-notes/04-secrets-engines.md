# 04 — Secrets Engines

## 🎯 Goal

Understand what secrets engines are, how they're "mounted" at paths, and how to enable/disable them.

---

## 🧩 What is a secrets engine?

A **secrets engine** is a plugin that **stores, generates, or encrypts** a certain kind of data.

Think of Vault as a **building**, and each secrets engine as a **department** on its own floor:

| Engine | What it does | Type |
|---|---|---|
| `kv` | Stores static key/value secrets | Store |
| `aws` | Generates AWS IAM credentials on demand | Generate (dynamic) |
| `database` | Generates DB usernames/passwords on demand (Postgres, MySQL, etc.) | Generate (dynamic) |
| `transit` | Encrypts/decrypts data you send it — stores nothing | Encrypt |
| `pki` | Issues TLS certificates | Generate |
| `ssh` | Signs SSH keys / one-time SSH passwords | Generate |
| `cubbyhole` | Private storage for **one token only** | Store (built-in) |

---

## 📍 Mounting = giving the engine an address

Every engine is enabled at a **path** (its "mount"). Requests to that path get routed to that engine.

```bash
# Enable KV v2 at the path "kv/"  (default path = engine name)
vault secrets enable -path=kv kv-v2

# Enable another copy at a custom path — totally separate data
vault secrets enable -path=team-a-secrets kv-v2

# See everything that's mounted
vault secrets list
```

> 💡 You can mount the **same engine type many times** at different paths. Each mount is isolated. That's how you'd separate teams or environments (`dev-secrets/`, `prod-secrets/`).

Add a description so future-you knows what it's for:
```bash
vault secrets enable -path=marketing -description="Marketing team secrets" kv-v2
```

---

## ✏️ Move, tune, disable

```bash
# Rename / move a mount
vault secrets move team-a-secrets/ team-alpha/

# Change settings (e.g., default lease TTL)
vault secrets tune -default-lease-ttl=1h aws/

# Disable a mount
vault secrets disable marketing/
```

> ⚠️ **Disabling a secrets engine deletes all its data and revokes all its leases.** No undo.

**Can't disable:** `sys/`, `identity/`, `cubbyhole/` — they're built in.

---

## 🧠 Static vs dynamic — why this is Vault's superpower

| | Static (KV) | Dynamic (AWS, DB…) |
|---|---|---|
| Who creates it? | **You** put it in | **Vault** creates it on request |
| How long does it live? | Until someone changes it | Has a **lease** — auto-deleted when it expires |
| Shared? | Often shared by many apps/people | **Unique per request** — easy to trace and revoke |
| If leaked? | Valid until manually rotated | Probably already expired |

➡️ Next module: actually generate dynamic AWS creds.

---

## ✍️ Check yourself

1. What's the default path if you run `vault secrets enable kv-v2` with no `-path`?
2. Can you enable the KV engine twice? How?
3. What happens to data when you disable a secrets engine?
4. Which engine stores nothing at all?

<details>
<summary>Answers</summary>

1. `kv-v2/` (the default path is the engine type name).
2. Yes, mount it at different paths with `-path=`.
3. It's deleted, and leases are revoked.
4. Transit (encryption as a service).

</details>
