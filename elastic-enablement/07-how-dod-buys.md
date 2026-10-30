# 07 — How DoD Buys

[← Prev](06-elastic-for-defense.md) · [Index](00-index.md) · [Next: Buyers & Discovery →](08-buyers-and-discovery.md)

---

## 🎯 Goal
For any deal, be able to answer: **Who has the money? How will it be bought? Through whom? When?**

## 💡 The Idea
In commercial sales, a happy buyer can usually sign. In DoD, a happy buyer **still needs funded budget, an approved contract path, a partner, and authorization to run the software.**

> **Analogy:** Commercial buying is **walking into a store and paying**. DoD buying is **planning a wedding**: the budget is set a year or two ahead, you book through approved vendors, paperwork has deadlines, and nothing happens without sign-off from several people.

## The 4 questions for every deal

```
  💰 MONEY            📄 VEHICLE            🤝 PARTNER             📅 TIMING
  Which program       Which contract        Which SI / reseller    When can funds be
  office funds it?    will it go through?   holds the contract?    obligated? Any CR?
```

### 1. 💰 Money — the budget cycle
| Concept | What to know |
|---|---|
| **Fiscal year (FY)** | **Oct 1 – Sep 30.** FY2027 started Oct 1, 2026 |
| **Q4 = year-end spend** | **Jul – Sep.** Agencies rush to obligate expiring funds, so many deals close in August and September |
| **PPBE / POM** | DoD plans budgets **2+ years ahead**. Big new programs must get into the budget early |
| **Continuing resolution (CR)** | No budget passed → agencies run at last year's levels and usually **can't start new programs** → deals slip |
| **Colors of money** | Funds have specific purposes and expiration dates (e.g., O&M for operations, RDT&E for research, procurement). Ask what *type* of money is available |

### 2. 📄 Vehicle — how it's bought
A **contract vehicle** is a pre-competed agreement agencies can buy through quickly.

| Vehicle | What it is |
|---|---|
| **GSA Multiple Award Schedule (MAS)** | Government-wide catalog of pre-approved products and prices |
| **NASA SEWP** | Popular government-wide IT products vehicle |
| **DoD ESI** (Enterprise Software Initiative) | DoD's enterprise software agreements |
| **Army CHESS** | Army's IT buying program |
| **Agency IDIQs / BPAs** | Agency-specific contracts (often held by SIs) |
| **OTA** (Other Transaction Authority) | Faster, flexible path for prototypes |

> 🎯 Learn **which vehicles Elastic and its partners already hold** in your accounts. That's a day-one question.

### 3. 🤝 Partner — who holds the paper
Most DoD deals flow through **partners**, not directly from Elastic:

| Partner type | Examples | Role |
|---|---|---|
| **System integrator (SI) / prime** | Booz Allen, Leidos, SAIC, GDIT, Peraton, CACI | Holds the program contract; designs and runs the system; can bring Elastic in |
| **Reseller / distributor** | e.g., Carahsoft and other federal resellers | Holds vehicles; processes the order |
| **Small business** | Set-aside contracts | Some contracts must go to small businesses |

> **Why SIs matter so much:** they're often **in the building**, they **write the architecture**, and they **hold the contract**. An SI that recommends Elastic is like a champion with a budget. (Endgame also has a separate AE role just for Federal SIs.)

### 4. 📅 Timing — the gates
```
Need identified → Budget → Evaluation/pilot → Security review & ATO → Contract action → Deployment
```
| Gate | What it is |
|---|---|
| **ATO** (Authority to Operate) | Formal approval to run software on a DoD network, under the **Risk Management Framework (RMF)** |
| **Impact Levels (IL2/4/5/6)** | DoD cloud security levels. Higher = more sensitive (IL6 = Secret) |
| **FedRAMP** (Moderate / High) | Government-wide cloud authorization. DoD builds on it for IL levels |
| **Contracting officer (CO)** | The only person who can legally sign the contract |

> 💡 **ATOs take months.** If the customer already has an ATO for Elastic (or a partner's system that includes it), the deal can move much faster. Always ask.

## 🔑 Key Terms

| Term | Meaning |
|---|---|
| **FY** | Federal fiscal year: Oct 1 – Sep 30 |
| **CR** | Continuing resolution: temporary funding, no new starts |
| **PPBE / POM** | DoD's multi-year budget process |
| **Obligate** | Legally commit funds to a contract |
| **Contract vehicle** | Pre-competed buying agreement |
| **IDIQ / BPA** | Common vehicle types |
| **Prime** | The contractor holding the main contract |
| **ATO / RMF** | Approval to operate / the framework for getting it |
| **IL** | DoD Impact Level |
| **CO** | Contracting officer |

## 🗣️ Say It
Walk an imaginary deal through the 4 questions out loud: *"DISA's SOC wants Elastic Security. The money is ___, the vehicle is ___, the partner is ___, and the timing is ___."*

## ⚠️ Watch Out
- **Enthusiasm ≠ funding.** A thrilled engineer with no budget isn't a deal yet.
- **Watch for CRs.** A CR can push a "Q1 close" to Q3.
- **Ethics rules are strict.** Gifts, meals and hiring discussions with government employees have legal limits. When in doubt, ask legal or compliance.

## 🧠 Recall
1. When does the federal fiscal year start and end?
2. Why do many deals close in August and September?
3. What is a CR, and how does it affect deals?
4. What's a contract vehicle? Name two.
5. What is an ATO, and why does it affect timing?

<details><summary>Answers</summary>

1. October 1 to September 30.
2. Year-end spend: agencies obligate expiring funds before Sep 30.
3. Temporary funding at prior-year levels; usually no new programs, so deals get delayed.
4. A pre-competed buying agreement; e.g., GSA MAS, NASA SEWP, DoD ESI, Army CHESS.
5. Authority to Operate: approval to run software on a DoD network; it can take months.
</details>

## 📌 Remember
- Every deal: **Money · Vehicle · Partner · Timing**
- **FY = Oct–Sep**, Q4 rush, CRs delay
- **SIs** are kingmakers; **ATO** is a gate

---

[← Prev](06-elastic-for-defense.md) · [Index](00-index.md) · [Next: Buyers & Discovery →](08-buyers-and-discovery.md)
