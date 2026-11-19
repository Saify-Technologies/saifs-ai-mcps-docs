# 16 · Inventory Optimizer — EOQ, Newsvendor & Safety-Stock MCP
**Score: 83/100 · Phase: BUILD EARLY (Decision-Math family flagship #2 after Tiering)**
**Classical model: EOQ (Harris, 1913) · Newsvendor (Edgeworth, 1888) · Safety-stock service-level models**

## Problem statement
Every business holding stock — distributors, retailers, manufacturers, pharmacies, e-commerce — decides *how much to order, when, and how much buffer to hold*. Most do it by gut or a static reorder level that's wrong half the year. Overstock ties up cash and spoils; understock loses sales. The math to solve this optimally has existed since 1913, but it's locked in expensive ERP modules (SAP APO, Oracle) that mid-market and SMB never deploy — they run spreadsheets and instinct instead. An LLM can compute EOQ once from a prompt; it cannot recompute nightly as demand shifts, watch reorder points, or hold versioned assumptions.

## Solution
Continuous, explained inventory math over the customer's stock + sales data (CSV-first, then ERP/Tally/QuickBooks pulls).

**Tool set (v1):**
- `optimize_order_quantity(item, demand, order_cost, holding_cost)` → EOQ + total-cost curve
- `set_safety_stock(item, demand_variance, lead_time, service_level)` → buffer + reorder point
- `optimize_perishable_order(item, demand_dist, unit_cost, sale_price, salvage)` → newsvendor critical-fractile quantity
- `flag_overstock_understock(inventory_csv)` → items outside optimal bands, ranked by cash impact
- `watch_reorder_points(inventory_source)` → nightly recompute; alert when stock crosses reorder point
- `explain_recommendation(item)` → the math trace + model version

## THE ALGORITHMS (implementation guide)
### EOQ (Harris 1913)
`EOQ = √(2·D·S / H)` where D=annual demand, S=order cost, H=holding cost/unit/year. Reorder point `ROP = d·L + SS` (d=daily demand, L=lead time, SS=safety stock).
### Newsvendor (Edgeworth 1888) — perishables/seasonal/one-shot
Critical fractile `CF = Cu/(Cu+Co)` (Cu=underage cost, Co=overage cost); optimal order = inverse-CDF of demand at CF. For normal demand: `Q* = μ + z·σ` where z = Φ⁻¹(CF).
### Safety stock / service level
`SS = z(service_level) · σ_demand · √(lead_time)`; supports demand *and* lead-time variance (the full formula). Service level → z via inverse normal.
**Numerical/data notes:** requires clean demand history (dedup + gap-fill = a `data_quality_report` pre-step); holding-cost estimation is the fuzzy input (offer a guided default: ~20-30% of unit cost/yr, editable). Reference: standard OR textbooks; OSS `scipy.stats` for the inverse-CDF work.

## Pressure test
G1 (27/30): interpreter can do one formula ⚠️ but not nightly recompute + watchers + versioned assumptions (continuity is the product). Native ✅ safe (ERP modules are the un-deployed competition; you fight the spreadsheet). G2 (20/25): moats 3 (continuity/watchers) + 4 (accumulated demand history feeds forecasting doc 18) + 5 (explained, versioned). G3 (22/25): CSV-first self-serve, universal need, usage/subscription pricing. G4 (14/20): compounds via demand history. **Total 83.**

## Audience & user profiles
Distributors, wholesalers, retailers, D2C/e-commerce, manufacturers, pharmacies — inventory/purchasing managers and SMB owners.
1. **Distributor purchasing manager** — nightly agent alert: "Reorder SKU-4471 today (below ROP); you're overstocked on 12 slow movers tying up ₹8L."
2. **E-commerce ops** — festival-season buy quantities via newsvendor instead of guessing.
3. **Pharmacy owner** — perishable/expiry-aware order quantities.

## Where/how to get data & components
| Need | Source | Access |
|---|---|---|
| Stock + sales history | CSV/XLSX upload (v1), Tally/QuickBooks/ERP exports, Dataverse | none for CSV; connectors later |
| Math | `scipy`, `numpy` (OSS) | pip; we harden + version |
| Jobs/watchers | TECH-00 §4 engine | internal |

## Monetization
- **Free**: 25 SKUs, weekly recompute
- **Pro ₹4,100/$49 mo**: 500 SKUs, nightly recompute, reorder watchers + alerts
- **Business ₹12,400/$149 mo**: 5,000 SKUs, perishable/newsvendor models, ERP pulls, cash-impact ranking
- **Enterprise**: multi-warehouse, custom service-level policies. Overage per 500 SKUs.

## India sole-prop notes
Inventory data isn't personal data (lighter DPDP surface). INR pricing via Razorpay; Tally-export onboarding is a massive Indian SMB wedge (most run Tally, none run APO). Export invoicing under LUT for foreign tenants.

## Risks
Holding-cost input quality (guided defaults + sensitivity display); demand-history gaps (refuse-with-reason below threshold); native ERP encroachment (position vs. the spreadsheet, never vs. SAP).

---

## MODE: Pharmacy & Hospital Store (added — a mode of this MCP, not a new product)
**Classical models: ABC-VED matrix (hospital-pharmacy standard since the 1970s) · FEFO (first-expiry-first-out) · perishable newsvendor**

**Why a separate mode:** pure ABC (value-based) ranking treats a cheap, life-critical drug as low priority. Pharmacies and hospital stores must combine **value** with **criticality**.

**ABC-VED matrix:** A/B/C by annual consumption value × V/E/D (Vital / Essential / Desirable, assigned by the pharmacy or therapeutics committee — the caller supplies VED, we never decide clinical criticality).
- Category I (AV, AE, AD, BV, CV): tight control, high service level — vital items must never stock out
- Category II (BE, CE, BD): moderate control
- Category III (CD): minimal control

**Tools added:**
- `abc_ved_classify(items, consumption_value, ved_class)` → matrix category and recommended service level
- `fefo_pick(item, batches[{batch, expiry, qty}], quantity)` → which batches to issue first
- `near_expiry_alert(stock, days_threshold)` → value at risk of expiry write-off, ranked
- Service levels by category feed the existing safety-stock formula (higher z for Category I).

**Guardrails:** operational stock control only — no prescribing, substitution or clinical advice. VED classes come from the customer's committee, never inferred by us.

---

---

## LISTING & DISCOVERABILITY
*Keep this section current. It drives marketplace search, AI tool selection and the buying decision.*

**Who discovers this MCP:** agent developers, developers and agent builders (Copilot Studio makers, Agentforce, Breeze and Gemini Enterprise builders, Claude/ChatGPT connector users). They find it, test it, and **champion the purchase** to the budget owner. Every field below is written for them first, with a business tagline the manager can understand.

### 1. Names
| Field | Use this | Rule |
|---|---|---|
| **Listing title** | **Inventory Optimization (Reorder Point & Safety Stock) — Saifs AI** | Capability keyword first; ≤ 60 characters before "— Saifs AI" |
| **Tagline** (for the budget owner) | How much to order, when to reorder, and what's overstocked | Plain business words; ≤ 90 characters |
| **Server / connector ID** | `saifs-inventory-optimization` | Lowercase, hyphenated, no coined words |
| **Internal code name** | Inventory Optimizer (doc 16) | Docs and code only, **never** in a listing |

### 2. Description: the first 160 characters decide the click
1. **Developer contract:** SKU stock + sales rows (JSON) → reorder points, order quantities, cash stuck, buying list.
2. **Business value (for the champion):** Inventory cash freed 10–30%; stockouts cut at the same service level.
3. **Methods (specialist search and credibility):** uses EOQ, newsvendor, ABC-VED, FEFO.
4. **Trust:** stateless; data is processed in the request and never stored; per-user OAuth sign-in. Ranges on every number; refuses below data thresholds.

### 3. Keywords
- **Developer search terms (title, description, tags):** inventory optimization, reorder point, safety stock, eoq, stock replenishment, overstock analysis, abc ved, pharmacy inventory
- **Business phrases (tagline, FAQ, tool descriptions):** "what should I order", "what is overstocked", "when to reorder"
- **Method terms (description body):** EOQ, newsvendor, ABC-VED, FEFO
- **Platform terms (only where actually listed):** MCP server, Copilot Studio connector, Agentforce action, Breeze agent tool, Gemini Enterprise, Amazon Bedrock AgentCore, Claude connector, ChatGPT app

### 4. Tools as shown in the listing (alphabetical)
`inventory_optimizer` · `inventory_optimizer_get_engine`

**Tool-description rule:** the AI picks tools from these descriptions, so they count as discoverability too.
- Start with a verb and the business noun.
- Name the input fields.
- Say what comes back.
- Say when **not** to use the tool.

### 5. Example prompts (listing, demo video, tool descriptions)
- "What should we reorder this week?"
- "How much cash is stuck in slow stock?"
- "Set safety stock for these SKUs"

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
- Sample JSON payload shaped like a real export from: Tally, ERP, Zoho Inventory, CSV
- One copy-paste example call **and** its response
- A 60–90 second demo video: prompt → tool call → answer
- Link to the docs and the MCP/OpenAPI schema; privacy policy and terms

### 8. Champion kit (for the developer to forward upward)
- **One-line business case:** Inventory cash freed 10–30%; stockouts cut at the same service level. Always as a range, never a bare number.
- **Security note:** stateless, nothing stored, per-user OAuth, DPDP/GDPR-friendly
- **Price:** the tier table from §Monetization, with the free tier stated first

### 9. Don'ts
- Coined names (e.g. "Inventory Optimizer") in titles or tags
- "Best", "#1" or "guaranteed" claims
- Bare money figures
- Promises beyond the guardrail: Ranges on every number; refuses below data thresholds
