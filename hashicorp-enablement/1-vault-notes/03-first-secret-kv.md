# 03 — My First Secret (Key/Value Engine)

## 🎯 Goal

Write, read, update, version, and delete a secret using the **KV (Key/Value)** secrets engine.

> Prereq: dev server running and `VAULT_ADDR` / `VAULT_TOKEN` / `LAB_PASSWORD` set (module 02).

---

## 🗂️ KV v1 vs KV v2

| | KV v1 | KV v2 |
|---|---|---|
| Versioning | ❌ One value, overwritten | ✅ Keeps history of versions |
| Undelete | ❌ | ✅ Soft delete + undelete |
| Dev mode default at `secret/` | | ✅ |

We'll use **KV v2** (the default at `secret/` in dev mode).

---

## 1️⃣ Write a secret

```bash
vault kv put -mount=secret myapp/db username="admin" password="$LAB_PASSWORD"
```

- `-mount=secret` → which KV engine (the one mounted at `secret/`)
- `myapp/db` → the path of my secret inside that engine
- `username=…  password=…` → key/value pairs

Output shows `version 1`.

---

## 2️⃣ Read it back

```bash
vault kv get -mount=secret myapp/db
```

Just one field (great for scripts):
```bash
vault kv get -mount=secret -field=password myapp/db
```

As JSON (great for apps):
```bash
vault kv get -mount=secret -format=json myapp/db
```

---

## 3️⃣ Update it → new version

```bash
vault kv put -mount=secret myapp/db username="admin" password="$(openssl rand -base64 16)"
```
Now it's **version 2**.

> ⚠️ `kv put` **replaces the whole secret**. If you leave out `username`, it's gone in version 2.
> To change one key and keep the rest, use **`patch`**:
> ```bash
> vault kv patch -mount=secret myapp/db password="$(openssl rand -base64 16)"
> ```

Read an older version:
```bash
vault kv get -mount=secret -version=1 myapp/db
```

See all versions + metadata:
```bash
vault kv metadata get -mount=secret myapp/db
```

---

## 4️⃣ Delete vs. Destroy (exam favorite!)

| Command | What happens | Recoverable? |
|---|---|---|
| `vault kv delete -mount=secret myapp/db` | **Soft delete** of the latest version | ✅ `vault kv undelete -mount=secret -versions=3 myapp/db` |
| `vault kv destroy -mount=secret -versions=1 myapp/db` | Permanently removes that version's **data** | ❌ |
| `vault kv metadata delete -mount=secret myapp/db` | Wipes **all versions + metadata** | ❌ |

---

## 5️⃣ List secrets under a path

```bash
vault kv put -mount=secret myapp/api key="abc123"
vault kv list -mount=secret myapp
```
```
Keys
----
api
db
```

---

## 🧠 Behind the scenes: the hidden `data/` path

The CLI hides this, but in KV **v2** the *real* API paths are:

| You type | Actual path |
|---|---|
| `vault kv get -mount=secret myapp/db` | `secret/data/myapp/db` |
| `vault kv metadata get …` | `secret/metadata/myapp/db` |

**Why care?** Because **policies** (module 07) and the **API** (module 10) use the real path with `data/`. Forgetting `data/` in a KV v2 policy is the #1 beginner mistake.

---

## ✍️ Check yourself

1. What's the difference between `kv put` and `kv patch`?
2. You soft-deleted version 3. How do you get it back?
3. Which is permanent: `delete` or `destroy`?
4. In KV v2, what's the full API path for a secret at `secret/` → `team/creds`?

<details>
<summary>Answers</summary>

1. `put` replaces the whole secret (new version with only the keys you pass). `patch` updates only the keys you pass and keeps the rest.
2. `vault kv undelete -mount=secret -versions=3 <path>`
3. `destroy`.
4. `secret/data/team/creds`

</details>
