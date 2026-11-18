# 05 — Dynamic Secrets with AWS

## 🎯 Goal

Have Vault **create real AWS IAM credentials on demand**, then watch them get revoked.

> This is the "wow" demo of Vault. Instead of long-lived AWS keys in `~/.aws/credentials`, you ask Vault and get a key that **expires on its own**.

---

## 🧠 How it works

```
 You / your app                Vault                         AWS
      │   "give me creds"        │                             │
      │ ───────────────────────► │  create IAM user + key      │
      │                          │ ──────────────────────────► │
      │   access key + lease     │                             │
      │ ◄─────────────────────── │                             │
      │                          │                             │
      │   (lease expires)        │  delete IAM user            │
      │                          │ ──────────────────────────► │
```

Vault itself needs an AWS identity with permission to create/delete IAM users. That's the **root config**.

---

## ⚠️ Before you start

- Use a **sandbox/personal AWS account**, never production.
- Create an IAM user for Vault (e.g., `vault-admin`) with permission to manage IAM users, and generate an access key for it. HashiCorp's docs list the exact minimum IAM policy: https://developer.hashicorp.com/vault/docs/secrets/aws
- Clean up at the end.

---

## 1️⃣ Enable the AWS engine

```bash
vault secrets enable -path=aws aws
```

## 2️⃣ Give Vault its AWS credentials

```bash
vault write aws/config/root \
    access_key="$AWS_ACCESS_KEY_ID" \
    secret_key="$AWS_SECRET_ACCESS_KEY" \
    region=us-east-1
```

> 💡 Using env vars keeps the real keys out of your shell history.

## 3️⃣ Create a role (what the generated creds can do)

This role says: "generated users can do anything with EC2."

```bash
vault write aws/roles/my-ec2-role \
    credential_type=iam_user \
    policy_document=-<<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:*",
      "Resource": "*"
    }
  ]
}
EOF
```

## 4️⃣ Generate credentials 🎉

```bash
vault read aws/creds/my-ec2-role
```
```
Key                Value
---                -----
lease_id           aws/creds/my-ec2-role/AbCdEf123...
lease_duration     768h
lease_renewable    true
access_key         AKIA...
secret_key         ...
```

Go to the **AWS Console → IAM → Users**: there's a brand-new user named something like `vault-token-my-ec2-role-...`. Vault made it.

Run it again → **a different user and key.** Every request is unique.

## 5️⃣ Revoke it

```bash
vault lease revoke aws/creds/my-ec2-role/AbCdEf123...
```

Refresh IAM in the console → the user is **gone**.

Revoke **every** cred from that role at once (e.g., during an incident):
```bash
vault lease revoke -prefix aws/creds/my-ec2-role
```

---

## ⏱️ Make creds expire faster

The default 768h (32 days) is long. Shorten it:
```bash
vault secrets tune -default-lease-ttl=15m -max-lease-ttl=1h aws/
```

---

## 🧹 Clean up

```bash
vault lease revoke -prefix aws/
vault secrets disable aws/
```
Then delete/disable the `vault-admin` access key in AWS if you're done.

---

## ✍️ Check yourself

1. Who creates the IAM user — you, or Vault?
2. What does the **role** define?
3. What does `lease_id` let you do?
4. A key leaked during an incident. What single command kills every credential from that role?

<details>
<summary>Answers</summary>

1. Vault.
2. What permissions the generated credentials get (the IAM policy) and the credential type.
3. Renew or revoke that specific credential.
4. `vault lease revoke -prefix aws/creds/<role>`

</details>
