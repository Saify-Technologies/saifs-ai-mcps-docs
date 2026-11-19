# 59 · Quote & Bid Price Optimizer: Win-Probability & Expected-Margin Pricing MCP
**Initial moat: 73/100 (potential 77) · IW-Breadth: 60 · Phase: Wave 6 · Rep uses it directly while quoting**
**Classical models: Friedman competitive bidding (1956) · Gates bidding model (1967) · Logistic regression win-probability (Cox 1958) · Expected-margin maximisation · Price-waterfall analysis (Marn-Rosiello 1992)**

## 1. Problem statement
In B2B, most prices are negotiated quote by quote: distributors, manufacturers, contractors, IT services, logistics, packaging. Reps discount by gut feel. Some deals are lost by quoting too high, and many more are won at prices that left money on the table. Lawrence Friedman showed in 1956 that the right bid maximises **margin × probability of winning**, and that win probability can be estimated from past bids. Enterprise CPQ pricing engines (PROS, Zilliant, Vendavo) do this for large companies. Everyone else uses a discount matrix.

## 2. Solution
Quote history with outcomes + the new deal's attributes in → win-probability curve, recommended price range, expected margin at each price, and discount guardrails out.

**Tool set (v1, alphabetical):**
- `discount_leakage(quotes)`: price waterfall showing where margin leaks by rep, segment and product line
- `explain_price(quote_id)`: why this range: segment, deal size, competition, and how win odds change with price
- `fit_win_model(quote_history)`: win-probability model per segment; returned as `quote_model_snapshot`
- `price_recommendation(deal, quote_model_snapshot)`: price range, P(win) and expected margin at each price point
- `tender_bid(tender, competitors_count, history)`: Friedman/Gates sealed-bid recommendation for tenders
- `win_loss_report(quotes)`: win rate and margin by segment, price band and rep team

## 3. THE ALGORITHMS
### Win-probability curve
`P(win | m, x) = 1 / (1 + exp(−(α + γ·x − β·m)))`, where `m` = markup over cost (or discount from list) and `x` = deal features (segment, size, product, region, new vs repeat customer). Fit by regularised logistic regression; pooled hierarchically across segments when data is thin.
### Expected margin
`E(m) = m · cost · P(win | m)`. Choose `m* = argmax E(m)`. The output is a **range** where E stays within 95% of its maximum, so reps have negotiation room.
### Friedman sealed bids
Against n competitors with bid ratio distributions F, `P(win) = Π (1 − F_i(bid))`. Gates' variant for unknown competitor identity.
### Waterfall
List → invoice discounts → off-invoice items (freight, rebates, payment terms excluded: no interest logic) → pocket price. Leakage by segment.

## 4. Pressure test
- **G1 Substitution (26/30):** fitting and re-fitting win curves per segment is beyond a one-off sandbox run. CPQ engines are enterprise-priced.
- **G2 Moats (18/25):** continuity (every quote), accumulated win/loss history via snapshot, witness.
- **G3 Business (17/25):** rep uses it live, which is rare; but many CRMs don't record lost quotes well, so data clean-up is needed.
- **G4 Time (12/20):** compounds with every outcome. **Total 73; potential 77.**
- **Recurrence:** every quote ✅

## 5. Audience & user profiles
Sales reps and inside-sales teams, sales managers, pricing analysts, bid/tender managers, founders who approve discounts.
1. **Distributor rep:** "Quote ₹118–124 per unit. At ₹120, win odds are about 50% and expected margin is highest. Below ₹112 you gain little."
2. **Bid manager, contractor:** tender with 4 competitors gets a bid ratio of 1.06–1.09 × estimated cost.
3. **Sales head:** discount leakage report shows 3 segments giving away 4–6% with no win-rate gain.

## 6. Data & components (stateless contract)
**Input JSON:** `quote_history[{quote_ref, date, segment, customer_type, product, qty, cost, list_price, quoted_price, outcome: won|lost|open, competitor_count?}]`, `deal{...}`, optional `quote_model_snapshot`.
**Output JSON:** price range, P(win) curve, expected margin, explanations, updated snapshot.
Data comes from the caller's CRM/CPQ (Salesforce, HubSpot, Dynamics, Zoho). Nothing stored. Math: scikit-learn-style logistic regression (NumPy), SciPy optimisation.
**Refusal thresholds:** under 200 quotes with outcomes, or under 30 in a segment, it gives rules of thumb only and says so. Open quotes are excluded from fitting.

## 7. Monetization
- Free: 300 quotes history, 20 recommendations/month
- Pro ₹4,100/$49 mo: unlimited recommendations, 5 segments
- Business ₹16,600/$199 mo: tenders, leakage, unlimited segments, multi-currency
- Revenue Intelligence bundle with 14, 38, 24, 37

## 8. India notes & risks
Fits Indian B2B distribution, industrial supply and government tenders (GeM and CPPP bids are prepared by the caller). Values and law: **never prices on protected attributes; never uses competitor prices exchanged between rivals** (Competition Act / antitrust), only the company's own outcomes; payment-term financing is excluded (no interest logic). Risks: sparse lost-deal data (refuse thresholds), reps gaming outcomes (audit trail), over-trust (ranges, not single numbers). Disclaimer: decision support; the company sets prices.

---

---

## LISTING & DISCOVERABILITY
*Keep this section current. It drives marketplace search, AI tool selection and the buying decision.*

**Who discovers this MCP:** agent developers, developers and agent builders (Copilot Studio makers, Agentforce, Breeze and Gemini Enterprise builders, Claude/ChatGPT connector users). They find it, test it, and **champion the purchase** to the budget owner. Every field below is written for them first, with a business tagline the manager can understand.

### 1. Names
| Field | Use this | Rule |
|---|---|---|
| **Listing title** | **Quote Price Optimization (Win Probability) — Saifs AI** | Capability keyword first; ≤ 60 characters before "— Saifs AI" |
| **Tagline** (for the budget owner) | The price for each quote that maximizes expected margin | Plain business words; ≤ 90 characters |
| **Server / connector ID** | `saifs-quote-pricing` | Lowercase, hyphenated, no coined words |
| **Internal code name** | Quote & Bid Price Optimizer (doc 59) | Docs and code only, **never** in a listing |

### 2. Description: the first 160 characters decide the click
1. **Developer contract:** Quote history + deal (JSON) → price range, win probability, expected margin.
2. **Business value (for the champion):** Margin up 1–4% on quoted business.
3. **Methods (specialist search and credibility):** uses Friedman bidding, logistic regression.
4. **Trust:** stateless; data is processed in the request and never stored; per-user OAuth sign-in. No competitor price-sharing; no protected attributes.

### 3. Keywords
- **Developer search terms (title, description, tags):** price optimization, quote pricing, win probability, cpq pricing, bid pricing, discount optimization
- **Business phrases (tagline, FAQ, tool descriptions):** "what price to quote"
- **Method terms (description body):** Friedman bidding, logistic regression
- **Platform terms (only where actually listed):** MCP server, Copilot Studio connector, Agentforce action, Breeze agent tool, Gemini Enterprise, Amazon Bedrock AgentCore, Claude connector, ChatGPT app

### 4. Tools as shown in the listing (alphabetical)
`discount_leakage` · `explain_price` · `fit_win_model` · `price_recommendation` · `tender_bid` · `win_loss_report`

**Tool-description rule:** the AI picks tools from these descriptions, so they count as discoverability too.
- Start with a verb and the business noun.
- Name the input fields.
- Say what comes back.
- Say when **not** to use the tool.

### 5. Example prompts (listing, demo video, tool descriptions)
- "What price should we quote for this deal?"
- "Bid for this tender with 4 competitors"
- "Where are we leaking discount?"

### 6. Categories per marketplace
| Marketplace | Category / tags |
|---|---|
| Microsoft (Copilot Studio certified connector / AppSource) | Sales; CRM; Analytics |
| Salesforce AgentExchange | Sales |
| AWS Marketplace: AI Agents & Tools | Sales & Marketing → Sales |
| Google Cloud Marketplace / Gemini Agent Gallery | Sales / CRM |
| HubSpot Marketplace (Breeze) | Sales |

### 7. Developer try-it kit (ship with the listing)
- Free tier that works in under 10 minutes, no sales call
- Sample JSON payload shaped like a real export from: Salesforce, HubSpot, CPQ, Zoho
- One copy-paste example call **and** its response
- A 60–90 second demo video: prompt → tool call → answer
- Link to the docs and the MCP/OpenAPI schema; privacy policy and terms

### 8. Champion kit (for the developer to forward upward)
- **One-line business case:** Margin up 1–4% on quoted business. Always as a range, never a bare number.
- **Security note:** stateless, nothing stored, per-user OAuth, DPDP/GDPR-friendly
- **Price:** the tier table from §Monetization, with the free tier stated first

### 9. Don'ts
- Coined names (e.g. "Quote & Bid Price Optimizer") in titles or tags
- "Best", "#1" or "guaranteed" claims
- Bare money figures
- Promises beyond the guardrail: No competitor price-sharing; no protected attributes
