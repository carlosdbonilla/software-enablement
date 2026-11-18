# 14 — Vault Cheat Sheet

## ⚙️ Setup
```bash
vault server -dev -dev-root-token-id="root"   # dev server
export VAULT_ADDR='http://127.0.0.1:8200'
export VAULT_TOKEN='root'
vault status
```

## 🗝️ KV v2
```bash
vault kv put     -mount=secret app/db user=admin pass="$LAB_PASSWORD"  # write (new version)
vault kv patch   -mount=secret app/db pass="$(openssl rand -base64 16)"  # update one key
vault kv get     -mount=secret app/db                   # read
vault kv get     -mount=secret -field=pass app/db       # one field
vault kv get     -mount=secret -version=1 app/db        # old version
vault kv list    -mount=secret app
vault kv delete  -mount=secret app/db                   # soft delete
vault kv undelete -mount=secret -versions=2 app/db
vault kv destroy -mount=secret -versions=1 app/db       # permanent
vault kv metadata get    -mount=secret app/db
vault kv metadata delete -mount=secret app/db           # wipe everything
```

## 🧩 Secrets engines
```bash
vault secrets list
vault secrets enable -path=kv kv-v2
vault secrets tune -default-lease-ttl=1h aws/
vault secrets move old/ new/
vault secrets disable kv/                               # deletes data!
```

## ☁️ AWS dynamic secrets
```bash
vault secrets enable aws
vault write aws/config/root access_key=... secret_key=... region=us-east-1
vault write aws/roles/my-role credential_type=iam_user policy_document=@policy.json
vault read aws/creds/my-role
```

## 🪪 Auth
```bash
vault auth list
vault auth enable userpass
vault write auth/userpass/users/carlos password="$LAB_PASSWORD" policies=myapp-read
vault login -method=userpass username=carlos

vault auth enable approle
vault write auth/approle/role/my-app token_policies=myapp-read
vault read  auth/approle/role/my-app/role-id
vault write -f auth/approle/role/my-app/secret-id
vault write auth/approle/login role_id=... secret_id=...
```

## 📜 Policies
```bash
vault policy list
vault policy write myapp-read myapp-read.hcl
vault policy read myapp-read
vault token capabilities <token> secret/data/app/db
```
```hcl
path "secret/data/app/*" { capabilities = ["read"] }
```

## 🎟️ Tokens & leases
```bash
vault token create -policy=p -ttl=1h -explicit-max-ttl=4h
vault token create -orphan -policy=p
vault token create -policy=p -period=24h               # periodic
vault token lookup [token]
vault token renew  [token]
vault token revoke <token>
vault lease renew  <lease_id>
vault lease revoke <lease_id>
vault lease revoke -prefix aws/creds/my-role
```

## 🏰 Operations
```bash
vault server -config=config.hcl
vault operator init                     # once per cluster
vault operator unseal                   # x3 (default threshold)
vault operator seal                     # emergency brake
vault audit enable file file_path=./audit.log
```

## 🔒 Transit
```bash
vault secrets enable transit
vault write -f transit/keys/k
vault write transit/encrypt/k plaintext=$(echo -n "hi" | base64)
vault write -field=plaintext transit/decrypt/k ciphertext=vault:v1:... | base64 --decode
vault write -f transit/keys/k/rotate
vault write transit/rewrap/k ciphertext=vault:v1:...
```

## 🌐 API
```bash
curl -H "X-Vault-Token: $VAULT_TOKEN" $VAULT_ADDR/v1/secret/data/app/db
curl $VAULT_ADDR/v1/sys/health
vault kv get -output-curl-string -mount=secret app/db
```

---

## 🧠 Exam-style facts to memorize

- Default **deny**; **deny beats allow**
- KV v2 policies need **`data/`** and **`metadata/`** paths
- Token auth **can't be disabled**; `root` and `default` policies are built in
- **Max TTL** is a hard ceiling; renewals can't pass it
- Revoke a **parent** token → **children** are revoked (except orphans)
- **Batch** tokens: not renewable, no children, not persisted
- Default Shamir: **5 shares, threshold 3**
- Auto-unseal gives **recovery keys** (can't unseal)
- **Raft** (Integrated Storage) = recommended storage
- Disabling an engine or auth method **deletes data / revokes tokens & leases**
- Transit **stores no data**, only keys
- Audit device can't write → Vault **stops serving requests**
