# 24 · Marketing Intelligence — MMM, Market Basket & A/B Testing MCP
**Score: 79/100 · IW-Breadth: 68 · Phase: STRONG NEW DOMAIN (green-light)**
**Classical models: Market Basket / Association Rules (Agrawal, 1993) · Marketing Mix Modeling (regression, 1960s) · A/B significance testing (Fisher / Neyman-Pearson, 1920s–30s) · Bass Diffusion (1969)**

## Problem statement
Marketing teams spend budget on gut and vanity metrics. They can't answer the questions that actually matter: which channel *really* drives sales (not last-click), which products to bundle, whether an A/B result is real or noise, how a new product will ramp. The math for all four has existed for 30–100 years but lives in analytics platforms and agencies priced out of the mid-market — most marketers run spreadsheets and hope. An LLM computes any one of these once from a prompt; it cannot re-run MMM as spend/sales evolve, watch test significance as data accrues, or hold versioned attribution history.

## Solution
Explained, continuously-updated marketing math over the customer's spend, sales, and transaction data.

**Tool set (v1):**
- `attribute_channel_roi(spend_by_channel, sales_history)` → MMM: each channel's incremental contribution + ROI
- `optimize_budget_allocation(channels, constraints)` → recommended reallocation for max return
- `find_product_affinities(transaction_data)` → association rules (support/confidence/lift)
- `recommend_bundles(product)` → cross-sell bundles from affinity math
- `evaluate_ab_test(variant_a, variant_b)` → significance, power, confidence interval, "real or noise" verdict
- `forecast_adoption(early_sales)` → Bass diffusion curve for new products
- `explain_attribution(channel)` → model trace + version

## THE ALGORITHMS (implementation guide)
### Market Basket / Association Rules (Apriori, Agrawal 1993)
For itemsets: `support(X) = P(X)`, `confidence(X→Y) = P(Y|X)`, `lift(X→Y) = P(Y|X)/P(Y)` (lift >1 = genuine affinity). Apriori/FP-Growth to find frequent itemsets efficiently. OSS: `mlxtend`.
### Marketing Mix Modeling (regression, 1960s)
`Sales_t = β₀ + Σ βᵢ·f(Spend_{i,t}) + seasonality + trend + ε`, where f() captures **adstock** (carryover) and **saturation** (diminishing returns) — the two things that make MMM more than naive regression. Ridge/Bayesian regression for stability. The adstock+saturation transforms are where naive LLM attempts fail. OSS: `statsmodels`, or lightweight Bayesian (`pymc`) for premium.
### A/B significance (Fisher / Neyman-Pearson)
Two-proportion z-test or t-test; report p-value, confidence interval, AND statistical power (most marketers ignore power and stop tests too early — flagging under-powered tests is a differentiator). Sequential-testing guardrails to prevent peeking.
### Bass Diffusion (1969)
`f(t)/(1−F(t)) = p + q·F(t)` — innovation (p) + imitation (q) coefficients fit adoption S-curve. Reference: marketing-science texts.

## Pressure test
G1 (26/30): interpreter runs one regression ⚠️; can't re-run MMM continuously, track attribution over time, or guard test-peeking. Native ⚠️ (analytics platforms exist but are enterprise/agency-priced; mid-market runs spreadsheets — you fight the spreadsheet). G2 (19/25): moats 4 (own transaction/spend history + accumulated attribution) + 3 (continuous re-run) + 5 (explained, versioned). G3 (21/25): CSV-first, marketing is a top agent-automation area, self-serve. G4 (14/20): compounds. **Total 79.**

## Audience & user profiles
Marketing managers, growth teams, D2C/e-commerce, agencies, retail — mid-market and SMB especially (enterprises have agencies; SMBs have nothing).
1. **D2C growth lead** — agent: "Meta is over-invested past saturation; shift ₹2L to email, projected +14% return."
2. **E-commerce merchandiser** — bundle recommendations from real basket affinities, not guesses.
3. **Startup marketer** — "is this landing-page A/B win real?" → agent: "no, under-powered; need 3 more days."

## Where/how to get data & components
| Need | Source | Access |
|---|---|---|
| Spend by channel | CSV/upload, ad-platform exports (Meta/Google Ads APIs) | none for CSV; connectors later |
| Sales/transactions | POS/e-comm exports (Shopify API), CRM, CSV | none for CSV |
| A/B data | Uploaded conversion counts, analytics exports | manual/upload |
| Math | `mlxtend`, `statsmodels`, `scipy`, `pymc` (all OSS) | pip; we harden + version |

## Monetization
- **Free**: A/B calculator + basket analysis on ≤1K transactions
- **Pro ₹4,100/$49 mo**: MMM (1 model), full basket analysis, A/B with power
- **Business ₹16,500/$199 mo**: multi-channel MMM, budget optimization, adstock/saturation tuning, Bass forecasts, ad-platform pulls
- **Bundle** with Customer Tiering (14) → "Marketing + Revenue Intelligence" suite.

## India sole-prop notes
Transaction data may include personal data (customer purchases) → DPDP consent + anonymize for basket analysis. INR pricing via Razorpay; huge Indian D2C/e-commerce market is a strong home wedge. Export invoicing under LUT.

## Risks
MMM is genuinely hard to do well (adstock/saturation/multicollinearity) — ship with confidence intervals and honest "directional, not causal-proof" framing, never overclaim causality; A/B misuse via peeking (build in sequential guards); basket noise on thin data (min-support thresholds).

---

---

## LISTING & DISCOVERABILITY
*Keep this section current. It drives marketplace search, AI tool selection and the buying decision.*

**Who discovers this MCP:** agent developers, developers and agent builders (Copilot Studio makers, Agentforce, Breeze and Gemini Enterprise builders, Claude/ChatGPT connector users). They find it, test it, and **champion the purchase** to the budget owner. Every field below is written for them first, with a business tagline the manager can understand.

### 1. Names
| Field | Use this | Rule |
|---|---|---|
| **Listing title** | **Marketing Mix Modeling & A/B Testing — Saifs AI** | Capability keyword first; ≤ 60 characters before "— Saifs AI" |
| **Tagline** (for the budget owner) | Which channels pay off, where to move budget, and whether tests are real | Plain business words; ≤ 90 characters |
| **Server / connector ID** | `saifs-marketing-analytics` | Lowercase, hyphenated, no coined words |
| **Internal code name** | Marketing Intelligence (doc 24) | Docs and code only, **never** in a listing |

### 2. Description: the first 160 characters decide the click
1. **Developer contract:** Spend, sales, baskets, A/B counts (JSON) → channel ROI, budget moves, bundles, test verdicts.
2. **Business value (for the champion):** Wasted ad spend cut 10–25%.
3. **Methods (specialist search and credibility):** uses MMM, adstock, Apriori, Bass diffusion.
4. **Trust:** stateless; data is processed in the request and never stored; per-user OAuth sign-in. Observational attribution is labelled as such.

### 3. Keywords
- **Developer search terms (title, description, tags):** marketing mix modeling, mmm, marketing attribution, a/b test significance, market basket analysis, budget optimization, cac ltv
- **Business phrases (tagline, FAQ, tool descriptions):** "which ads work", "where to move budget", "is this test real"
- **Method terms (description body):** MMM, adstock, Apriori, Bass diffusion
- **Platform terms (only where actually listed):** MCP server, Copilot Studio connector, Agentforce action, Breeze agent tool, Gemini Enterprise, Amazon Bedrock AgentCore, Claude connector, ChatGPT app

### 4. Tools as shown in the listing (alphabetical)
`marketing_intelligence` · `marketing_intelligence_get_engine`

**Tool-description rule:** the AI picks tools from these descriptions, so they count as discoverability too.
- Start with a verb and the business noun.
- Name the input fields.
- Say what comes back.
- Say when **not** to use the tool.

### 5. Example prompts (listing, demo video, tool descriptions)
- "Which channel gives the best ROI?"
- "Is our landing-page test result real?"
- "What products sell together?"

### 6. Categories per marketplace
| Marketplace | Category / tags |
|---|---|
| Microsoft (Copilot Studio certified connector / AppSource) | Marketing; Analytics |
| Salesforce AgentExchange | Marketing |
| AWS Marketplace: AI Agents & Tools | Sales & Marketing → Marketing analytics |
| Google Cloud Marketplace / Gemini Agent Gallery | Marketing |
| HubSpot Marketplace (Breeze) | Marketing |

### 7. Developer try-it kit (ship with the listing)
- Free tier that works in under 10 minutes, no sales call
- Sample JSON payload shaped like a real export from: Meta/Google Ads exports, Shopify, GA, CRM
- One copy-paste example call **and** its response
- A 60–90 second demo video: prompt → tool call → answer
- Link to the docs and the MCP/OpenAPI schema; privacy policy and terms

### 8. Champion kit (for the developer to forward upward)
- **One-line business case:** Wasted ad spend cut 10–25%. Always as a range, never a bare number.
- **Security note:** stateless, nothing stored, per-user OAuth, DPDP/GDPR-friendly
- **Price:** the tier table from §Monetization, with the free tier stated first

### 9. Don'ts
- Coined names (e.g. "Marketing Intelligence") in titles or tags
- "Best", "#1" or "guaranteed" claims
- Bare money figures
- Promises beyond the guardrail: Observational attribution is labelled as such
