# 09 — Architecture, Seal & Unseal

## 🎯 Goal

Understand how Vault protects data at rest, and run a "real" (non-dev) server.

---

## 🏰 The big picture

```
          ┌─────────────────── Vault ───────────────────┐
 clients  │  API / UI / CLI                             │
 ───────► │   │                                         │
          │   ▼                                         │
          │  Auth methods · Policies · Secrets engines  │
          │  Audit devices · Token store                │
          │ ═══════════════ BARRIER ═══════════════════ │ ← everything is encrypted
          └───────────────────────┬─────────────────────┘    when it crosses here
                                  ▼
                        Storage backend (Raft, Consul…)
                        → only ever sees ENCRYPTED data
```

> 🔑 **Vault doesn't trust its own storage.** All data is encrypted *before* it leaves Vault. If someone steals the disk, they get gibberish.

---

## 🔐 Seal / unseal: the key chain

When Vault starts, it's **sealed**: it knows *where* its data is, but can't decrypt it.

```
 Unseal keys (shares)  ──combine──►  Root key  ──decrypts──►  Encryption key  ──decrypts──►  Your data
```

| Piece | Job |
|---|---|
| **Encryption key** | Actually encrypts your data. Stored (encrypted) in storage. |
| **Root key** (used to be called "master key") | Encrypts the encryption key |
| **Unseal keys** | Pieces of the root key, split with **Shamir's Secret Sharing** |

### Shamir's Secret Sharing (in plain English)

The root key is split into **5 pieces** (default). Any **3** of them (default threshold) can rebuild it.

> 🚀 Like a missile launch that needs 3 of 5 officers to turn their keys. No single person can unseal Vault.

### Auto-unseal

Typing unseal keys every restart is a pain. **Auto-unseal** hands that job to a trusted service:
- **Cloud KMS:** AWS KMS, Azure Key Vault, GCP Cloud KMS
- **HSM** (Enterprise)
- **Transit** engine on another Vault

With auto-unseal you get **recovery keys** instead of unseal keys. They're used for certain admin operations, but they **can't** unseal Vault.

**Sealing on purpose:** `vault operator seal` is the emergency brake. If you suspect a breach, seal Vault and nothing can be read until it's unsealed again.

---

## 💾 Storage backends

| Backend | Notes |
|---|---|
| **Integrated Storage (Raft)** | ✅ HashiCorp's recommended default. Built into Vault, no extra system to run. Supports HA. |
| **Consul** | Older common choice; separate system to manage |
| **In-memory** | Dev mode only |

**HA (high availability):** run a cluster (typically 3 or 5 nodes). One is **active**, the others are **standby** and forward requests to the active one.

---

## 🧪 Hands-on: run a "real" server locally

**1. Create `config.hcl`:**
```hcl
storage "raft" {
  path    = "./vault-data"
  node_id = "node1"
}

listener "tcp" {
  address     = "127.0.0.1:8200"
  tls_disable = true   # OK for a laptop lab ONLY. Always use TLS for real.
}

api_addr     = "http://127.0.0.1:8200"
cluster_addr = "https://127.0.0.1:8201"
ui           = true
```

**2. Start it:**
```bash
mkdir -p vault-data
vault server -config=config.hcl
```

**3. In a second terminal, initialize** (this happens **once per cluster, ever**):
```bash
export VAULT_ADDR='http://127.0.0.1:8200'
vault operator init
```
Output: **5 unseal keys + 1 initial root token**.
> ⚠️ In real life you'd hand each key to a different person. For the lab, save them in a scratch file **you never commit to GitHub**.

**4. Check status:**
```bash
vault status     # Sealed: true, Unseal Progress 0/3
```

**5. Unseal** (run 3 times, with 3 *different* keys):
```bash
vault operator unseal   # paste key 1
vault operator unseal   # paste key 2
vault operator unseal   # paste key 3  → Sealed: false 🎉
```

**6. Log in with the root token:**
```bash
vault login <initial-root-token>
```

**7. Restart the server** (Ctrl+C, start again) → it's **sealed** again, but your data is still there (unlike dev mode).

---

## 📝 Audit devices

Log every request/response. Sensitive values are **hashed (HMAC)**, not stored in plain text.
```bash
vault audit enable file file_path=./vault-audit.log
vault audit list
```
> ⚠️ If audit devices are enabled and **none** of them can write, Vault **stops answering requests**. It refuses to work without an audit trail.

---

## ✍️ Check yourself

1. What does "sealed" mean?
2. Default number of unseal key shares and threshold?
3. With auto-unseal, what do you get instead of unseal keys, and can they unseal Vault?
4. What's HashiCorp's recommended storage backend?
5. How often do you run `vault operator init`?

<details>
<summary>Answers</summary>

1. Vault can't decrypt its data. It knows where storage is but has no root key in memory.
2. 5 shares, threshold of 3.
3. Recovery keys, and no, they can't unseal.
4. Integrated Storage (Raft).
5. Once per cluster.

</details>
