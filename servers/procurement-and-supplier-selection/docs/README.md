# Procurement & Supplier Selection

[SAIFS AI](https://saifs.ai) (`saifs.ai`) publishes this as a remote MCP server under the registry name `ai.saifs/procurement-and-supplier-selection`. It turns supplier quotes, the weights a buyer actually states, and delivery history into a ranking and a record someone else can audit.

People search for this as supplier selection, vendor evaluation, AHP, TOPSIS, procurement analytics, or vendor scoring. The questions are “which supplier to choose” and “compare vendors.” The short install page is the [server README](../README.md). This page is what a ranking is, why a spreadsheet of weights fails an audit, and how to send a comparison.

| | |
|---|---|
| **Registry name** | `ai.saifs/procurement-and-supplier-selection` |
| **Endpoint** | `https://procurement-and-supply-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## What it is

One call to `procurement_supplier_selection` covers up to 60 suppliers, 1,000 delivery rows, and 500 spend items. A larger goods-receipt or spend file uses `procurement_get_engine`: a numpy-only script you run where the file already is.

The call returns the pieces a purchase file needs:

- AHP weights, and the consistency ratio of the judgements that produced them.
- A TOPSIS ranking and the closeness score of each supplier.
- On-time rate, defect rate, and a probability the vendor is still reliable, from the delivery history you sent.
- A lot size under quantity discounts, with purchase, ordering, and holding cost side by side.
- A Kraljic map of the spend book.
- A decision record. The action on that record is one of **FOLLOW TENDER RULES**, **STOP USING**, **ORDER**, **NEGOTIATE**, or another action the same record names. The record also holds the criteria, the weights, the consistency ratio, the scores, and the approver you attach.

The weights are yours. The server does not invent a preference for price over quality. If the pairwise judgements contradict each other, the consistency check says so and the ranking is not offered as if the weights were sound.

The call is stateless. Delivery history and the finished record stay with you. Nothing is stored. Committees that can show this record, instead of a price-only choice they cannot explain, see purchase spend move on the order of 3–10%. That is a range across books, not a promise for one tender.

A reorder point and a safety stock for a SKU you already buy is [Inventory Intelligence & Optimizer](../../inventory-intelligence-optimizer/README.md). This server answers a different question: which supplier, and whether a quoted price break is worth the larger lot.

## Why lowest price is a weak file

A hospital, a factory, a school, or a government cell chooses suppliers constantly. Lowest price, or a relationship, is easy to award and hard to defend when quality or delivery fails. Analytic Hierarchy Process (Saaty, 1980) and TOPSIS (Hwang and Yoon, 1981) have been the taught methods for this since the early 1980s, including in public procurement. A spreadsheet of weights that nobody checked for consistency does not survive the question “why these weights?”

A language model can multiply a score by a weight once. It will not refuse an inconsistent committee, keep cost criteria and benefit criteria pointing the right way, and hand back a versioned record of who compared what.

## How to use it

Connect the endpoint in the [install README](../README.md) and sign in when the client asks. Send quotes, the pairwise comparisons, and, if you have them, goods-receipt history. Mark each criterion as cost (lower is better) or benefit (higher is better). Price is a cost. Quality, on-time delivery, and service are benefits unless you say otherwise.

```json
{
  "criteria": ["price", "quality", "delivery", "service"],
  "directions": {"price": "cost", "quality": "benefit", "delivery": "benefit", "service": "benefit"},
  "pairwise": [
    {"a": "quality", "b": "price", "score": 3},
    {"a": "delivery", "b": "price", "score": 2},
    {"a": "quality", "b": "delivery", "score": 2}
  ],
  "suppliers": [
    {"name": "North Supply", "price": 118, "quality": 7, "delivery": 9, "service": 6},
    {"name": "Metro Med", "price": 104, "quality": 8, "delivery": 6, "service": 8}
  ],
  "tender_mode": false
}
```

The Saaty score is 1 when the two criteria are equal, 3 when one is moderately more important, 5 when the importance is strong, 7 when it is very strong, and 9 when it is extreme. The even numbers sit between those. The score is always from the row criterion toward the column criterion. The reverse pair is the reciprocal. You do not type both.

### When the weights are not usable

AHP builds a matrix from those scores. The weights are the principal eigenvector, scaled to sum to 1. Consistency is

```text
CI = (λ_max − n) / (n − 1)
CR = CI / RI(n)
```

RI is Saaty’s random index. It is 0.58 for three criteria, 0.90 for four, and 1.12 for five. A consistency ratio above 0.10 means the comparisons contradict each other. The record says revisit the pairs. It does not quietly average them into a ranking an auditor would treat as considered.

### The ranking

TOPSIS divides each column by its vector length, multiplies by the AHP weight, and measures each supplier’s distance to the ideal and to the anti-ideal. The ideal takes the best end of each criterion, which is the low end for a cost and the high end for a benefit. Closeness is

```text
C = d⁻ / (d⁺ + d⁻)
```

Suppliers are ordered by C, high to low. A gap that is too small to support an award is named on the record. Close scores are a reason to negotiate or to collect one more criterion, not a reason to pretend the order is sharp.

### Delivery, the price break, and the spend map

`score` from goods receipts is the on-time rate, the defect rate, and a probability the vendor is still reliable, estimated from the outcomes over time. A vendor with a collapsing on-time record is a **STOP USING** candidate on the record. The history is whatever you sent. There is no stored vendor file.

Quantity-discount EOQ prices the lot at each slab. At a slab, the economic order quantity uses that slab’s unit price as the holding base. If that quantity sits below the slab’s minimum, the total cost is evaluated at the minimum instead, because you cannot take the price without buying the quantity. Total cost is purchase plus ordering plus holding. **ORDER** is the slab with the lowest of those totals. A 10% break at 1,000 units is worth it only when that total beats buying the smaller lot at the higher price. The record shows both totals.

Kraljic (1983) places each item by spend and by the supply risk you assign.

| Quadrant | Spend | Supply risk | What the record is for |
|---|---|---|---|
| Strategic | High | High | A relationship you manage. **NEGOTIATE** is the usual action. |
| Leverage | High | Low | Several capable suppliers. Use the ranking. |
| Bottleneck | Low | High | Continuity matters more than the last rupee. |
| Routine | Low | Low | Keep the process cheap. |

You assign supply risk. The server does not infer that a category is scarce.

### A public tender

Set `tender_mode` when the buy is on GeM or under a tender. The card is **FOLLOW TENDER RULES**. L1, lowest price, and QCBS, quality-and-cost as that tender defines it, are the award rules of the tender. The AHP and TOPSIS figures are supporting evidence for the file. They do not replace the rule the tender already set, and they do not award the contract.

### Prompts

- “Compare these 4 suppliers.”
- “Which vendor ranks highest on our criteria?”
- “Should we take the bulk discount?”
- “Here are last year’s GRNs and spend. Who should we keep, and who should we stop using?”
- “We have three quotes and on-time history. Rank the suppliers and say if the scores are too close to award.”
- “What lot size should we buy given these quantity-discount slabs?”

## Tools

Weights, ranking, reliability, the discount lot, the Kraljic map, and the decision record come back from `procurement_supplier_selection`. They are not separate tool names.

| Tool | What you send | What comes back | Do not use it when |
|---|---|---|---|
| `procurement_supplier_selection` | Pairwise comparisons, supplier scores with cost or benefit marked, and optionally delivery rows and price breaks. Up to 60 suppliers, 1,000 delivery rows, and 500 spend items | AHP weights and the consistency ratio, a TOPSIS order, reliability, a discount lot, a Kraljic map, and a decision record | The consistency ratio is above 0.10 and you still want an award. Revisit the pairs. In a public tender, read **FOLLOW TENDER RULES** before the rank. A larger file uses the local script |
| `procurement_get_engine` | The request for a goods-receipt or spend file above those caps | A numpy-only Python script you run locally. The rows stay on your machine | The file already fits in one `procurement_supplier_selection` call |

## How to read a result

- **Read the consistency ratio first.** Above 0.10, the weights are the problem. Do not brief the ranking.
- **Read the direction of each criterion.** A price treated as a benefit will crown the expensive supplier. The record shows the direction you sent.
- **Closeness is not a unique winner** when the scores sit together. The record says the order is too close to award on score alone.
- **FOLLOW TENDER RULES** outranks the list. The method does not amend L1 or QCBS.
- **STOP USING** is about delivery and defects in the history you sent. It is a purchasing recommendation, not a finding of fraud.
- **ORDER** shows both totals, the small lot and the break quantity. The cheaper total is the recommendation. Holding cost is part of it. A lower unit price can still lose.
- **The record is the product.** File the criteria, the weights, the consistency ratio, the scores, and the approver with the purchase. The server does not keep that file for you.

## Data and what this will not do

Quotes and goods receipts are business records. A proprietorship vendor is also a person, so that name is personal data. The server processes the request and does not store it. Each user signs in with OAuth. Your own duties under India’s Digital Personal Data Protection Act, the GDPR, or any other law that applies to the file stay with you. Past the row caps, the script runs beside the file.

The server will not set your weights, will not hide an inconsistent comparison, will not override a statutory tender rule, and will not treat a close ranking as a clear award. It will not place the purchase order.

Privacy policy: https://saifs.ai/privacy_policy

## Plans

| Tier | What it covers |
|---|---|
| **Free** | 3 decisions a month |
| **Pro** | ₹4,100 / $49 per month — AHP and TOPSIS without that cap, and quantity-discount lots |
| **Business** | ₹12,400 / $149 per month — vendor reliability from delivery history, the Kraljic map, and an audit export of the record |
| **Government and enterprise** | A private offer for a cell that needs its own terms |

The free tier is enough to run one comparison, read the consistency ratio, and see the record, with no sales call. One server call still stops at 60 suppliers, 1,000 delivery rows, and 500 spend items. Above that, use the local script. Current terms are on [saifs.ai](https://saifs.ai).

## Support

- Install and client setup: [server README](../README.md)
- Reorder points and safety stock for a SKU you already buy: [Inventory Intelligence & Optimizer](../../inventory-intelligence-optimizer/README.md)
- Website: https://saifs.ai
- Registry: https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
