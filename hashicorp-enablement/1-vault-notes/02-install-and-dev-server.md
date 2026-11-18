# 02 — Install Vault & Start a Dev Server

## 🎯 Goal

Get Vault installed and a throwaway server running on my laptop.

---

## 1️⃣ Install

Official instructions for every OS: https://developer.hashicorp.com/vault/install

**macOS (Homebrew):**
```bash
brew tap hashicorp/tap
brew install hashicorp/tap/vault
```

**Windows (Chocolatey):**
```powershell
choco install vault
```

**Ubuntu/Debian:**
```bash
wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install vault
```

**Verify:**
```bash
vault version
vault -h          # lists all commands
```

> 💡 Same binary is the **server** *and* the **CLI client**. `vault server` runs Vault; every other command talks to it.

---

## 2️⃣ Start the dev server

```bash
vault server -dev -dev-root-token-id="root"
```

- `-dev` → start in development mode
- `-dev-root-token-id="root"` → make the root token literally `root` so it's easy to type (otherwise it's random)

Leave this terminal window **open**. It's the server. Open a **second terminal** for the commands below.

### What dev mode does for you

| Dev mode… | Why it matters |
|---|---|
| Stores everything **in memory** | Stop the server = everything is gone. Perfect for practice. |
| Starts already **unsealed** | Skips the unseal step (covered in module 09) |
| Listens on `http://127.0.0.1:8200` **without TLS** | Easy to use locally |
| Logs you in with a **root token** | Root = can do anything |
| Enables **KV v2 at `secret/`** | Ready for your first secret |

> ⚠️ **Never use dev mode in production.** No persistence, no TLS, root token printed on screen.

---

## 3️⃣ Point the CLI at the server (second terminal)

**macOS/Linux:**
```bash
export VAULT_ADDR='http://127.0.0.1:8200'
export VAULT_TOKEN='root'
```

**Windows PowerShell:**
```powershell
$env:VAULT_ADDR="http://127.0.0.1:8200"
$env:VAULT_TOKEN="root"
```

| Env var | What it does |
|---|---|
| `VAULT_ADDR` | Where the Vault server is |
| `VAULT_TOKEN` | Who I am (my token) — so I don't have to `vault login` every time |

### 🎲 Make a random lab password (used in later modules)

These notes **never hardcode passwords**. Instead, generate a random one and keep it in an env var:

**macOS/Linux:**
```bash
export LAB_PASSWORD="$(openssl rand -base64 16)"
echo $LAB_PASSWORD   # see it when a prompt asks for it
```

**Windows PowerShell:**
```powershell
$env:LAB_PASSWORD = [Convert]::ToBase64String((1..16 | ForEach-Object { Get-Random -Max 256 }))
```

> 💡 Why? Literal passwords in code or docs get flagged by secret scanners (like **HCP/IBM Vault Radar**) and can block your commits. That's good practice, and a nice real-world reason Vault exists.

---

## 4️⃣ Check it's working

```bash
vault status
```

Look for:
```
Sealed          false    ← good, it's unsealed
Storage Type    inmem    ← in-memory (dev mode)
```

Peek at what's enabled out of the box:
```bash
vault secrets list
```
```
Path          Type         Description
----          ----         -----------
cubbyhole/    cubbyhole    per-token private secret storage
identity/     identity     identity store
secret/       kv           key/value secret storage
sys/          system       system endpoints used for control, policy and debugging
```

---

## 🛑 Stop the server

Press `Ctrl + C` in the server terminal. Everything you created is wiped. Start it again for a clean slate.

---

## ✍️ Check yourself

1. Why should dev mode never be used in production? (Give 2 reasons.)
2. What do `VAULT_ADDR` and `VAULT_TOKEN` do?
3. What port does Vault listen on by default?
4. What happens to your secrets when you stop a dev server?

<details>
<summary>Answers</summary>

1. Any two: data is in memory only, no TLS, root token exposed, auto-unsealed.
2. `VAULT_ADDR` tells the CLI where the server is; `VAULT_TOKEN` is the token used to authenticate.
3. 8200.
4. They're gone — dev mode storage is in-memory.

</details>
