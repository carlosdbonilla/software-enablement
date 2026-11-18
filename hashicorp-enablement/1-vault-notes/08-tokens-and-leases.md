# 08 — Tokens & Leases

## 🎯 Goal

Understand how long things live in Vault and how to extend or kill them.

---

## 🎟️ Tokens recap

- **Every** request to Vault uses a token.
- Every token has: **policies**, a **TTL**, and (usually) a **parent**.
- Token prefixes tell you the type at a glance:

| Prefix | Type |
|---|---|
| `hvs.` | Service token (the normal kind) |
| `hvb.` | Batch token |
| `hvr.` | Recovery token |

---

## ⏱️ TTL vs Max TTL (exam favorite)

| | Meaning |
|---|---|
| **TTL** | How long until it expires *unless renewed* |
| **Max TTL** | The **hard ceiling**. No amount of renewing goes past this. |

**Example:** TTL = 1h, Max TTL = 4h
- Renew every hour → it stays alive…
- …until hour 4. Then it's dead, period.

```bash
# Create a token with a policy and TTL
vault token create -policy=myapp-read -ttl=1h -explicit-max-ttl=4h

# Inspect a token
vault token lookup <token>

# Renew it
vault token renew <token>

# Kill it
vault token revoke <token>
```

---

## 🌳 Token hierarchy (parent → child)

When a token creates another token, the new one is its **child**.

```
root
 └── token A
      ├── token B
      └── token C
           └── token D
```

> ⚠️ **Revoke a parent → all its children are revoked too.** Revoke A → B, C, and D all die.

**Orphan tokens** have no parent, so they survive when the creator is revoked:
```bash
vault token create -orphan -policy=myapp-read
```

---

## 🧱 Service tokens vs batch tokens

| | Service token (`hvs.`) | Batch token (`hvb.`) |
|---|---|---|
| Renewable | ✅ | ❌ |
| Can create child tokens | ✅ | ❌ |
| Stored on disk | ✅ | ❌ (lightweight) |
| Can be revoked individually | ✅ | ❌ (expires at TTL, or when parent is revoked) |
| Best for | Most use cases | High-volume, short jobs (thousands of short-lived workloads) |

**Periodic tokens:** a service token with a `period` instead of a max TTL. As long as it's renewed within the period, it can live forever. Good for long-running services that can't re-authenticate easily.
```bash
vault token create -policy=myapp-read -period=24h
```

---

## 📜 Leases

A **lease** is metadata attached to **dynamic secrets** (and service tokens): how long it's valid and whether it can be renewed.

> KV secrets do **not** have leases. They don't expire.

```bash
# Renew a dynamic secret's lease
vault lease renew <lease_id>

# Revoke one
vault lease revoke <lease_id>

# Revoke everything under a path (incident response!)
vault lease revoke -prefix aws/creds/my-ec2-role
```

---

## ✍️ Check yourself

1. TTL is 30m, max TTL is 2h. You renew every 25 minutes. When does the token die?
2. You revoke a parent token. What happens to its children?
3. Which token type can't be renewed and doesn't create children?
4. Do KV secrets have leases?

<details>
<summary>Answers</summary>

1. At 2 hours (max TTL is the hard ceiling).
2. They're revoked too (unless they're orphans).
3. Batch tokens.
4. No, only dynamic secrets and service tokens do.

</details>
