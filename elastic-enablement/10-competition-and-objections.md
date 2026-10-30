# 10 — Competition & Objection Handling

[← Prev](09-qualifying-meddpicc.md) · [Index](00-index.md) · [Next: Interview Prep →](11-interview-prep.md)

---

## 🎯 Goal
Know who you'll compete with, where Elastic is different, and how to handle the most common objections.

## 💡 The Idea
**Never trash a competitor.** DoD buyers often have years invested in them. Respect the incumbent, then **ask questions that reveal the gap**.

> **Analogy:** You're not telling them their car is junk. You're asking, *"How's the mileage? What does maintenance cost? Can it go off-road?"* and letting them notice for themselves.

## The competitive landscape

| Area | Main competitors |
|---|---|
| **SIEM** | **Splunk** (owned by Cisco), **Microsoft Sentinel**, Google SecOps |
| **Endpoint / XDR** | **CrowdStrike**, **Trellix** (successor to McAfee, the vendor behind DoD's legacy HBSS), Microsoft Defender, SentinelOne |
| **Observability** | Splunk, Datadog, Dynatrace |
| **Search / vector** | **OpenSearch**, cloud-native AI search services |
| **Always** | **Status quo** — doing nothing |

## Where each competitor is strong, and where Elastic differs

| Competitor | Where they're strong | Elastic's angle (ask about it) |
|---|---|---|
| **Splunk** | Deep DoD footprint, mature SIEM, loyal users | **Cost as data grows** (ingest-based pricing makes customers drop data); Elastic keeps more searchable for less; one platform for SIEM + endpoint + observability + AI |
| **Microsoft Sentinel / Defender** | Bundled with Microsoft licensing; strong in Azure | **Runs anywhere:** Sentinel is cloud-based; Elastic runs on-prem, air-gapped and on any cloud. Open, not tied to one ecosystem |
| **CrowdStrike** | Leading endpoint brand, strong threat intel | **Air-gapped and DIL support**; endpoint + SIEM + data in one platform rather than a separate data lake |
| **Trellix** | Incumbent from HBSS days | **Modernization:** a modern platform vs. legacy architecture |
| **OpenSearch** | "Free," AWS-backed | **Total cost:** who maintains, secures and supports it? Elastic's security, ML and AI features, plus vendor support |
| **Datadog / Dynatrace** | Easy SaaS observability | **Deployment flexibility** (classified/on-prem) and **one platform** with security |

## Common objections

Use **Acknowledge → Ask → Answer → Advance**:

> *"That makes sense..."* → *"Can I ask...?"* → *"Here's how others handle that..."* → *"Would it help to...?"*

| Objection | Ask first | Then answer |
|---|---|---|
| **"We already use Splunk."** | "How's cost trending as data grows? Do you drop any data to stay in budget?" | Elastic keeps more data searchable for less, and consolidates SIEM + endpoint + observability. Start by offloading high-volume data. |
| **"OpenSearch is free."** | "Who supports it at 2 a.m.? Who handles security patches and new features?" | Compare total cost: engineering time, support, security and AI features. Free to download isn't free to run. |
| **"We can't use cloud."** | "Which networks and classification levels do you need to cover?" | Elastic runs self-managed, on-prem and air-gapped; FedRAMP High is available when cloud is allowed. |
| **"AI can't run in our environment."** | "Which models are approved for you?" | Elastic's AI is model-agnostic: use government-managed or air-gapped models, with no public internet required. |
| **"We just renewed our endpoint tool."** | "When's the next renewal? What other pain is there, in SIEM or ops?" | Land with SIEM or observability now; plan for the endpoint renewal. Land and expand. |
| **"Procurement takes forever."** | "Which vehicles and integrators do you already use?" | Use existing vehicles and SI partners; align to FY timing; consider a pilot on an existing contract. |
| **"It's too expensive."** | "Compared to what? What does it cost to keep today's setup, including dropped data and extra tools?" | Reframe to TCO and mission value: tools retired, data retained, analyst time saved. |
| **"Now isn't a good time."** | "What would need to happen for this to be a priority?" | Find the compelling event (renewal, mandate, audit, FY end) and stay close. |

## 🔑 Key Terms

| Term | Meaning |
|---|---|
| **Incumbent** | The vendor already in place |
| **Ingest-based pricing** | Price grows with the amount of data collected |
| **TCO** | Total cost of ownership: license + people + infrastructure + risk |
| **Displacement** | Replacing an incumbent |
| **Wedge** | A small first use case that gets you in the door |

## 🗣️ Say It
Have a friend (or a voice memo) throw each objection at you. Answer with **Acknowledge → Ask → Answer → Advance**, in 30 seconds each.

## ⚠️ Watch Out
- **Never disparage.** Say "different," not "worse."
- **Don't guess** competitor features or certifications. Facts change; check with your SA or competitive team.
- The best displacement strategy is often a **wedge**: start where the incumbent is weakest or most expensive.

## 🧠 Recall
1. Who are the main SIEM competitors?
2. What's Elastic's main angle against Splunk?
3. Against Microsoft Sentinel?
4. What are the 4 steps for handling objections?
5. How do you answer "OpenSearch is free"?

<details><summary>Answers</summary>

1. Splunk (Cisco), Microsoft Sentinel, Google SecOps.
2. Cost at scale (keep more data searchable for less) and one consolidated platform.
3. Runs anywhere: on-prem, air-gapped, any cloud, not tied to one ecosystem.
4. Acknowledge → Ask → Answer → Advance.
5. Ask who supports and secures it; compare total cost, including engineering time, support and missing features.
</details>

## 📌 Remember
- **Respect, then reveal** — never trash the competition
- Splunk = **cost**, Microsoft = **runs anywhere**, CrowdStrike = **air-gap + one platform**, OpenSearch = **TCO**
- **Acknowledge → Ask → Answer → Advance**

---

[← Prev](09-qualifying-meddpicc.md) · [Index](00-index.md) · [Next: Interview Prep →](11-interview-prep.md)
