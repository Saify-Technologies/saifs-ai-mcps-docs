# 18 · Demand Forecaster — Holt-Winters & Croston's Method MCP
**Score: 81/100 · Phase: BUILD EARLY (feeds Inventory doc 16 + Staffing doc 17)**
**Classical model: Exponential Smoothing (Brown 1956) · Holt-Winters (1957/60) · Croston's (1972, intermittent demand)**

## Problem statement
"What will we sell next month, per SKU / per region?" is the input to inventory, staffing, cash, and hiring — get it wrong and everything downstream breaks. Businesses use last-year-plus-10% or gut feel. Proper time-series forecasting (Holt-Winters for trend+seasonality; Croston's for lumpy spare-parts/B2B demand) has 60+ years of published accuracy benchmarks but lives in analytics platforms and data-science teams that mid-market/SMB don't have. An LLM fits a forecast once from a prompt but can't re-fit nightly, track forecast-vs-actual accuracy, or refuse when history is too short.

## Solution
Continuous, accuracy-tracked forecasting over the customer's history, feeding their other decisions.

**Tool set (v1):**
- `forecast_demand(sales_history, horizon)` → point forecast + prediction interval per period
- `detect_seasonality(series)` → seasonal pattern + strength report
- `forecast_intermittent(sparse_history)` → Croston's method for lumpy demand
- `forecast_accuracy_report(item)` → tracked MAPE/MASE vs. actuals, per model version (witness discipline)
- `watch_forecast(source)` → nightly re-fit; alert on demand regime change
- `explain_forecast(item)` → decomposition (level/trend/season) + model + confidence

## THE ALGORITHMS (implementation guide)
### Holt-Winters triple exponential smoothing (1957/60)
Level `ℓ_t = α(y_t/s_{t−m}) + (1−α)(ℓ_{t−1}+b_{t−1})`; trend `b_t = β(ℓ_t−ℓ_{t−1}) + (1−β)b_{t−1}`; seasonal `s_t = γ(y_t/ℓ_t) + (1−γ)s_{t−m}` (multiplicative variant shown). Forecast `ŷ_{t+h} = (ℓ_t + h·b_t)·s_{t−m+h}`. Optimize α,β,γ by minimizing in-sample error.
### Croston's method (1972) — intermittent/lumpy demand
Separately smooth demand *size* and *inter-arrival interval*; forecast = size/interval. Critical for spare parts, B2B, slow movers where Holt-Winters fails.
### Model selection & validation
Auto-pick among simple/Holt/Holt-Winters/Croston by backtested error (time-series cross-validation, never random split). **Track MAPE/MASE per model version** — the accuracy history is moat 4 and the trust signal. Reference: OSS `statsmodels` (ETS), `sktime`; we harden + version + track.

## Pressure test
G1 (27/30): interpreter fits once ⚠️; can't re-fit nightly, track accuracy, or refuse on thin data. Native ✅ safe. G2 (20/25): moats 4 (accuracy history + demand corpus) + 3 (nightly re-fit + watchers) + 5 (explained, backtested, versioned). G3 (21/25): CSV-first, universal, feeds 16/17 (bundle logic). G4 (14/20): compounds. **Total 81.**

## Audience & user profiles
Anyone who plans on future demand: retailers, distributors, manufacturers, D2C, support (volume forecasting), workforce planners.
1. **Retail buyer** — per-SKU next-quarter forecast with seasonality, feeding order quantities (doc 16).
2. **Support ops** — contact-volume forecast feeding Erlang staffing (doc 17).
3. **Spare-parts distributor** — Croston's forecasts for lumpy industrial demand nobody else models well.

## Where/how to get data & components
| Need | Source | Access |
|---|---|---|
| Sales/volume history | CSV/XLSX (v1), ERP/POS/helpdesk exports, Dataverse | none for CSV; connectors later |
| Math | `statsmodels`, `sktime` (OSS) | pip; we harden + track accuracy |
| Jobs/watchers | TECH-00 §4 | internal |

## Monetization
- **Free**: 25 series, monthly forecast
- **Pro ₹4,100/$49 mo**: 500 series, nightly re-fit, accuracy tracking, intervals
- **Business ₹12,400/$149 mo**: 5,000 series, Croston's, regime-change alerts, data pulls
- **Suite bundle** with 16+17 (Decision-Math): discounted combined plan (see doc 23)
- Positioning premium: forecasts priced above raw lookups (foresight).

## India sole-prop notes
Aggregate sales data — light DPDP. Bundles naturally with Inventory (16) for a "stock + demand" Indian-SMB suite via Tally exports. Export invoicing under LUT.

## Risks
Thin/short history (refuse-with-reason, publish minimum data bars); over-claiming accuracy (always ship prediction intervals + tracked MAPE, never a bare number); regime shifts (change-detection + honest confidence widening).

---

---

## LISTING & DISCOVERABILITY
*Keep this section current. It drives marketplace search, AI tool selection and the buying decision.*

**Who discovers this MCP:** agent developers, developers and agent builders (Copilot Studio makers, Agentforce, Breeze and Gemini Enterprise builders, Claude/ChatGPT connector users). They find it, test it, and **champion the purchase** to the budget owner. Every field below is written for them first, with a business tagline the manager can understand.

### 1. Names
| Field | Use this | Rule |
|---|---|---|
| **Listing title** | **Demand Forecasting — Saifs AI** | Capability keyword first; ≤ 60 characters before "— Saifs AI" |
| **Tagline** (for the budget owner) | Forecast next week, month or season's sales with ranges | Plain business words; ≤ 90 characters |
| **Server / connector ID** | `saifs-demand-forecasting` | Lowercase, hyphenated, no coined words |
| **Internal code name** | Demand Forecaster (doc 18) | Docs and code only, **never** in a listing |

### 2. Description: the first 160 characters decide the click
1. **Developer contract:** Sales history (date, qty, SKU) → forecast with P10–P90 per SKU and period.
2. **Business value (for the champion):** Forecast error cut 10–30% vs naive methods.
3. **Methods (specialist search and credibility):** uses Holt-Winters, Croston, seasonality.
4. **Trust:** stateless; data is processed in the request and never stored; per-user OAuth sign-in. Always ranges; refuses with too little history.

### 3. Keywords
- **Developer search terms (title, description, tags):** demand forecasting, sales forecasting, time series forecasting, holt winters, intermittent demand, croston, forecast api
- **Business phrases (tagline, FAQ, tool descriptions):** "how much will we sell", "next month's demand"
- **Method terms (description body):** Holt-Winters, Croston, seasonality
- **Platform terms (only where actually listed):** MCP server, Copilot Studio connector, Agentforce action, Breeze agent tool, Gemini Enterprise, Amazon Bedrock AgentCore, Claude connector, ChatGPT app

### 4. Tools as shown in the listing (alphabetical)
`demand_forecaster` · `demand_forecaster_get_engine`

**Tool-description rule:** the AI picks tools from these descriptions, so they count as discoverability too.
- Start with a verb and the business noun.
- Name the input fields.
- Say what comes back.
- Say when **not** to use the tool.

### 5. Example prompts (listing, demo video, tool descriptions)
- "Forecast next month's sales per product"
- "How much will we sell during Diwali?"
- "Forecast spare-parts demand"

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
- Sample JSON payload shaped like a real export from: Tally, ERP, POS, CSV
- One copy-paste example call **and** its response
- A 60–90 second demo video: prompt → tool call → answer
- Link to the docs and the MCP/OpenAPI schema; privacy policy and terms

### 8. Champion kit (for the developer to forward upward)
- **One-line business case:** Forecast error cut 10–30% vs naive methods. Always as a range, never a bare number.
- **Security note:** stateless, nothing stored, per-user OAuth, DPDP/GDPR-friendly
- **Price:** the tier table from §Monetization, with the free tier stated first

### 9. Don'ts
- Coined names (e.g. "Demand Forecaster") in titles or tags
- "Best", "#1" or "guaranteed" claims
- Bare money figures
- Promises beyond the guardrail: Always ranges; refuses with too little history
