# 12 — Review Sheet, Glossary & Flashcards

[← Prev](11-interview-prep.md) · [Index](00-index.md)

---

Use this page for spaced review and **right before every interview**. Cover the answers, say them out loud, then check.

## The whole path on one screen

```
 ELASTIC                       DEFENSE                       SELLING
 Search AI Company             Territory: 4th Estate (DISA)  Discovery: Current→Pain→Impact→Future→Process
 3 solutions on 1 platform     NIPR · SIPR · JWICS · edge    Qualify: MEDDPICC
 Runs anywhere (air-gapped)    Zero Trust FY27 · CJADC2      Objections: Acknowledge→Ask→Answer→Advance
 Land & expand                 Buy: Money·Vehicle·Partner·Timing   Interview: Story · STAR · Territory plan
```

## 🗣️ The 30-second pitch (say it from memory)
> "Elastic is the Search AI Company. We take in all of an agency's data, from logs and endpoints to applications and documents, and make it searchable in seconds. On that one platform, agencies detect and stop cyber threats, keep mission systems running, and build trusted AI on their own data. And it runs wherever the mission is: in the cloud, on-prem, or fully air-gapped at the tactical edge."

## Key frameworks

| Framework | Steps |
|---|---|
| **Story formula** | Situation → Problem → Elastic → Outcome → Proof |
| **Discovery** | Current → Pain → Impact → Future → Process |
| **Deal map** | Money · Vehicle · Partner · Timing |
| **MEDDPICC** | Metrics · Economic buyer · Decision criteria · Decision process · Paper · Identify pain · Champion · Competition |
| **Objections** | Acknowledge → Ask → Answer → Advance |
| **Your story** | Present → Past → Future |
| **Experience answers** | Situation → Task → Action → Result |

## Proof points

| | |
|---|---|
| U.S. Air Force | Elastic SIEM on NIPR + SIPR, **600K+ endpoints** |
| CISA | Partnership (Dec 2025) to standardize civilian cyber defense / SIEM-as-a-Service |
| Compliance | **FedRAMP High** (AWS GovCloud), **IL5-aligned** controls |
| Air-gapped | Embedded in **Google Distributed Cloud air-gapped** |
| AI | **OpenAI GPT cyber models** in Elastic Security (Sept 2026); bring your own model |
| Heritage | **Endgame** (2019) → Elastic Defend |

## Competitor angles

| vs. | Angle |
|---|---|
| Splunk | Cost at scale; keep more data; one platform |
| Microsoft Sentinel | Runs anywhere, open |
| CrowdStrike | Air-gap/DIL; endpoint + SIEM + data in one |
| OpenSearch | Total cost, support, security and AI features |
| Status quo | Pain + compelling event |

## Glossary

| Term | Meaning |
|---|---|
| **AE / SA / SI** | Account Executive / Solutions Architect / System Integrator |
| **OTE** | On-Target Earnings (pay at 100% of quota) |
| **Elasticsearch / Kibana** | Engine / user interface |
| **Elastic Agent / Fleet** | Data collector / its central management |
| **Elastic Defend** | Endpoint protection (from Endgame) |
| **Inverted index** | Why search is fast (book-index lookup) |
| **Shard / replica / node / cluster** | Data slice / backup copy / server / group of servers |
| **ILM / data tiers** | Auto-move data hot → frozen to cut cost |
| **Searchable snapshots** | Cheap storage that stays searchable |
| **CCS / CCR** | Cross-cluster search / replication |
| **SIEM / XDR / EDR** | Security log analytics / extended & endpoint detection and response |
| **APM / OTel** | App performance monitoring / OpenTelemetry |
| **RAG / vector search** | AI grounded in retrieved data / search by meaning |
| **DoD / OSD** | Department of Defense / Office of the Secretary of Defense |
| **4th Estate** | Defense Agencies and Field Activities outside the military departments |
| **DISA** | Defense Information Systems Agency — DoD's IT backbone |
| **DLA / DHA / DTRA / DCSA** | Logistics / Health / Threat Reduction / Counterintelligence & Security agencies |
| **DODIN** | DoD Information Network |
| **NIPR / SIPR / JWICS** | Unclassified / Secret / Top Secret networks |
| **DIL** | Disconnected, intermittent, limited |
| **IC** | Intelligence Community |
| **HBSS** | Legacy DoD endpoint security |
| **Zero Trust** | Verify every user/device/request; DoD target FY2027 |
| **CJADC2** | Connect all forces' data for faster decisions |
| **JWCC** | DoD's enterprise cloud contract (AWS, Google, Microsoft, Oracle) |
| **FY** | Federal fiscal year, Oct 1 – Sep 30 |
| **CR** | Continuing resolution, no new starts |
| **PPBE / POM** | DoD multi-year budget process |
| **Contract vehicle / IDIQ / BPA** | Pre-competed buying agreements |
| **GSA MAS / SEWP / ESI / CHESS** | Common vehicles |
| **OTA** | Other Transaction Authority (fast prototyping path) |
| **ATO / RMF** | Authority to Operate / Risk Management Framework |
| **IL2–IL6** | DoD cloud Impact Levels (IL6 = Secret) |
| **FedRAMP** | Federal cloud authorization program |
| **CO** | Contracting officer |
| **MEDDPICC** | Deal qualification checklist |
| **TCO** | Total cost of ownership |

---

## 🧠 Flashcards

### Elastic
<details><summary>1. What are Elastic's 3 solutions?</summary>Search & AI, Observability, Security — all on one platform.</details>

<details><summary>2. How does Elastic make money?</summary>Free/open core drives adoption; customers pay for cloud, advanced features and support; growth through land and expand.</details>

<details><summary>3. Why is Elasticsearch fast?</summary>The inverted index: like a book's index, it jumps to matching documents instead of scanning all data.</details>

<details><summary>4. How does Elastic keep long-term data affordable?</summary>Data tiers + ILM move old data to cheaper storage; searchable snapshots keep it searchable.</details>

<details><summary>5. What happens if a node fails?</summary>A replica on another node is promoted to primary — no data loss.</details>

<details><summary>6. What is cross-cluster search good for in DoD?</summary>One view across bases, enclaves and regions without moving the data.</details>

<details><summary>7. What is RAG?</summary>AI answers grounded in documents retrieved from the agency's own data.</details>

<details><summary>8. Where did Elastic Defend come from?</summary>The 2019 Endgame acquisition.</details>

### Defense
<details><summary>9. What is the 4th Estate?</summary>DoD agencies and field activities outside the military departments and combatant commands (DISA, DLA, DHA…).</details>

<details><summary>10. What does DISA do?</summary>Runs DoD's IT backbone: networks, enterprise services, cloud and DODIN cyber defense.</details>

<details><summary>11. NIPR, SIPR, JWICS?</summary>Unclassified, Secret, Top Secret/SCI networks.</details>

<details><summary>12. Which Zero Trust pillars map to Elastic Security?</summary>Visibility & analytics and automation (SIEM/SOC), plus device (endpoint). Target: FY2027.</details>

<details><summary>13. What's Elastic's biggest DoD proof point?</summary>Air Force: Elastic SIEM across NIPR and SIPR, 600K+ endpoints.</details>

<details><summary>14. Why does "runs anywhere" matter in DoD?</summary>Classified, air-gapped and tactical edge networks can't use cloud-only tools.</details>

### Buying
<details><summary>15. When is the federal fiscal year?</summary>Oct 1 – Sep 30. Q4 (Jul–Sep) is year-end spend.</details>

<details><summary>16. What is a CR, and why does it matter?</summary>A continuing resolution: prior-year funding levels, usually no new starts, so deals slip.</details>

<details><summary>17. The 4 questions for every DoD deal?</summary>Money (who funds it), Vehicle (how it's bought), Partner (who holds the paper), Timing (when funds can be obligated).</details>

<details><summary>18. Why are SIs so important?</summary>They're often in the building, design the architecture and hold the contract.</details>

<details><summary>19. What is an ATO?</summary>Authority to Operate: approval to run software on a DoD network under RMF. Takes months.</details>

### Selling
<details><summary>20. The discovery flow?</summary>Current → Pain → Impact → Future → Process.</details>

<details><summary>21. MEDDPICC?</summary>Metrics, Economic buyer, Decision criteria, Decision process, Paper process, Identify pain, Champion, Competition.</details>

<details><summary>22. What makes a DoD deal qualified?</summary>Mission pain, identified funding, a contract path, a tested champion, and a compelling event.</details>

<details><summary>23. Elastic vs. Splunk in one line?</summary>Keep more data searchable for less, on one platform for security, ops and AI.</details>

<details><summary>24. Elastic vs. Microsoft Sentinel in one line?</summary>Runs anywhere (on-prem, air-gapped, any cloud) and isn't tied to one ecosystem.</details>

<details><summary>25. Objection-handling steps?</summary>Acknowledge → Ask → Answer → Advance.</details>

### Interview
<details><summary>26. Your story structure?</summary>Present → Past → Future, 60–90 seconds.</details>

<details><summary>27. How do you handle the DoD experience gap?</summary>Name it, show what you've learned, explain your 90-day plan, then ask if anything concerns them.</details>

<details><summary>28. 30-60-90?</summary>Learn → Engage → Build pipeline.</details>

---

## 🏁 Final challenge (do this before the hiring manager round)

Out loud, without notes:
1. Give the **30-second pitch**.
2. Explain **why Elastic is fast and affordable** with the book-index and data-tier ideas.
3. Explain **the 4th Estate and DISA**.
4. Tell **Story 1 (the SOC)** using the story formula.
5. Run a **5-question discovery** with an imaginary DISA CISO.
6. Score that deal with **MEDDPICC**.
7. Handle **"We're a Splunk shop."**
8. Give your **60-second story** and your **gap answer**.

If you can do all 8 comfortably, you're ready. 🎯

---

[← Prev](11-interview-prep.md) · [Index](00-index.md)
