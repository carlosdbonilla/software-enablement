# 13 — Explain It Like an SE

## 🎯 Goal

Turn the technical stuff into a **customer conversation**. Lead with problems, not features.

---

## 🗣️ The 30-second pitch

> "Most companies have passwords, API keys, and cloud credentials scattered across code, config files, and chat, and nobody can say who has access to what. Vault gives you **one secure place** for those secrets, ties access to **identity**, and can **generate credentials on demand that expire on their own**. So a leaked key is useless in minutes, and you have an **audit trail** of every access."

---

## 🔍 Discovery questions

Use these to find pain before you mention a single feature:

1. "Where do your applications get their database passwords and API keys today?"
2. "If a developer leaves tomorrow, how do you make sure the credentials they had are rotated?"
3. "How often do you rotate your cloud access keys? What does that process look like?"
4. "Have you ever had a secret committed to a Git repo? What happened next?"
5. "When an auditor asks 'who accessed production credentials last quarter,' how long does it take to answer?"
6. "How do your apps and pipelines prove who they are to cloud providers and databases?"
7. "How are you managing TLS certificates? Ever had an outage from one expiring?"

---

## 🧭 Pain → Capability map

| Customer says… | Vault capability | Module |
|---|---|---|
| "Secrets are everywhere" | Central KV store + policies | 03, 07 |
| "We share AWS keys that never expire" | Dynamic AWS secrets | 05 |
| "Rotating DB passwords breaks apps" | Database secrets engine (dynamic creds) | 04 |
| "Auditors keep asking who touched what" | Audit devices | 09 |
| "We need to encrypt PII but devs aren't crypto experts" | Transit | 11 |
| "Expired certs keep causing outages" | PKI engine | 04 |
| "We're multi-cloud and every cloud does IAM differently" | One identity + policy layer across clouds | 06 |
| "We already use Terraform" | Vault provider, config as code, dynamic creds for pipelines | 12 |

---

## 💼 Business outcomes (what execs care about)

- **Reduce breach risk:** short-lived credentials shrink the window an attacker has
- **Pass audits faster:** centralized logs of every secret access
- **Developer speed:** self-service secrets instead of tickets to the security team
- **Consistency across clouds:** one workflow whether it's AWS, Azure, GCP, or on-prem

---

## 🥊 Objection handling (starter answers)

| Objection | Response angle |
|---|---|
| "We already use AWS Secrets Manager." | Great for AWS-only. Vault adds multi-cloud/on-prem consistency, dynamic secrets across many systems, encryption as a service, and one policy model everywhere. |
| "Vault seems complex to run." | HCP Vault Dedicated is fully managed. Self-managed Vault uses built-in Raft storage, so there's no extra database to run. |
| "Our developers won't adopt it." | Vault Agent and the Kubernetes integrations can deliver secrets to apps without code changes. |
| "Isn't it open source? Why pay?" | Community is free for core features. Enterprise/HCP add scale and governance features (namespaces, replication, HSM support, support SLAs). |

---

## 🎬 Demo flow that lands (10 minutes)

1. **The problem:** show a fake `.env` file with a hardcoded AWS key
2. **Store a static secret** in KV, read it with a scoped policy (01 → 03 → 07)
3. **The wow moment:** `vault read aws/creds/my-role` → show the IAM user appear in the console → revoke → it disappears (05)
4. **Audit log:** show the entry for what you just did (09)
5. **Tie to Terraform:** "your pipelines can do the same thing, with no static keys" (12)

---

## ✍️ Practice

- Say the 30-second pitch out loud 3 times without reading it.
- Pick 3 discovery questions and write how a "bad" vs "good" answer from a customer changes your next move.
