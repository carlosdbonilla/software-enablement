# 11 — Transit: Encryption as a Service

## 🎯 Goal

Encrypt and decrypt data with Vault **without Vault storing the data** and **without my app ever seeing the key**.

---

## 🧠 Why this matters

Developers are bad at crypto (no offense, devs). Rolling your own encryption = bugs, leaked keys, keys hardcoded in repos.

With **transit**:
- The app sends **plaintext** → gets **ciphertext** back
- The app stores the ciphertext in **its own database**
- The **key never leaves Vault**
- Vault stores **nothing** except the keys

> 🏦 Analogy: a bank's check-signing machine. You hand it a check, it stamps it. You never get to hold the stamp.

---

## 🧪 Hands-on

**1. Enable transit and create a key:**
```bash
vault secrets enable transit
vault write -f transit/keys/orders
```

**2. Encrypt.** Transit expects **base64** input:
```bash
vault write transit/encrypt/orders \
    plaintext=$(echo -n "customer SSN goes here" | base64)
```
```
ciphertext    vault:v1:8SDd3WHDOjf7mq69CyCqYjBXAiQQAVZRkFM13ok481zoCmHnSeDX9vyf7w==
```
- `vault:` → it came from Vault
- `v1` → encrypted with **version 1** of the key

**3. Decrypt:**
```bash
vault write -field=plaintext transit/decrypt/orders \
    ciphertext="vault:v1:8SDd3W..." | base64 --decode
```

---

## 🔄 Key rotation (easy mode)

```bash
vault write -f transit/keys/orders/rotate
```
- New encryptions use **v2**
- Old `v1` ciphertext **still decrypts** — nothing breaks

Upgrade old ciphertext to the newest key **without ever seeing the plaintext**:
```bash
vault write transit/rewrap/orders ciphertext="vault:v1:8SDd3W..."
# → vault:v2:...
```

Stop allowing decryption of very old versions:
```bash
vault write transit/keys/orders/config min_decryption_version=2
```

---

## 🧾 Who gets which permissions?

```hcl
# The app can encrypt and decrypt, but can't touch, export, or delete keys
path "transit/encrypt/orders" { capabilities = ["update"] }
path "transit/decrypt/orders" { capabilities = ["update"] }
```

---

## ✍️ Check yourself

1. Does transit store your data?
2. What format must plaintext be in before sending it?
3. What does `v1` in `vault:v1:...` mean?
4. After rotating a key, can you still decrypt old ciphertext?
5. What does `rewrap` do?

<details>
<summary>Answers</summary>

1. No, only the keys.
2. Base64.
3. The key version used to encrypt it.
4. Yes (unless you raise `min_decryption_version`).
5. Re-encrypts ciphertext with the latest key version without exposing the plaintext.

</details>
