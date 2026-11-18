# 07 — Policies

## 🎯 Goal

Write policies that control **who can do what, on which paths**.

---

## 🧠 The 3 rules to remember

1. **Default deny.** If a policy doesn't say you can, you can't.
2. **Policies are attached to tokens.** You log in → token gets policies → policies decide access.
3. **`deny` always wins.** If any policy denies a path, it's denied, no matter what else allows it.

---

## ✍️ Policy syntax (HCL — same language as Terraform!)

```hcl
# Read-only access to my app's secrets (KV v2)
path "secret/data/myapp/*" {
  capabilities = ["read"]
}

# Allow listing what's under myapp/
path "secret/metadata/myapp/*" {
  capabilities = ["list"]
}
```

### Capabilities

| Capability | HTTP verb | Meaning |
|---|---|---|
| `create` | POST/PUT | Create new data |
| `read` | GET | Read data |
| `update` | POST/PUT | Change existing data |
| `patch` | PATCH | Partially update data |
| `delete` | DELETE | Delete data |
| `list` | LIST | List keys (doesn't show values) |
| `sudo` | — | Access root-protected paths |
| `deny` | — | Block access. Overrides everything. |

### Path wildcards

| Pattern | Matches |
|---|---|
| `secret/data/myapp/*` | Anything starting with `secret/data/myapp/` (`*` only at the **end**) |
| `secret/data/+/config` | `+` = exactly **one** path segment → `secret/data/app1/config`, `secret/data/app2/config` |

> ⚠️ **KV v2 gotcha:** policies need the **`data/`** (and `metadata/`) segment. `path "secret/myapp/*"` does **nothing** for a KV v2 mount.

---

## 🧪 Hands-on

**1. Write the policy file** `myapp-read.hcl`:
```hcl
path "secret/data/myapp/*" {
  capabilities = ["read"]
}
```

**2. Upload it to Vault:**
```bash
vault policy write myapp-read myapp-read.hcl
vault policy list
vault policy read myapp-read
```

**3. Attach it to a user** (from module 06):
```bash
vault write auth/userpass/users/carlos password="$LAB_PASSWORD" policies="myapp-read"
```

**4. Test it:**
```bash
unset VAULT_TOKEN
vault login -method=userpass username=carlos

vault kv get -mount=secret myapp/db       # ✅ works
vault kv put -mount=secret myapp/db x=y   # ❌ permission denied
vault kv get -mount=secret other/thing    # ❌ permission denied
```

**5. Quick check without logging in as them:**
```bash
export VAULT_TOKEN=root
vault token capabilities <carlos-token> secret/data/myapp/db
```

---

## 🧱 Built-in policies

| Policy | What it is |
|---|---|
| `root` | Can do **everything**. Can't be changed or deleted. Use only for initial setup/emergencies. |
| `default` | Attached to every token by default. Lets a token look up / renew / revoke **itself**, use its cubbyhole, etc. |

---

## 💡 Real-world pattern: a policy per role

```hcl
# app-team.hcl — full control of their own secrets, nothing else
path "secret/data/app-team/*" {
  capabilities = ["create", "read", "update", "patch", "delete"]
}
path "secret/metadata/app-team/*" {
  capabilities = ["list", "read", "delete"]
}

# but never the production DB root creds
path "secret/data/app-team/prod-db-root" {
  capabilities = ["deny"]
}
```

---

## ✍️ Check yourself

1. A token has no policy that mentions `secret/data/billing`. Can it read it?
2. Policy A allows `read` on a path, policy B says `deny`. Result?
3. What's wrong with `path "secret/myapp/*"` on a KV v2 mount?
4. What's the difference between `*` and `+` in a path?

<details>
<summary>Answers</summary>

1. No, default deny.
2. Denied; `deny` always wins.
3. It's missing `data/`. It should be `secret/data/myapp/*`.
4. `*` = glob, only at the end, matches anything after. `+` = exactly one path segment, can be in the middle.

</details>
