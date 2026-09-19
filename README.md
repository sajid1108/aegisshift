<div align="center">

<img src="tree-poster.jpg" alt="A lone world tree at blue hour on a misty ridge, its crown lit with small lights, a few of them red" width="100%" />

# AegisShift

**A prediction is not a decision.**<br/>
Return-abuse risk management that shifts its stance for every order.

<a href="https://sajid1108.github.io/aegisshift/"><img src="https://img.shields.io/badge/Live%20site-sajid1108.github.io%2Faegisshift-5266eb?style=for-the-badge" alt="Live site" /></a>

<img src="https://img.shields.io/badge/data-synthetic-70707d?style=flat-square" alt="Synthetic data" />
<img src="https://img.shields.io/badge/policy-v1.0-70707d?style=flat-square" alt="Policy v1.0" />
<img src="https://img.shields.io/badge/explanations-no%20SHAP%2C%20no%20LLM%20text-70707d?style=flat-square" alt="Deterministic explanations" />

</div>

---

## The idea

E-commerce merchants lose money to **return abuse**: wardrobing, empty-box returns, false "item not received" claims, and coordinated rings of accounts. Most tools fight it by punishing customers who simply **return a lot**. That blocks loyal shoppers and still misses the rings, because every ring account looks clean on its own.

AegisShift keeps two questions apart:

| | Return probability | Abuse probability |
|---|---|---|
| **What it asks** | Will this order come back? | Will this order be confirmed as abuse? |
| **What it drives** | Nothing. It's operational context only | An expected-cost decision, constrained by guardrails |

Models **predict**. A deterministic policy **decides**. People can **override**, and every decision is **recorded**.

## The stance change

Like a shield changing stance, AegisShift moves between four postures, only as far as the cost of **that** order justifies:

| Stance | What the customer experiences | When it wins |
|---|---|---|
| 🟢 **Allow** | Nothing, the order goes through | Low abuse probability |
| 🟡 **Prepaid only** | Pay online; the refund is released after warehouse inspection | Moderate risk on cheaper orders |
| 🟠 **Manual review** | The order is confirmed and a person checks it | The uncertain middle on valuable orders |
| 🔴 **Block** | The order is refused, with a support reference | High confidence **and** at least two independent signals agree |

Each stance is priced in rupees: `EC(a) = p·A(a) + (1 − p)·G(a) + O(a)`. The cheapest stance the guardrails still permit wins. **Guardrails can only remove options, never add them**, so a block needs confidence, corroboration, and evidence that isn't stale.

## Three orders, three answers

| | The order | AegisShift | A typical merchant rule |
|---|---|---|---|
| **Demo 1** | A 3½-year customer who returns 61 % of orders | **Allow**, abuse < 0.1 % | Block |
| **Demo 2** | A 5-day-old account buying a ₹24,000 phone on a device shared with confirmed abusers | **Block**, 95.1 % (9.9 % without the relationship evidence) | Manual review |
| **Demo 3** | A ₹12,000 order sitting at 69.1 % | **Manual review**: cheapest, and just below the blocking floor | Prepaid only |

## Results (synthetic backtest, 1,833 held-out orders)

| | |
|---|---|
| **0.814** | abuse PR-AUC with relationship-graph features (0.527 without) |
| **72.7 %** | recall on a fraud ring never seen in training (14.3 % without the graph) |
| **87.5 %** | of blocks were real abuse |
| **0.12 %** | of genuine customers blocked, against 2.42 % under rule-based decisions |

**What it costs, stated plainly:** a tuned fixed threshold has a lower realized cost (₹1,60,112 against ₹1,95,313 per 1,000 orders). AegisShift pays that premium so that it wrongly blocks 3.3× fewer genuine customers. It's a deliberate trade, not a win on cost.

## What we disclose

- **All data is synthetic:** a seeded world with deliberately hard cases (households, hostels, second-hand phones). It validates the architecture and the policy's behaviour, not real-world effectiveness.
- **New accounts carry more friction:** 36.1 % against about 5 % for older accounts. Newness alone can never trigger a block.
- **Privacy by design:** identifiers arrive already hashed; no names, IP addresses or pincodes are used; the graph shows masked labels only.

## This repository

This repo holds the **landing page**, a static site with no build step. The scoring engine, models and reviewer console live in a private repository.

| Page | What you'll find |
|---|---|
| **Home** | The world tree. The fraud ring from Demo 2 is overlaid on its branches |
| **Stance change** | Drag the abuse probability and watch the stance and the tree change |
| **Demos** | The three decisions, replayed from recorded values |
| **Results** | The full synthetic backtest |
| **Disclosures** | The limits, stated as part of the result |

**Run it locally:** open `index.html` in any browser, or serve the folder:

```bash
python -m http.server 8000
```

<div align="center">
<sub>Synthetic data is used to validate the architecture, policy behaviour, auditability, and coordinated-pattern detection. Real deployment would require merchant-specific historical data and prospective validation.</sub><br/>
<sub>Tree footage is AI-generated. Built by <a href="https://github.com/sajid1108">Sayed Sajid Ali</a>.</sub>
</div>
