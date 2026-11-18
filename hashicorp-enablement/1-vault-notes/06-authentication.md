# 06 — Authentication (Auth Methods)

## 🎯 Goal

Stop using the root token. Log in as a **human** (userpass) and as an **app** (AppRole).

---

## 🧠 How login works in Vault

```
1. Authenticate  →  "Here's my username/password (or cloud identity, or OIDC…)"
2. Vault checks  →  with the auth method (or an outside system like AWS, Okta, LDAP)
3. Vault returns →  a TOKEN with POLICIES attached
4. Use the token →  on every request after that
```

> 🔑 **Every auth method ends the same way: you get a token.** The token is what Vault actually checks.

---

## 🧰 Common auth methods

| Auth method | Who it's for | Example |
|---|---|---|
| **token** | Everyone (built-in, always on, can't be disabled) | `vault login <token>` |
| **userpass** | Humans (simple, good for labs) | username + password |
| **ldap / oidc** | Humans at a company | Active Directory, Okta, Azure AD/Entra ID |
| **github** | Humans | GitHub personal access token |
| **approle** | Apps, scripts, CI/CD pipelines | role_id + secret_id |
| **aws** | Workloads running in AWS | EC2 instance / IAM role identity |
| **kubernetes** | Pods in K8s | Service account token |
| **jwt** | CI systems (GitHub Actions, GitLab) | Signed identity token |

> 💡 **Humans vs machines** is a common exam theme: humans → OIDC/LDAP/userpass; machines → AppRole, AWS, Kubernetes, JWT.

---

## 1️⃣ Human login: userpass

```bash
# Enable it (lives at auth/userpass/)
vault auth enable userpass

# Create a user
vault write auth/userpass/users/carlos \
    password="$LAB_PASSWORD" \
    policies="default"

# See enabled auth methods
vault auth list
```

Log in (it'll prompt for the password; paste the value from `echo $LAB_PASSWORD`):
```bash
vault login -method=userpass username=carlos
```

You get a token back. Check who you are:
```bash
vault token lookup
```

> ⚠️ Because `VAULT_TOKEN` is set in your env, it overrides your login. To actually *act as* Carlos, run `unset VAULT_TOKEN` first (or open a new terminal). Switch back to admin with `export VAULT_TOKEN=root`.

---

## 2️⃣ Machine login: AppRole

AppRole = a **username + password for apps**:
- **role_id** → like a username (not very secret)
- **secret_id** → like a password (secret, can expire, can be single-use)

```bash
# Enable it
vault auth enable approle

# Create a role for my app
vault write auth/approle/role/my-app \
    token_policies="default" \
    token_ttl=1h \
    token_max_ttl=4h \
    secret_id_ttl=10m

# Get the role_id ("username")
vault read auth/approle/role/my-app/role-id

# Generate a secret_id ("password")
vault write -f auth/approle/role/my-app/secret-id
```

The app logs in with both:
```bash
vault write auth/approle/login \
    role_id="<role_id>" \
    secret_id="<secret_id>"
```
→ returns a token for the app.

> 💡 Best practice: deliver `role_id` and `secret_id` to the app through **different channels** (e.g., role_id baked into the image, secret_id injected by CI at deploy time). Neither alone is enough to log in.

---

## 🧹 Disable an auth method

```bash
vault auth disable userpass
```
⚠️ All tokens issued by it are revoked.

---

## ✍️ Check yourself

1. What does every auth method give you at the end?
2. Which auth method can never be disabled?
3. Which auth method is designed for apps and CI/CD with a role_id + secret_id?
4. Name one auth method for humans and one for workloads running in AWS.

<details>
<summary>Answers</summary>

1. A token (with policies attached).
2. Token auth.
3. AppRole.
4. Humans: userpass, LDAP, OIDC, GitHub. AWS workloads: `aws` auth method.

</details>
