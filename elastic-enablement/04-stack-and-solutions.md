# 04 — The Elastic Stack & the Three Solutions

[← Prev](03-how-elasticsearch-works.md) · [Index](00-index.md) · [Next: DoD 101 →](05-dod-101.md)

---

## 🎯 Goal
Know the products, and map each solution to the customer problem it solves.

## 💡 The Idea
The **Elastic Stack** is the foundation. The **three solutions** are what customers actually buy.

> **Analogy:** The stack is the **kitchen** (stove, fridge, knives). The solutions are the **meals on the menu**. Customers order meals, but every meal comes out of the same kitchen. That's why adding a second or third use case is easy.

## The stack: data in → store & search → see & act

| Layer | Product | Job |
|---|---|---|
| **Collect** | **Elastic Agent** (managed by **Fleet**) | One agent per machine: collects logs, metrics and security data, and protects the endpoint |
| | **Integrations** | Hundreds of prebuilt connectors (AWS, Cisco, Palo Alto, Windows, Linux…) |
| | **Logstash** | Server-side pipeline to transform data from anywhere |
| | **Beats** | Older single-purpose data shippers (Filebeat, Metricbeat) |
| **Store & search** | **Elasticsearch** | Store, search, analyze ([Lesson 03](03-how-elasticsearch-works.md)) |
| **See & act** | **Kibana** | Dashboards, investigation, alerts, admin |
| **Common language** | **Elastic Common Schema (ECS)** | Standard field names so data from different sources can be correlated (contributed to OpenTelemetry) |

### Deployment options (say these from memory)
**Serverless · Elastic Cloud (hosted) · Self-managed · On-prem · Air-gapped · Edge**

---

## Solution 1: Elastic Security 🛡️

**Problem:** Attackers hide in mountains of data, SOCs are overloaded, and legacy SIEMs cost too much as data grows.

| Capability | What it does |
|---|---|
| **SIEM** | Collects security logs, detects threats with prebuilt rules and ML, supports investigations |
| **Elastic Defend (endpoint/XDR)** | Prevents malware and ransomware on laptops and servers, plus detection and response. Comes from Endgame |
| **Threat hunting** | Search months of data quickly to find hidden attackers |
| **Cloud security** | Posture and workload protection for cloud environments |
| **AI for the SOC** | AI Assistant, **Attack Discovery** (AI groups related alerts into likely attacks), automated triage |

**One-liner:** *"SIEM, endpoint and AI-driven SOC on one platform, with a year of data searchable instead of a week."*

## Solution 2: Elastic Observability 📈

**Problem:** Outages take too long to diagnose because logs, metrics and traces live in separate tools.

| Capability | What it does |
|---|---|
| **Log analytics** | All logs centralized and searchable |
| **Infrastructure monitoring** | Servers, networks, containers, Kubernetes |
| **APM** | Traces a slow transaction through an application |
| **AIOps** | ML anomaly detection and root-cause help |
| **OpenTelemetry** | Vendor-neutral data collection, natively supported |

**One-liner:** *"Find out why a mission system is down in minutes, not hours, with all signals in one place."*

## Solution 3: Elastic Search & AI 🔍

**Problem:** People can't find information across silos, and AI pilots give wrong answers because they can't reach trusted data.

| Capability | What it does |
|---|---|
| **Enterprise search** | Search documents across systems |
| **Vector database** | Search by meaning (semantic), combined with keywords (**hybrid search**) |
| **RAG** | *Retrieval-Augmented Generation*: AI answers grounded in the agency's own documents |
| **Agent Builder** | Build AI agents that query and act on agency data |
| **Bring your own model** | Works with government-approved or air-gapped AI models |

> **RAG analogy:** An AI model alone is a smart person answering from memory. Sometimes they make things up. **RAG gives them the right files to read first.** Elastic finds those files.

**One-liner:** *"Trusted AI grounded in your own data, with your access controls, even air-gapped."*

---

## Why one platform matters (your consolidation story)

```
 BEFORE                                   AFTER
 SIEM tool        ─┐                      ┌─ Security
 Endpoint tool     ├─ 4 contracts,        │
 Logging tool      │  4 data copies  ──▶  ├─ Observability   one platform,
 Search/AI tool   ─┘  4 skill sets        └─ Search & AI     one data copy
```
**Value:** fewer licenses, less data duplication, fewer skills to train, and correlation across security and ops data.

## 🔑 Key Terms

| Term | Meaning |
|---|---|
| **Elastic Agent / Fleet** | One collection agent / central management for it |
| **ECS** | Elastic Common Schema: standard field names |
| **SIEM** | Security Information & Event Management |
| **XDR / EDR** | Extended / Endpoint Detection & Response |
| **Elastic Defend** | Elastic's endpoint protection (from Endgame) |
| **APM** | Application Performance Monitoring |
| **OpenTelemetry (OTel)** | Open standard for collecting telemetry |
| **Vector search / hybrid search** | Search by meaning / meaning + keywords |
| **RAG** | AI grounded in retrieved documents |

## 🗣️ Say It
Give each solution's one-liner out loud. Then answer: *"A CIO says, 'We have separate tools for SIEM, endpoint and logs.' What's your 20-second response?"*

## ⚠️ Watch Out
- Customers buy **outcomes**, not products. Lead with the problem, then name the product.
- Don't promise features in a specific environment (e.g., "AI works on SIPR") without checking with your SA.

## 🧠 Recall
1. What are the three layers of the stack?
2. What does Elastic Defend do, and where did it come from?
3. What is RAG, in one sentence?
4. Name 3 benefits of one platform vs. many tools.

<details><summary>Answers</summary>

1. Collect (Agent, integrations, Logstash), store & search (Elasticsearch), see & act (Kibana).
2. Endpoint protection (malware/ransomware prevention, detection & response); it came from the 2019 Endgame acquisition.
3. AI answers that are grounded in documents retrieved from the agency's own data.
4. Fewer licenses, one data copy, fewer skills to train, cross-domain correlation (any 3).
</details>

## 📌 Remember
- **Kitchen (stack) → meals (solutions)**
- **Security · Observability · Search & AI** — know a one-liner for each
- **Consolidation** is the big value story

---

[← Prev](03-how-elasticsearch-works.md) · [Index](00-index.md) · [Next: DoD 101 →](05-dod-101.md)
