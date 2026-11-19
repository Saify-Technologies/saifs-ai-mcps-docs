# 14 · Customer Tiering Engine — A/B/C/D Segmentation, Migration & Business-Value MCP
**Score: 82/100 · Phase: BUILD FIRST · The flagship — and the template for every other Decision-Math MCP**

*This doc is written as a REUSABLE FRAMEWORK, not just a product spec. Section 7 ("The Generator Pattern") shows explicitly how to lift each piece out and build new MCPs from it. If you only read one section to spin off a new product, read Section 7.*

---

## 1. Problem statement
Sales teams treat all customers equally because nobody tells them who matters — in a language they act on. Dashboards show tier labels (A/B/C/D); managers want **rupees**. The cream gets under-served, the tail eats rep hours, and the customers *about to move between tiers* — the highest-leverage moment — are invisible until the revenue chart shows the damage, by which point it's too late. LLMs can rank a CSV once; they cannot fit survival models reliably, recompute nightly, hold migration watches, or explain a tier assignment (or a rupee figure) auditably. Native CRM scoring exists but is enterprise-gated, black-box, single-CRM, and never translates into money-at-stake.

## 2. Solution
A continuously-watched, explained, versioned customer ledger that outputs **three kinds of thing**: a tier label (for sorting), a probability (for confidence), and — the layer sales managers actually act on — **a rupee figure** (for urgency and prioritization). Input: just `customer_id, date, amount` transactions (every CRM/Tally/Excel has this).

**Tool set (v1):**
- `score_customer_book(source)` → tiered ledger A/B/C/D (CSV first; Dataverse/Salesforce/HubSpot pulls later)
- `explain_tier(customer_id)` → the auditable why: scores, model, percentile, trace
- `watch_migrations(book_id)` → nightly recompute; alerts on tier slips/jumps → the daily rep queue
- `rank_uplift_targets(tier)` → "which B's most likely reach A if touched" (Markov-based)
- `predict_customer_value(customer_id, horizon)` → expected future transactions & spend (CLV)
- `value_at_risk(customer_id)` → rupee figure: expected revenue lost if this account is neglected
- `contact_uplift(customer_id)` → rupee figure: expected revenue gained/saved by contacting, ranks the queue by money not label
- `book_exposure(book_id)` → rupee figure: total at-risk + total upgrade-upside across the whole book (the number the manager reports upward)
- `data_quality_report(source)` → dedupe/gap detection pre-step (part of the product, not overhead)

---

## 3. THE ALGORITHM STACK (six layers, each independently reusable — see Section 7)

### Layer 1 — Pareto / ABC classification (1896 to 1951, GE's Dickie)
```
sort customers by revenue desc
cum_share[i] = sum(revenue[1..i]) / total_revenue
A: cum_share <= 0.80 · B: <= 0.95 · C: <= 0.99 · D: remainder
```
Report the book's Gini coefficient (concentration health). Role: baseline + sanity check, never the headline — trivially replicable, so it's the free/demo layer.

### Layer 2 — RFM scoring (catalog marketing 1930s; Hughes 1994)
```
R_score = 6 - quintile(recency)   # recent = high
F_score = quintile(frequency)
M_score = quintile(monetary)
segment = concat(R,F,M) -> named buckets: Champions(555), At-Risk(1xx w/ high F,M), etc.
```
Role: behavioral early-warning; feeds the explanation trace ("R collapsed 5 to 2 in 60 days").

### Layer 3 — BG/NBD "is this customer alive?" (Fader-Hardie-Lee 2005; Pareto/NBD Schmittlein et al. 1987)
The crown jewel. Poisson(lambda) purchases while "alive"; silent death after any purchase w.p. p; lambda~Gamma(r,alpha), p~Beta(a,b) across the book.
Inputs: frequency x, recency t_x, customer age T.
```
P(alive | x, t_x, T)              # probability still active -- feeds the money layer (Section 4)
E[X(t) | x, t_x, T]               # expected purchases in next t
```
Fit (r,alpha,a,b) by MLE over the book. Numerical traps (why runtime AI fits fail): log-Gamma overflow, flat likelihood ridges, degenerate books. Hardening: parameter bounds, multi-start optimization, convergence diagnostics, golden tests, refuse-with-reason below data thresholds. Reference: OSS `lifetimes` package lineage.

### Layer 4 — Gamma-Gamma spend model (Fader-Hardie 2013)
```
E[M | x, m_x] = weighted blend of customer's average and population prior
CLV = sum_t  E[X(t)] * E[M] * d^t        # discounted over horizon -- the central number everything else derives from
```
Validity check: correlation(frequency, avg_value) ~ 0 -- test and report it.
Tiering on predicted CLV instead of past revenue is the product's core claim: who WILL be cream, not who WAS.

### Layer 5 — Markov migration model (Pfeifer & Carraway 2000)
States = {A,B,C,D,Lost}. Transition matrix P (row-stochastic) estimated from longitudinal recomputes. Two matrices -- touched vs. untouched by a rep -- give uplift:
```
uplift(customer) = [P_touched - P_untouched] applied to state, valued by tier-CLV vector
CLV_book = sum over horizon of  s0^T * P^t . reward_vector . d^t
```
This is "rep works to move D to A" made mathematical, and it's the engine behind the money layer's `contact_uplift`.

### Layer 6 — Assembly & explanation
Final tier = predicted-CLV ranking, annotated with ABC baseline, RFM cell, P(alive), migration risk. `explain_tier` emits the full trace + `model_version` + `trained_through_date`.

---

## 4. THE MONEY LAYER — turning the math into rupees (what managers actually act on)

The insight: a tier label is a classification; a rupee figure is a decision. Everything below is arithmetic on numbers Layers 3-5 already produce -- no new modeling, just reframing the output.

### 4a. Value at risk (urgency)
```
Value_at_Risk(customer) = CLV(customer) * P(churn | neglected)
```
Where P(churn | neglected) = 1 - P(alive) from Layer 3, or more precisely the projected drop in P(alive) over the next period if no contact/purchase occurs. Manager-facing: "Acme is worth Rs 6L/yr; 35% lapse risk this quarter -> Rs 2.1L at risk."

### 4b. Value of contact / uplift (prioritization -- ranks the queue)
```
Value_of_Contact(customer) = CLV(customer) * [ P(retain | contacted) - P(retain | not contacted) ]
```
The bracketed term comes straight from Layer 5's touched-vs-untouched matrices. This is the number that should rank the daily queue -- NOT the tier label. An account that stays loyal regardless has near-zero uplift (low priority even if it's an A); an account that a call would visibly save has high uplift (top of the queue, even if it's a B).

### 4c. Upgrade upside (growth framing -- the positive mirror of 4a)
```
Upgrade_Upside(customer) = (CLV_as_next_tier - CLV_current) * P(upgrade | contacted)
```
Manager-facing: "Invest an hour in Beta Ltd -> unlock Rs 1.8L of growth."

### 4d. Book-level exposure (the number that goes in the manager's report upward)
```
Book_at_Risk = sum of Value_at_Risk(c) for c in neglected segment
Book_upside  = sum of Upgrade_Upside(c) for c in contactable B/C segment
```
"Rs 34L at risk across 23 neglected accounts. Rs 52L upgrade upside in 41 B-accounts."

### 4e. Honesty discipline (non-negotiable -- this is what makes managers trust the number twice)
Every rupee figure ships as a range with the model's actual confidence, never a bare point estimate:
```
"~Rs 4.2L at risk (Rs 3.1L-5.4L, 90% CI)" -- never just "Rs 4.2L"
```
And every figure carries its explanation trace (Layer 6) one click away. A manager who sees a confident "Rs 4.2L" churn anyway and later discovers it was a rough estimate stops trusting every number you've ever shown them. A manager who sees a range and a reason keeps trusting you when reality lands inside the range.

---

## 5. "If it's this easy, why didn't Salesforce build it?" (memorize for investor/judge questions)
They partially did -- proving demand, not saturation:
1. Enterprise-gated. Einstein-class scoring exists behind expensive editions and data-science setup. SMB/mid-market with a CSV is structurally unserved.
2. Black-box vs. explained. Native scores are opaque; reps distrust and ignore them. Our traced, versioned math is a positioning choice incumbents can't retrofit.
3. Single-CRM prisons. Salesforce scores Salesforce data. Real businesses have data in Tally + CRM + Excel. Cross-source scoring is against platform incentives, aligned with ours.
4. The math isn't their differentiator. Platforms invest where they differentiate (their data, their UI); commoditized-literature models are classic ISV territory.
5. The agent surface + money-layer is new. A daily rupee-ranked queue delivered into Teams/WhatsApp didn't exist 18 months ago.

## 6. Audience, data sources, monetization, India notes
Audience: sales managers/founders at SMB/mid-market (distributors, agencies, D2C, travel trade); reps consuming the daily queue; RevOps consultants running multiple client books.

Data: CSV/XLSX (launch vehicle, no integration needed) -> Dataverse Web API, Salesforce REST/Bulk (Opportunity/Order), HubSpot/Zoho Deals APIs (read-only OAuth) as connectors mature. Model reference: OSS `lifetimes` lineage -- harden, never just wrap. Jobs/watchers: doc-03 engine.

Monetization:
- Free: <=250 customers, monthly recompute, tier labels only -- the demo that closes itself
- Pro Rs 4,100/$49 mo: <=2,500 customers, nightly recompute, migration alerts, explanations, value-at-risk figures
- Team Rs 12,400/$149 mo: <=10,000 customers, rep queues delivered (Teams/WhatsApp), uplift-ranked queue + book exposure reports, CRM pulls
- Consultant Rs 24,900/$299 mo: 10 books, white-label reports
- The money layer (Section 4) is the natural Pro-to-Team upsell trigger: "see WHO is at risk" (Pro) vs. "see HOW MUCH and act on the ranked list" (Team).

India notes: transaction data = personal data under DPDP -> consent, retention policy, delete-on-request. INR pricing via Razorpay is a genuine wedge (Tally-exporting SMBs are a massive unserved base). Export invoices under LUT.

Risks: one-shot-analysis churn (mitigation: the daily rupee-figure habit IS retention); native-feature encroachment (lane = cross-CRM + explained + money-framed + SMB-priced); overconfident rupee figures (Section 4e discipline is the mitigation, not optional); non-contractual-model misuse on pure-subscription books (documented renewal-mode variant needed, honest refusals below data thresholds).

---

## 7. THE GENERATOR PATTERN — how to build NEW MCPs out of this doc

This is the section to use when you want to create something new. Customer Tiering isn't one idea -- it's five reusable components, each of which generalizes to other domains. Recognizing the component is how you spin off a new product in an afternoon instead of starting from zero.

| Component | What it does here | Generalizes to | Already used in |
|---|---|---|---|
| A. Pareto/ABC ranking | Rank customers by revenue | Rank ANY entity by ANY cumulative value (suppliers, products, regions, support tickets by cost) | doc 16 Inventory (SKU ranking) |
| B. RFM-style recency/frequency/monetary scoring | Behavioral early-warning on customers | Any entity with event history: employee engagement (last-active/frequency), equipment (last-service/usage), content (last-viewed/frequency) | -- open for a new doc |
| C. BG/NBD "alive" survival modeling | P(customer still active) | P(any entity still "alive" in its relationship): P(employee still with us), P(subscription still active), P(equipment still in service before failure) | doc 22 Cash & Retention (employee/customer survival) |
| D. Gamma-Gamma / CLV-style value projection | Expected future value of a customer | Expected future value of ANY recurring relationship: vendor contract value, subscription LTV, asset residual value | -- the CLV pattern is portable to any "value over time" problem |
| E. Markov migration + uplift | P(state transition) + value of intervention | ANY state-transition system: delinquency buckets (doc 37 Collections roll-rate), project-risk states, inventory health states, employee career-stage transitions | doc 37 Collections (roll-rate is literally this layer reused) |
| F. THE MONEY LAYER (Section 4) | Converts probability x value into a rupee figure managers act on | Apply this to EVERY MCP in the portfolio that produces a probability or a classification -- it's the universal "make it actionable" wrapper | Not yet applied elsewhere -- see below |

### 7a. The single highest-leverage move available to you right now
Apply the Money Layer (Section 4's three formulas) to every other MCP that currently outputs a label or probability instead of a rupee figure. Concretely:

- Doc 05 Prediction House (`predict_delay`) -> "this shipment delay risk costs you Rs X in SLA penalties/customer goodwill"
- Doc 20 Process Monitor (out-of-control signal) -> "this quality drift is costing Rs X/day in defects"
- Doc 19 Counterparty Risk (Altman Z zone) -> "extending credit here risks Rs X of receivables"
- Doc 09 Accessibility Plant (conformance gap) -> "this compliance gap risks Rs X in potential litigation exposure"
- Doc 26 Hiring Fairness (adverse-impact ratio) -> "this bias pattern risks Rs X in potential legal exposure" (careful, factual framing only)
- Doc 33 Lending Fairness (disparate-impact) -> same pattern

The formula template to copy for any of these:
```
Money_Figure = Impact_if_ignored(entity) * Probability_of_ignored_outcome(entity)
Money_Figure_range = same, with the model's actual confidence interval attached (Section 4e discipline)
```
This single addition -- a rupee/impact translation layer -- could become its own cross-cutting feature bundled into every Decision-Math MCP rather than a one-off. Consider documenting it as a shared "Business Impact Layer" in TECH-00 (the technical playbook), the same way the audit log or entitlement middleware is shared substrate.

### 7b. How to spin off a genuinely NEW MCP from a single component
Pick any row in the table above, ask "what other entity/domain has this same shape of data," and you have a new doc. Worked examples:
- Take component C (survival/alive modeling) + apply to vendors instead of customers -> "Vendor Reliability Score: P(this supplier is still a going concern)" -- a new MCP for procurement teams.
- Take component E (Markov migration) + apply to support tickets instead of customers -> "Ticket-Escalation Risk: P(this ticket rolls from Tier-1 to Tier-3)" -- a new MCP for support ops.
- Take component A+D (Pareto + CLV-style projection) + apply to content/products instead of customers -> "Content Value Ranking: which articles/SKUs will earn the most over the next quarter" -- a new MCP for media/e-commerce.

The discipline that makes this repeatable: every new spin-off must still pass the four gates (Substitution / Structural Moats / Business Reality / Time) from the Saifs Moat Test -- the generator gives you the math, not an automatic pass. Check the new candidate against the same rigor before building it.

---

## 8. THE DAILY FEEDBACK LOOP (stateless design) + CUSTOMER TERMINATION

### 8.1 The problem this solves
Without feedback, the queue is a report: it re-ranks the same book every day and hands the rep the same names. A rep who called Acme yesterday sees Acme at rank 1 again today, distrusts the tool by day three, and stops opening it. The fix is a feedback edge -- but as an MCP we do NOT call CRM APIs ourselves and we do NOT want to store customer data.

### 8.2 The stateless contract (chosen architecture)
The caller supplies contact state in the same JSON payload. The queue becomes a pure function:
```
queue = f(transactions, contact_state, as_of_date, capacity)
```
Same inputs twice = same output. Different day = different output, because `as_of` moved and recency decayed. No customer data persisted at our end.

```json
{
  "book": [
    {"customer_id":"c_882","date":"2026-05-20","amount":48000},
    {"customer_id":"c_882","date":"2026-04-02","amount":51000}
  ],
  "contact_state": [
    {"customer_id":"c_882","last_contact":"2026-07-26","outcome":"no_answer"},
    {"customer_id":"c_119","last_contact":"2026-07-11"}
  ],
  "as_of": "2026-07-27",
  "capacity": 40
}
```
`contact_state` is OPTIONAL; `outcome` is optional within it. Tool description must instruct the orchestrator: *"Pass each customer's last engagement date from your CRM's activity data. If omitted, queue rotation falls back to presumed-contact modelling and confidence is reported lower."*

### 8.3 What we DO and DO NOT keep
- **KEEP**: `tenant_id` / `user_id` from the OAuth token -- required for billing, metering and MAU counting. Tiny, no customer PII.
- **DO NOT KEEP**: the book, transactions, contact dates, customer identifiers. Computed in-request, discarded.
- **Optional efficiency trick**: return a `model_snapshot` blob the caller can pass back next call, so THEY hold the fitted parameters and we stay stateless while avoiding a refit.
- **Sell this**: "we never store your customer data -- everything is computed in-request and discarded" removes a major enterprise-security objection and cuts DPDP obligations sharply. Headline feature, not a footnote.

### 8.4 Queue logic with contact state
```
for each customer:
    base = Value_of_Contact(customer)
    days_since = as_of - last_contact
    if days_since < cooldown(outcome):  suppress, UNLESS churn risk spiked sharply
    outcome adjustments:
      promised_to_buy -> suppress until followup date, then resurface hard
      order_placed    -> recompute tier, suppress from queue
      not_interested  -> long cooldown (30-60 days)
      no_answer       -> short cooldown (1-2 days), rotate channel/time
today_queue = rank(remaining, by adjusted Value_of_Contact)
```
Every queue entry returns WHY it is there today: `new_at_risk | followup_due | retry_no_answer | never_contacted`.

### 8.5 Degradation when contact state is absent
- **Tier 1 (rich)**: contact_state supplied -> full logic above.
- **Tier 2 (presumed)**: no contact data -> presumed-contact decay. Reps work lists top-down, so suppress by rank position of our own prior recommendation (if the caller passes back `previous_queue`), decayed by days elapsed. Label output as modelled, not measured.
- **Tier 3 (inferred)**: diff today's transactions against the payload's history -- a new order after a recommendation is evidence contact happened and worked.
- Always return `"queue_mode": "outcome_informed" | "inferred" | "presumed"` plus a confidence note. Honest degradation, visibly labelled.

### 8.6 The long-lag problem (order takes 8-20+ days to appear)
Do NOT treat "no transaction yet" as "contact failed" -- it is a **censored observation**, the same statistical object BG/NBD already handles.
Three states per account: `ACTIVE` (workable) -> `PENDING` (recommended, plausibly in-flight, do not resurface, do not judge) -> `RESOLVED` (order landed, or window expired).
Pending window is derived from the customer's OWN rhythm, not a flat number:
```
pending_window(c) = median_IPT(c) + k * dispersion(c)        (k ~ 1)
```
Thin history -> fall back to segment median IPT, then book median.
**Key reassurance:** even with identical data for 20 days, the queue still moves daily, because P(alive) decays against `as_of`. Value_at_Risk rises every silent day; accounts cross their pending boundary on different days. The queue evolves on the clock, not the data.

### 8.7 Lag-weighted attribution
When an order lands 22 days after a recommendation, credit it with a distributed-lag kernel (Koyck/adstock family, 1954):
```
credit(recommendation at d | order at t) proportional to w(t - d)
w = lognormal or Weibull pdf fitted to observed (recommendation -> order) gaps
```
Split credit across multiple prior recommendations by kernel weight. Over time the fitted kernel becomes per-tenant knowledge of their sales cycle.

### 8.8 Randomised holdout (the measurement protocol)
Withhold a random 5-10% of eligible accounts from the queue each cycle, matched on tier and CLV.
```
uplift = conversion_rate(recommended) - conversion_rate(holdout)
```
This yields a genuine touched-vs-untouched estimate with zero contact data and zero CRM integration -- and produces the single best sales sentence available: *"accounts we surfaced converted X% more than a matched control."*
**Disclose it in-product** ("a small rotating control group is held back to measure lift") and rotate membership so no account is permanently neglected. Silently withholding a manager's high-value accounts would be a bad surprise to discover later.

---

## 9. CUSTOMER TERMINATION — when is a customer actually dead?

### 9.1 There is no "terminated" flag -- it is a threshold decision
BG/NBD never returns P(alive) = 0; it returns a probability that asymptotically approaches zero. So termination is a POLICY we define on top of the model, and it must be stated explicitly rather than implied:
```
DORMANT   : P(alive) < 0.20
LAPSED    : P(alive) < 0.05  AND  (as_of - last_purchase) > 3 * median_IPT(c)
```
Require BOTH conditions. P(alive) alone can be misleadingly low for a naturally slow buyer; the IPT multiple grounds it in that customer's own rhythm.

### 9.2 Never hard-delete -- reclassify
A "lapsed" customer stays in the book, moves to a `LAPSED` tier below D, and exits the daily queue. They surface only in a periodic **win-back list** (quarterly), never in the daily rep queue. Reasons: reactivation happens, statistical death is not certainty, and a resurrected account carries full history.

### 9.3 Never-purchased is a DIFFERENT object
BG/NBD models customers who have made at least one purchase. A prospect with zero transactions is not a dormant customer -- it is a **lead**, and belongs to doc 38 (Lead Conversion), which handles it with survival analysis on censored non-conversions. Do not feed zero-transaction records into the tiering model; route them out with a clear reason.
Note also: customers with exactly ONE purchase (x = 0 repeats) are handled by BG/NBD but weakly -- flag them as `PROVISIONAL` and report wider confidence.

### 9.4 Report status, not verdicts
Output `{status, p_alive, days_since_last, median_ipt, multiples_of_ipt_elapsed}` so the manager sees the reasoning, not a bare label. "Lapsed" attached to a real account is a consequential claim; show the evidence.

---

## 10. HOW MUCH HISTORY IS ENOUGH? (is one year sufficient?)

### 10.1 Short answer: it depends entirely on interpurchase time -- one year is often NOT enough
The window must be measured in **purchase cycles, not calendar time**.
```
minimum_window(c) >= 4 * median_IPT(c)          (for reliable P(alive))
preferred_window  >= 2 full seasonal cycles      (to separate season from trend)
```

| Buyer rhythm | Purchases in 1 year | 1-year verdict |
|---|---|---|
| Weekly (FMCG retail, cafes) | ~50 | Plenty. 1 year is generous |
| Monthly (subscription-ish, distributors) | ~12 | Adequate for tiering; thin for seasonality |
| Quarterly (B2B supplies) | ~4 | Marginal -- barely distinguishes alive from dead |
| Semi-annual / annual (capital goods, insurance) | 1-2 | **Insufficient.** Cannot fit BG/NBD meaningfully |

### 10.2 The two failure modes of a one-year window
1. **Slow buyers look dead.** A customer who orders every 8 months with one order 9 months ago looks lapsed on a 1-year window; on a 3-year window they are obviously a normal slow buyer whose next order is due. This produces false LAPSED calls on exactly the high-value B2B accounts you least want to abandon.
2. **Left-truncation makes old customers look new.** Cutting history at 12 months sets every customer's age T to at most 365 days, so a 9-year loyal customer is modelled as a 1-year customer. BG/NBD's P(alive) depends on the relationship between recency and age -- truncating age systematically distorts it.

### 10.3 What to require, and what to do when you cannot get it
- **Ask for 24-36 months** in the tool description. State the reason: seasonality plus enough purchase cycles.
- **Accept less, but degrade honestly.** Compute `cycles_observed = window_days / median_IPT` per customer and:
  - `cycles >= 4` -> full scoring
  - `2 <= cycles < 4` -> score with widened confidence, flag `LIMITED_HISTORY`
  - `cycles < 2` -> refuse to assign a tier; return `INSUFFICIENT_HISTORY` with the reason and what would fix it
- **Never silently score on thin data.** A confident tier built on two observations is exactly the failure that destroys trust in the whole product.
- **Seasonality needs 2+ cycles regardless.** With 12 months you have one cycle; you cannot separate "December is always big" from "they are growing." Report seasonal claims only when 2+ cycles exist (this constraint applies equally to doc 18 Demand Forecaster).

### 10.4 Practical default for the onboarding prompt
> *"Provide at least 24 months of transaction history (customer_id, date, amount). If your business has long purchase cycles -- quarterly or slower -- provide 36 months. With less than 24 months, customers whose buying cycle exceeds a quarter will be scored with reduced confidence or skipped."*

This is also a natural upsell moment: customers who supply richer history get better output, which is a fair and honest reason for them to do the work.

---

---

## LISTING & DISCOVERABILITY
*Keep this section current. It drives marketplace search, AI tool selection and the buying decision.*

**Who discovers this MCP:** agent developers, developers and agent builders (Copilot Studio makers, Agentforce, Breeze and Gemini Enterprise builders, Claude/ChatGPT connector users). They find it, test it, and **champion the purchase** to the budget owner. Every field below is written for them first, with a business tagline the manager can understand.

### 1. Names
| Field | Use this | Rule |
|---|---|---|
| **Listing title** | **Customer Segmentation & Churn Risk (RFM, CLV) — Saifs AI** | Capability keyword first; ≤ 60 characters before "— Saifs AI" |
| **Tagline** (for the budget owner) | Rank customers A/B/C/D, flag who's slipping and build today's call list | Plain business words; ≤ 90 characters |
| **Server / connector ID** | `saifs-customer-segmentation` | Lowercase, hyphenated, no coined words |
| **Internal code name** | Customer Tiering Engine (doc 14) | Docs and code only, **never** in a listing |

### 2. Description: the first 160 characters decide the click
1. **Developer contract:** Purchase rows (customer_id, date, amount) → tiers, churn risk, value at risk, daily call queue.
2. **Business value (for the champion):** Revenue at risk identified as a range; 2–8% revenue retained from top accounts.
3. **Methods (specialist search and credibility):** uses Pareto, RFM, BG/NBD, Markov migration.
4. **Trust:** stateless; data is processed in the request and never stored; per-user OAuth sign-in. Money figures as ranges; caller holds contact_state; no data stored.

### 3. Keywords
- **Developer search terms (title, description, tags):** customer segmentation, churn prediction, rfm analysis, customer lifetime value, clv, abc analysis, account prioritization, customer retention
- **Business phrases (tagline, FAQ, tool descriptions):** "who should we call today", "top customers", "which accounts are slipping"
- **Method terms (description body):** Pareto, RFM, BG/NBD, Markov migration
- **Platform terms (only where actually listed):** MCP server, Copilot Studio connector, Agentforce action, Breeze agent tool, Gemini Enterprise, Amazon Bedrock AgentCore, Claude connector, ChatGPT app

### 4. Tools as shown in the listing (alphabetical)
`customer_tiering` · `customer_tiering_get_engine`

**Tool-description rule:** the AI picks tools from these descriptions, so they count as discoverability too.
- Start with a verb and the business noun.
- Name the input fields.
- Say what comes back.
- Say when **not** to use the tool.

### 5. Example prompts (listing, demo video, tool descriptions)
- "Who should my reps call today?"
- "Which of my top customers are going quiet?"
- "Tier my HubSpot customers A to D"

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
- Sample JSON payload shaped like a real export from: HubSpot, Salesforce, Dynamics, Zoho, Tally, CSV
- One copy-paste example call **and** its response
- A 60–90 second demo video: prompt → tool call → answer
- Link to the docs and the MCP/OpenAPI schema; privacy policy and terms

### 8. Champion kit (for the developer to forward upward)
- **One-line business case:** Revenue at risk identified as a range; 2–8% revenue retained from top accounts. Always as a range, never a bare number.
- **Security note:** stateless, nothing stored, per-user OAuth, DPDP/GDPR-friendly
- **Price:** the tier table from §Monetization, with the free tier stated first

### 9. Don'ts
- Coined names (e.g. "Customer Tiering Engine") in titles or tags
- "Best", "#1" or "guaranteed" claims
- Bare money figures
- Promises beyond the guardrail: Money figures as ranges; caller holds contact_state; no data stored
