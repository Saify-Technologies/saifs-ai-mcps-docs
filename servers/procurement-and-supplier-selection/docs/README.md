# 44 · Procurement & Supplier Selection — AHP/TOPSIS Vendor Decision MCP
**Initial moat: 78/100 (potential 81) · IW-Breadth: 75 · Phase: STRONG NEW (every organisation buys things)**
**Classical models: Analytic Hierarchy Process (Saaty 1980) · TOPSIS (Hwang-Yoon 1981) · Quantity-discount EOQ (Hadley-Whitin 1963) · Vendor survival / reliability (doc 14 Component C) · Kraljic portfolio matrix (1983)**

## 1. Problem statement
Every business, hospital, school and government office chooses suppliers — and almost none do it with a method. Decisions are made on lowest price or relationships, then must be *justified* to a manager, finance head, auditor or purchase committee. When a cheap supplier fails on quality or delivery, nobody can show why it was chosen. Multi-criteria decision methods (AHP, TOPSIS) have been the academic and public-procurement standard since the early 1980s; mid-market buyers use spreadsheets with arbitrary weights that can't survive an audit question.

## 2. Solution
Explained, auditable supplier ranking and buying-quantity decisions.

**Tool set (v1):**
- `set_criteria_weights(pairwise_comparisons)` → AHP weights + consistency ratio (flags inconsistent judgements)
- `rank_suppliers(suppliers[], criteria_scores, weights)` → TOPSIS closeness scores and ranking
- `score_vendor_reliability(delivery_history)` → on-time rate, defect rate, P(vendor still reliable) — doc 14 survival engine pointed at suppliers
- `quantity_discount_order(demand, price_breaks, order_cost, holding_rate)` → whether bulk breaks are worth it
- `classify_spend(items, spend, supply_risk)` → Kraljic quadrant (strategic / leverage / bottleneck / routine)
- `decision_record(decision_id)` → audit-ready record: criteria, weights, scores, consistency, who approved

## 3. THE ALGORITHMS
### AHP (Saaty 1980)
Pairwise comparison matrix `A` (Saaty 1–9 scale). Weights = principal eigenvector of A, normalised. Consistency: `CI = (λ_max − n)/(n − 1)`, `CR = CI / RI(n)` with random index RI (0.58 for n=3, 0.90 for n=4, 1.12 for n=5). **CR > 0.10 → judgements inconsistent; ask the user to revisit** — this check is the value most spreadsheet weightings lack.
### TOPSIS (1981)
Normalise decision matrix by column vector norm, apply weights, define ideal (best per criterion, respecting cost vs benefit) and anti-ideal; closeness `C_i = d⁻ / (d⁺ + d⁻)`; rank by C descending.
### Quantity-discount EOQ
Compute EOQ at each price break; if EOQ falls below the break, evaluate at the break quantity; choose minimum total cost (purchase + ordering + holding).
### Vendor reliability
Kaplan-Meier / logistic on delivery outcomes over time — reuses doc 14 Component C.

## 4. Pressure test (initial)
- **G1 Substitution (25/30):** an LLM can compute TOPSIS once; it cannot hold a vendor-performance history, enforce consistency checks across a committee, or produce a versioned audit record. Native threat: procurement suites (Coupa, SAP Ariba) — enterprise-priced.
- **G2 Moats (19/25):** moat 5 (audit-ready, consistency-checked decisions — the core sale), moat 4 (vendor performance history), moat 3 (re-scoring as deliveries land).
- **G3 Business (20/25):** universal buyer across private, healthcare, education and government; "justify the decision" is a real, recurring need.
- **G4 Time (14/20):** public-procurement transparency norms favour documented methods. **Total 78; potential 81.**

## 5. Audience & user profiles
Purchase managers in manufacturing, hospitals and pharmacy chains, school/college administrations, construction firms, government procurement cells (later).
1. **Hospital purchase officer** — ranks four suppliers of surgical consumables on price, quality, delivery and service; record ready for the committee.
2. **Factory buyer** — "is the 10% bulk discount at 1,000 units worth it?" → yes/no with total-cost comparison.
3. **School administrator** — transparent vendor choice for uniforms or transport, defensible to the management board.

## 6. Data & components
Supplier quotes, criteria scores, delivery/quality history (caller-supplied CSV). Math: `numpy`, `scipy` (OSS). Stateless default; optional decision-record retention as a paid, consented feature.

## 7. Monetization
- Free: 3 decisions/month
- Pro ₹4,100/$49 mo: unlimited AHP/TOPSIS, quantity-discount analysis
- Business ₹12,400/$149 mo: vendor reliability tracking, Kraljic spend view, audit exports
- Gov/enterprise: private offers (post-incorporation)

## 8. India notes & risks
GeM (Government e-Marketplace) and public tenders use L1/QCBS rules — position as decision support, never as overriding statutory tender rules. Vendor data is business data (light DPDP; proprietorship vendors are individuals — consent applies). Risks: garbage weights (AHP consistency check is the mitigation), legal misuse in tenders (disclaim), over-complex UX (default criteria templates per category).

---

---

## LISTING & DISCOVERABILITY
*Keep this section current. It drives marketplace search, AI tool selection and the buying decision.*

**Who discovers this MCP:** agent developers, developers and agent builders (Copilot Studio makers, Agentforce, Breeze and Gemini Enterprise builders, Claude/ChatGPT connector users). They find it, test it, and **champion the purchase** to the budget owner. Every field below is written for them first, with a business tagline the manager can understand.

### 1. Names
| Field | Use this | Rule |
|---|---|---|
| **Listing title** | **Supplier Scoring & Selection (AHP / TOPSIS) — Saifs AI** | Capability keyword first; ≤ 60 characters before "— Saifs AI" |
| **Tagline** (for the budget owner) | Compare suppliers on price, quality and delivery, and pick the best | Plain business words; ≤ 90 characters |
| **Server / connector ID** | `saifs-supplier-selection` | Lowercase, hyphenated, no coined words |
| **Internal code name** | Procurement & Supplier Selection (doc 44) | Docs and code only, **never** in a listing |

### 2. Description: the first 160 characters decide the click
1. **Developer contract:** Supplier criteria + weights (JSON) → ranked suppliers, consistency check, order split.
2. **Business value (for the champion):** Purchase spend cut 3–10%.
3. **Methods (specialist search and credibility):** uses AHP, TOPSIS, quantity-discount EOQ.
4. **Trust:** stateless; data is processed in the request and never stored; per-user OAuth sign-in. Weights set by the buyer; transparent scoring.

### 3. Keywords
- **Developer search terms (title, description, tags):** supplier selection, vendor evaluation, ahp, topsis, procurement analytics, vendor scoring
- **Business phrases (tagline, FAQ, tool descriptions):** "which supplier to choose", "compare vendors"
- **Method terms (description body):** AHP, TOPSIS, quantity-discount EOQ
- **Platform terms (only where actually listed):** MCP server, Copilot Studio connector, Agentforce action, Breeze agent tool, Gemini Enterprise, Amazon Bedrock AgentCore, Claude connector, ChatGPT app

### 4. Tools as shown in the listing (alphabetical)
`classify_spend` · `decision_record` · `quantity_discount_order` · `rank_suppliers` · `score_vendor_reliability` · `set_criteria_weights`

**Tool-description rule:** the AI picks tools from these descriptions, so they count as discoverability too.
- Start with a verb and the business noun.
- Name the input fields.
- Say what comes back.
- Say when **not** to use the tool.

### 5. Example prompts (listing, demo video, tool descriptions)
- "Compare these 4 suppliers"
- "Which vendor is best overall?"
- "Should we take the bulk discount?"

### 6. Categories per marketplace
| Marketplace | Category / tags |
|---|---|
| Microsoft (Copilot Studio certified connector / AppSource) | Operations & Supply Chain; Analytics |
| Salesforce AgentExchange | Operations / Service |
| AWS Marketplace: AI Agents & Tools | Operations → Supply chain & logistics |
| Google Cloud Marketplace / Gemini Agent Gallery | Supply chain / Retail |
| HubSpot Marketplace (Breeze) | Operations |

### 7. Developer try-it kit (ship with the listing)
- Free tier that works in under 10 minutes, no sales call
- Sample JSON payload shaped like a real export from: ERP purchase data, quotes
- One copy-paste example call **and** its response
- A 60–90 second demo video: prompt → tool call → answer
- Link to the docs and the MCP/OpenAPI schema; privacy policy and terms

### 8. Champion kit (for the developer to forward upward)
- **One-line business case:** Purchase spend cut 3–10%. Always as a range, never a bare number.
- **Security note:** stateless, nothing stored, per-user OAuth, DPDP/GDPR-friendly
- **Price:** the tier table from §Monetization, with the free tier stated first

### 9. Don'ts
- Coined names (e.g. "Procurement & Supplier Selection") in titles or tags
- "Best", "#1" or "guaranteed" claims
- Bare money figures
- Promises beyond the guardrail: Weights set by the buyer; transparent scoring
