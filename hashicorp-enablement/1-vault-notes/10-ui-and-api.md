# 10 — The UI and the API

## 🎯 Goal

Know that the CLI, UI, and API are **three doors into the same room**, and use each one.

```
 CLI  ─┐
 UI   ─┼──►  HTTP API (/v1/...)  ──►  Vault
 curl ─┘
```

> 🔑 The CLI and the UI are just friendly wrappers around the **HTTP API**. Anything you can do in one, you can do in the others.

---

## 🖥️ The UI

1. Start the dev server: `vault server -dev -dev-root-token-id="root"`
2. Open **http://127.0.0.1:8200/ui**
3. Sign in with method **Token**, token `root`

Try:
- **Secrets** → `secret/` → create `myapp/ui-test`
- **Access** → enable the userpass auth method, create a user
- **Policies** → write a policy in the browser editor

> ℹ️ The Vault 2.0 UI was redesigned (problem-based navigation, guided onboarding), so menu names may differ from older tutorials and videos.

---

## 🌐 The HTTP API

All endpoints start with **`/v1/`**. You authenticate with the **`X-Vault-Token`** header.

**Read a KV v2 secret** (remember the hidden `data/`):
```bash
curl --header "X-Vault-Token: $VAULT_TOKEN" \
     $VAULT_ADDR/v1/secret/data/myapp/db
```

**Write a KV v2 secret** (note the `"data"` wrapper):
```bash
curl --header "X-Vault-Token: $VAULT_TOKEN" \
     --request POST \
     --data "{\"data\": {\"username\": \"admin\", \"password\": \"$LAB_PASSWORD\"}}" \
     $VAULT_ADDR/v1/secret/data/myapp/db
```

**Health check** (no token needed, used by load balancers):
```bash
curl $VAULT_ADDR/v1/sys/health
```

**Log in via API (userpass):**
```bash
curl --request POST \
     --data "{\"password\": \"$LAB_PASSWORD\"}" \
     $VAULT_ADDR/v1/auth/userpass/login/carlos
```
The token is in the response at `.auth.client_token`.

---

## 🪄 Superpower: turn any CLI command into curl

```bash
vault kv get -output-curl-string -mount=secret myapp/db
```
Prints the exact `curl` command instead of running it. Great for learning the API and for showing a developer how to integrate.

---

## 🤖 Getting secrets into apps (good to know)

| Option | Idea |
|---|---|
| **Call the API directly** / use a client library | App talks to Vault itself |
| **Vault Agent** | Sidecar/daemon that logs in for the app (auto-auth), caches, and writes secrets to a file via templates. App doesn't need to know Vault exists. |
| **Vault Secrets Operator / Agent Injector** | Kubernetes-native ways to get secrets into pods |

---

## ✍️ Check yourself

1. What header carries your token in an API call?
2. What prefix do all API paths start with?
3. Which endpoint is used for health checks?
4. What CLI flag shows you the equivalent `curl` command?

<details>
<summary>Answers</summary>

1. `X-Vault-Token`
2. `/v1/`
3. `/v1/sys/health`
4. `-output-curl-string`

</details>
