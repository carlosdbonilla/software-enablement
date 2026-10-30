# 02 — Elastic in One Page

[← Prev](01-the-role.md) · [Index](00-index.md) · [Next: How Elasticsearch Works →](03-how-elasticsearch-works.md)

---

## 🎯 Goal
Explain what Elastic is, how it makes money, and why it wins.

## 💡 The Idea

**Elastic is "the Search AI Company."** It makes a platform that takes in huge amounts of data, makes it searchable in seconds, and runs **security, observability and AI** on that same data.

> **Analogy:** Elastic is a **giant, instant index for all of an organization's data**. Ask any question, from "which laptop talked to this bad IP?" to "why did the logistics app crash?" to "what does our policy say about X?", and get the answer in seconds.

### The company

| | |
|---|---|
| **Origin** | Shay Banon created Elasticsearch as open source in 2010; Elastic founded 2012 |
| **Public** | NYSE: **ESTC** (IPO 2018). Fiscal year ends **April 30** |
| **CEO** | Ash Kulkarni |
| **Size** | FY26 revenue ≈ **$1.7B**, about **+17%**; Q4 FY26 revenue $451M (+16%); **1,800+ customers** paying over $100K/yr |
| **Key acquisition for you** | **Endgame (2019)** → endpoint security → Elastic Defend |

### One platform, three solutions

```
         SEARCH & AI          OBSERVABILITY            SECURITY
     find info, power AI    keep systems running    stop attackers
  ─────────────────────────────────────────────────────────────────
                  KIBANA  (see it, explore it, act on it)
               ELASTICSEARCH  (store, search, analyze)
          INTEGRATIONS / ELASTIC AGENT  (collect any data)
  ─────────────────────────────────────────────────────────────────
     Serverless · Elastic Cloud · Self-managed · On-prem · Air-gapped
```

### How Elastic makes money
- **Free and open core** → developers try it, adoption spreads.
- Customers pay for **Elastic Cloud**, **advanced features** (security, ML, AI) and **support**.
- Pricing is **resource-based** (compute/storage), not per user. That matters vs. per-GB competitors ([Lesson 10](10-competition-and-objections.md)).
- Growth comes from **land and expand**: start with one use case (say, logs) and grow into SIEM, endpoint and AI.

### License history (it comes up)
- **2021:** Elastic moved off the Apache 2.0 license to stop cloud providers reselling it. **AWS forked it** into **OpenSearch**.
- **2024:** Elastic added the **AGPL** open-source license back as an option.
- **Why it matters:** OpenSearch is a common "free alternative" objection.

### Why Elastic wins (the 4 pillars)

| Pillar | Why it matters in DoD |
|---|---|
| **Speed at scale** | Search petabytes in seconds — analysts find threats fast |
| **One platform, one data copy** | Collect once, use it for security, ops and AI → fewer tools, lower cost |
| **Runs anywhere** | Cloud, on-prem, **air-gapped, tactical edge** — most competitors can't |
| **Open & flexible** | No lock-in; bring your own AI models; open data schema |

## 🔑 Key Terms

| Term | Meaning |
|---|---|
| **Elasticsearch** | The core search and analytics engine |
| **Kibana** | The user interface |
| **Elastic Cloud** | Elastic's hosted SaaS (on AWS, Azure, Google Cloud) |
| **Self-managed** | Customer runs it on their own infrastructure |
| **Air-gapped** | Completely disconnected from the internet |
| **OpenSearch** | AWS's 2021 fork of Elasticsearch |
| **Land & expand** | Win one use case, then grow |

## 🗣️ Say It — your 30-second pitch
> "Elastic is the Search AI Company. We take in all of an agency's data, from logs and endpoints to applications and documents, and make it searchable in seconds. On that one platform, agencies detect and stop cyber threats, keep mission systems running, and build trusted AI on their own data. And it runs wherever the mission is: in the cloud, on-prem, or fully air-gapped at the tactical edge."

Say it 3 times. Then say it without reading.

## ⚠️ Watch Out
- Don't call Elastic "just a search engine" or "just a logging tool." It's a **platform** with three solutions.
- Don't quote numbers you're unsure of. "About $1.7B, growing high teens" is enough.

## 🧠 Recall
1. What are Elastic's three solutions?
2. How does Elastic make money if the core is free?
3. Why does OpenSearch exist?
4. Which pillar matters most for classified and tactical environments?

<details><summary>Answers</summary>

1. Search & AI, Observability, Security.
2. Cloud subscriptions, paid features (security, ML, AI) and support.
3. AWS forked Elasticsearch in 2021 after Elastic changed its license.
4. "Runs anywhere": on-prem, air-gapped, at the edge.
</details>

## 📌 Remember
- **Search AI Company** · one platform · **three solutions**
- Free core → paid cloud/features/support → **land & expand**
- Wins on **speed, one platform, runs anywhere, open**

---

[← Prev](01-the-role.md) · [Index](00-index.md) · [Next: How Elasticsearch Works →](03-how-elasticsearch-works.md)
