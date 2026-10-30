# 08 — Buyers & Discovery

[← Prev](07-how-dod-buys.md) · [Index](00-index.md) · [Next: MEDDPICC →](09-qualifying-meddpicc.md)

---

## 🎯 Goal
Know who's in a DoD deal, what each person cares about, and what to ask them.

## 💡 The Idea
**Discovery** = asking questions to understand the customer's problem *before* pitching.

> **Analogy:** A good AE is a **doctor, not a pharmacist**. A pharmacist hands over what you ask for. A doctor asks questions, finds the real problem, then prescribes. Pitching before discovery is **prescribing before diagnosing**.

## The people in a deal

| Persona | Cares about | Role in the deal | Speak to them about |
|---|---|---|---|
| **Mission owner / commander** | Readiness, mission outcomes, decision speed | Sponsor; defines the "why" | Mission impact, risk |
| **CIO / CTO** | Modernization, consolidation, cost, architecture | Economic buyer or key influencer | Tool consolidation, TCO, standards |
| **CISO / cyber lead** | Threat coverage, compliance, SOC performance | Economic buyer or champion (security) | Detection, Zero Trust, ATO |
| **SOC manager / analysts** | Fewer false positives, speed, usability | Champion, technical evaluator | Workflow, alert fatigue, data retention |
| **IT ops / SRE** | Uptime, faster troubleshooting | Champion (observability) | Root-cause time, tool sprawl |
| **Program manager (PM)** | Schedule, budget, delivery risk | Controls funding and timeline | Delivery risk, cost, timeline |
| **Contracting officer (CO)** | Following acquisition rules | Paperwork gatekeeper | Vehicles, compliance, pricing |
| **SI program lead** | Winning and delivering the program | Partner; often holds the contract | Win themes, delivery, margins |

> 🎯 **Rule of thumb:** go **high** (mission owner, CIO, CISO) for the *why* and the money, go **wide** (SOC, ops, SI) for champions and technical proof.

## The discovery flow: **Current → Pain → Impact → Future → Process**

| Stage | Purpose | Example question |
|---|---|---|
| **1. Current state** | Understand today | "Walk me through how your SOC handles an alert today." |
| **2. Pain** | Find the problem | "Where does that process break down?" |
| **3. Impact** | Make it matter | "What happens to the mission when an investigation takes three days?" |
| **4. Future state** | Define success | "If this were solved a year from now, what would be different?" |
| **5. Process** | Find the path to buy | "How would a decision like this get funded and approved?" |

## Top discovery questions

### Mission (ask everyone)
- What mission outcomes are you accountable for this year?
- What's driving the need to change **now**? What happens if nothing changes?
- How does this tie to Zero Trust, CJADC2 or your modernization goals?

### Security
- How many endpoints, and how much log data per day?
- What SIEM and endpoint tools do you use today? **When do those contracts renew?**
- How long are you able to keep data searchable? What do you drop because of cost?
- How long does it take to detect and investigate an incident?
- Do you operate on SIPR, JWICS, air-gapped or DIL networks?

### Observability
- How do you find out about outages: alerts, or users complaining?
- How long does root-cause analysis take? How many monitoring tools are in place?
- Are you moving to cloud (JWCC), containers or OpenTelemetry?

### Search & AI
- What information can't your people find today, and what does that cost?
- What AI pilots are underway? What's keeping them out of production?
- Which AI models are approved for your environment?

### Process & money (Lesson 07)
- Where would funding come from, and when can it be obligated?
- Which vehicles and integrators do you usually buy through?
- Who else needs to be involved? Is there an existing ATO path?

## 🔑 Key Terms

| Term | Meaning |
|---|---|
| **Discovery** | Questions to understand the problem before pitching |
| **Economic buyer** | The person who controls the budget and can say yes |
| **Champion** | An insider who sells for you |
| **Technical evaluator** | Tests and validates the solution |
| **TCO** | Total cost of ownership |
| **Compelling event** | A deadline that forces action (renewal, mandate, FY end) |

## 🗣️ Say It — role-play
Pretend you're on a call with a **DISA SOC manager**. Ask 5 questions out loud, one from each stage: Current → Pain → Impact → Future → Process. Then do it again for a **CIO**: same flow, more mission and cost focus.

## ⚠️ Watch Out
- **Talk less than 30% of the time** in discovery.
- **Ask "why" 2–3 times** to get from symptom to real pain.
- **Don't demo too early.** A demo before discovery is a guess.
- Always ask **"when does your current contract renew?"** Renewals are your best compelling events.

## 🧠 Recall
1. What's the doctor vs. pharmacist analogy?
2. What are the 5 stages of the discovery flow?
3. Who's usually the economic buyer in a security deal?
4. Which question uncovers a compelling event?

<details><summary>Answers</summary>

1. Diagnose (ask questions, find the real problem) before prescribing (pitching).
2. Current state → Pain → Impact → Future state → Process.
3. The CISO or CIO (or a program manager with funded authority).
4. "When does your current contract renew?" (or "What's driving the need to change now?")
</details>

## 📌 Remember
- **Doctor, not pharmacist**
- **Current → Pain → Impact → Future → Process**
- Go **high** for money, **wide** for champions

---

[← Prev](07-how-dod-buys.md) · [Index](00-index.md) · [Next: MEDDPICC →](09-qualifying-meddpicc.md)
