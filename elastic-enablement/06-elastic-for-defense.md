# 06 — Elastic for Defense: Use Cases & Proof Points

[← Prev](05-dod-101.md) · [Index](00-index.md) · [Next: How DoD Buys →](07-how-dod-buys.md)

---

## 🎯 Goal
Tell three mission stories, each backed by real proof points.

## 💡 The Idea
Defense buyers trust **what's already working in DoD**. Your job is to match their problem to a story they recognize.

> **Story formula:** **Situation → Problem → Elastic → Outcome → Proof**

## Where Elastic fits in Defense

| Use case | Mission problem | Elastic answer |
|---|---|---|
| **Cyber defense (SIEM)** | Threats hide across NIPR and SIPR; data volume is exploding | SIEM at massive scale, a year+ of searchable data |
| **Endpoint modernization** | Aging endpoint security (DoD relied on the legacy **HBSS** for 20+ years) | Elastic Defend: modern prevention + detection + response |
| **Tactical edge** | Forward units, ships and fly-away kits are disconnected (DIL) | Runs fully air-gapped; syncs when connected |
| **Data fusion / decision support** | Sensor, UAV, logistics and intel data in different formats and silos | Ingest anything, search across clusters → faster decisions (CJADC2) |
| **Mission system observability** | Logistics, C2 and enterprise apps go down; root cause takes hours | Logs + metrics + traces in one place |
| **Secure AI** | AI pilots can't touch classified data or hallucinate | RAG and agents on agency data with approved or air-gapped models |

## Proof points (memorize these 6)

| # | Proof point | Use it when… |
|---|---|---|
| 1 | **U.S. Air Force cyberspace defense:** Elastic SIEM across NIPR and SIPR covering **600,000+ endpoints** | They doubt scale or classified-network support |
| 2 | **CISA SIEM-as-a-Service:** CISA partnered with Elastic (announced Dec 2025) to standardize cyber defense, so civilian agencies can adopt Elastic Security through CISA | They want a government-wide standard or reference |
| 3 | **FedRAMP High** (AWS GovCloud) and **IL5-aligned** controls | Compliance and authorization come up |
| 4 | **Google Distributed Cloud air-gapped:** Elastic Security is the embedded security layer | Air-gapped or sovereign cloud requirement |
| 5 | **OpenAI GPT cyber models in Elastic Security** (Sept 2026), with a model-agnostic design so agencies can use their own approved models | AI in the SOC, or "can AI work in our environment?" |
| 6 | **Endgame heritage:** endpoint tech built for defense and intelligence customers | Endpoint credibility |

> Verify any proof point with your SA or marketing before using it in writing. Public references can change.

## Three stories to practice

### Story 1 — The SOC drowning in data (Security)
- **Situation:** A Defense Agency SOC monitors hundreds of thousands of endpoints across NIPR and SIPR.
- **Problem:** Its legacy SIEM charges by data volume, so it keeps only 30 days. Investigations stall, and analysts face thousands of alerts a day.
- **Elastic:** One platform for SIEM + endpoint; data tiers keep a year searchable at low cost; Attack Discovery groups alerts into likely attacks.
- **Outcome:** Faster detection and response, more data retained, fewer tools.
- **Proof:** Air Force — 600K+ endpoints on NIPR/SIPR.

### Story 2 — The disconnected edge (Tactical)
- **Situation:** Deployed units run systems on ships or at forward bases with limited connectivity.
- **Problem:** Cloud-only tools don't work there, so the edge is a blind spot.
- **Elastic:** The same platform runs air-gapped at the edge; cross-cluster replication syncs when a link is up.
- **Outcome:** Consistent security and visibility from the enterprise to the edge.
- **Proof:** Google Distributed Cloud air-gapped; tactical endpoint coverage.

### Story 3 — Trusted AI on mission data (Search & AI)
- **Situation:** Leadership wants AI assistants for analysts and planners.
- **Problem:** Pilots hallucinate, can't reach the right data, or can't run on classified networks.
- **Elastic:** RAG grounded in agency data, document-level access control, any approved model (including air-gapped ones).
- **Outcome:** AI that gives sourced answers and respects classification and access.
- **Proof:** Model-agnostic AI; OpenAI cyber models available in Elastic Security.

## 🔑 Key Terms

| Term | Meaning |
|---|---|
| **HBSS** | Host Based Security System, DoD's legacy endpoint security |
| **Fly-away kit** | Portable, deployable IT system |
| **IL5** | DoD Impact Level 5: controlled unclassified and mission-critical data |
| **FedRAMP High** | Highest FedRAMP cloud authorization baseline |
| **Attack Discovery** | Elastic AI feature that groups alerts into likely attacks |

## 🗣️ Say It
Tell **each story out loud in 60 seconds or less** using the formula. Record Story 1 and listen back. Did you lead with the mission problem?

## ⚠️ Watch Out
- Don't open with a proof point. Earn it: problem first, then proof.
- Don't overstate. "IL5-aligned controls" is not the same as "IL5 authorized for your system." Your SA confirms specifics.

## 🧠 Recall
1. What's the story formula?
2. What is the Air Force proof point?
3. What did CISA and Elastic announce?
4. Which proof point answers "can it run air-gapped?"
5. What legacy DoD endpoint system did Elastic position against?

<details><summary>Answers</summary>

1. Situation → Problem → Elastic → Outcome → Proof.
2. Elastic SIEM across NIPR and SIPR covering 600,000+ endpoints.
3. A partnership (Dec 2025) for Elastic to power SIEM-as-a-Service and standardize cyber defense for civilian agencies.
4. Google Distributed Cloud air-gapped (plus self-managed and air-gapped deployments generally).
5. HBSS.
</details>

## 📌 Remember
- **Problem first, proof second**
- 3 stories: **SOC · Edge · Trusted AI**
- 6 proofs: **Air Force · CISA · FedRAMP High/IL5 · GDC air-gap · OpenAI models · Endgame**

---

[← Prev](05-dod-101.md) · [Index](00-index.md) · [Next: How DoD Buys →](07-how-dod-buys.md)
