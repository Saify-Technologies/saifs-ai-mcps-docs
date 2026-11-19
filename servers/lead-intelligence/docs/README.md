# 38 · Lead Conversion & Pipeline Intelligence — Funnel Survival & Attribution MCP
**Score: 80/100 · IW-Breadth: 78 · Phase: STRONG NEW (companion to doc 14 — built via the Generator Pattern)**
**Classical models: Logistic lead scoring (1970s-80s) · Survival/hazard analysis for time-to-convert (Kaplan-Meier/Cox, 1958/72) · Markov funnel-stage modeling (Pfeifer-Carraway lineage) · Shapley-value multi-touch attribution (Shapley, 1953) · Bass Diffusion for pipeline forecasting (1969)**

*This doc is built explicitly using doc 14's Generator Pattern (Section 7) — most of its engine is doc-14 components re-aimed at leads instead of customers. Built alongside, not instead of, Customer Tiering; together they cover the full revenue lifecycle.*

---

## 1. Problem statement
Sales teams generate leads and then work them by gut: call whoever's newest, or loudest, or the rep's favorite. Marketing can't say which channel/touch actually drove the deals that closed. CRMs show a raw "lead score" (0-100) with no explanation and no rupee meaning. The math to properly answer "which leads will convert, when, and which of our touches gets credit" has existed since the 1950s-80s (survival analysis, Markov funnels, Shapley attribution) but sits in enterprise marketing-ops platforms (Marketo/Eloqua-class) priced and complexed out of the mid-market. An LLM can't hold funnel-stage transition history, fit survival curves on censored (not-yet-converted) leads, or compute Shapley values reliably (combinatorially expensive, easy to get wrong).

## 2. Solution
The same six-layer discipline as Customer Tiering, re-aimed at leads and funnel stages, plus a money layer that ranks the rep's call list by rupees, not lead-score.

**Tool set (v1):**
- `score_lead(lead_data)` → P(convert) + expected time-to-convert + explanation (Components C-lineage)
- `funnel_stage_probabilities(lead_id)` → P(reach Won) from current stage, expected time-in-stage (Component E — Markov funnel)
- `rank_call_queue(pipeline, rep_capacity)` → today's ranked list by **Value_of_Contact**, not raw lead score (Component F — Money Layer)
- `predict_pipeline_value(cohort, horizon)` → expected won-revenue from the current pipeline (Bass Diffusion at cohort level)
- `attribute_touches(lead_id)` → Shapley-value credit across marketing/sales touches that led to conversion
- `watch_stalled_leads(pipeline_source)` → nightly recompute; flags leads whose conversion hazard is dropping (going cold)
- `explain_score(lead_id)` → full trace: score, stage-probabilities, touches, model version

## THE ALGORITHMS (implementation guide — mapped explicitly to doc 14's Generator Pattern)

### Layer 1 (~ doc-14 Component A: Pareto/ranking, re-aimed)
Rank leads by expected value = `deal_size_estimate × P(convert)` — the baseline sort, cheap and replicable (free-tier layer, same role as ABC in doc 14).

### Layer 2 (~ doc-14 Component B: RFM, re-aimed as engagement scoring)
Recency (days since last touch), Frequency (touches/interactions), "Monetary"→**Engagement depth** (pages viewed, demo attended, content downloaded). Same quintile-scoring mechanic, new meaning for M.

### Layer 3 (~ doc-14 Component C: survival modeling, re-aimed — THE crown jewel here too)
Model each lead as **not-yet-converted = censored observation**, exactly like a customer who hasn't churned yet. Cox proportional hazards (or a BG/NBD-style formulation) on lead age, touch-frequency, engagement:
```
h(t | x) = h0(t) . e^{beta^T x}        # hazard of converting at time t given features x
P(convert by time t) = 1 - S(t|x)       # from the survival function
E[time_to_convert]                       # expected days to close
```
**Numerical traps (why runtime AI fails):** censoring handling (leads still open must be treated as "not yet, not never"), partial-likelihood optimization, feature scaling. Same hardening discipline as doc 14: bounds, convergence checks, refuse-with-reason on thin data. OSS: `lifelines` (same family as doc 14/22/35).

### Layer 4 (~ doc-14 Component D: CLV-style projection, re-aimed as expected deal value)
```
Expected_Pipeline_Value(lead) = P(convert) x Expected_Deal_Size x d^{E[time_to_convert]}
```
Discounted for time-value, same structure as CLV.

### Layer 5 (~ doc-14 Component E: Markov migration, re-aimed as funnel stages)
States = {Lead, MQL, SQL, Opportunity, Won, Lost}. Transition matrix P from historical funnel data:
```
P(reach Won | current stage) = row of P^n as n -> horizon, from current state
expected_time_in_stage = derived from diagonal/self-transition probabilities
```
Two matrices (rep-contacted vs. not) give the **funnel uplift** — identical mechanic to doc 14's touched/untouched, feeding the Money Layer below.

### Layer 6 (~ doc-14 Component F: THE MONEY LAYER, re-aimed at leads)
```
Value_of_Contact(lead) = Expected_Pipeline_Value(lead) x [P(convert|contacted) - P(convert|not contacted)]
Value_at_Risk(lead) = Expected_Pipeline_Value(lead) x P(goes cold if not contacted this week)
Pipeline_at_Risk = sum of Value_at_Risk(lead) for all stalling leads
```
Manager-facing: *"Rs 18L of pipeline is stalling this week across 14 leads — here's the ranked call list."* Same honesty discipline as doc 14 (Section 4e): ranges, never bare point estimates.

### NEW piece beyond doc 14 — Shapley-value attribution (Shapley, 1953)
For multi-touch credit: treat each marketing/sales touch as a "player" in a cooperative game where the "payout" is the conversion; Shapley value = average marginal contribution across all possible touch orderings:
```
phi_i = sum over subsets S (not containing touch i) of
        [|S|! (n-|S|-1)! / n!] x [v(S union {i}) - v(S)]
```
Computationally expensive at scale (combinatorial) — approximate via Monte Carlo sampling of orderings for practical lead volumes. This is genuinely hard to implement correctly and is where most marketing-attribution tools cut corners (first-touch/last-touch naive heuristics instead) — the correct Shapley implementation is a real moat. Reference: cooperative game theory texts; `shap` library's sampling approach adapts here.

---

## Pressure test (Saifs Moat Test)

**Gate 1 — Substitution (27/30):**
LLM-alone: fails (no funnel/touch history). Code-interpreter: partial threat — can fit a one-shot logistic regression, but CANNOT hold censored-lead survival state across sessions, recompute funnel-stage transition matrices nightly, or compute Shapley values correctly at scale (combinatorial + easy to get wrong). Workflow: fails (which leads to call is judgment, not pre-wired). Native: moderate risk — CRMs ship basic lead-scoring (Salesforce Einstein, HubSpot scoring) but it's black-box, single-CRM, and never funnel-Markov + Shapley + money-framed. Standalone software: marketing-ops platforms (Marketo-class) exist but are enterprise-priced, sales-led — mid-market is unserved.

**Gate 2 — Structural moats (20/25):**
Moat 4 (accumulated) — funnel transition matrices + attribution models sharpen with the tenant's own closed-deal history, strong compounding. Moat 3 (continuity) — nightly stalled-lead watching, funnel recompute. Moat 5 (witness) — explained scores + Shapley attribution with full trace, versioned. No moat 1/6/7 — good, not a fortress, matching doc 14's profile exactly.

**Gate 3 — Business reality (22/25):**
Self-serve/no-sales-team fit: excellent — CSV or CRM-pull, same onboarding as doc 14. Usage billing: natural (per-lead-scored, per-pipeline-value-tracked). Power-law: strong — this + doc 14 together could be top-2 in a "Revenue Intelligence" category with almost no agent-native competitors. Multi-platform: same engine serves Microsoft/Salesforce/HubSpot alike. Certification-compatible: yes, minimal data footprint concerns (deal metadata, not sensitive personal data at the same level as consumer data).

**Gate 4 — Time (13/20):**
Compounds via the same corpus-accumulation logic as doc 14. Regulation-tailwind: neutral/mild (marketing-attribution transparency trends help slightly). War-insurance: yes, sold to any platform.

**TOTAL SCORE: 80/100** — placing it just above doc 14's sibling-tier cluster, effectively tied with doc 28 (Route Optimizer) as one of your strongest non-flagship builds.

## Rep-directness: LIVE + PLAN (matches doc 14's pattern)
Sales reps rank their call list every morning by `Value_of_Contact`, exactly like the Customer Tiering queue — same adoption mechanic, same daily habit-forming loop, same viral peer-to-peer spread among reps.

## Audience & user profiles
Sales reps and managers, marketing ops, RevOps — the SAME buyer as doc 14, extended earlier in the lifecycle.
1. **SDR/BDR** — morning call list ranked by rupee-value-of-contact, not a mysterious lead score.
2. **Marketing ops lead** — finally knows which channel/touch actually produces closed deals (Shapley attribution), reallocates budget accordingly.
3. **Sales manager** — "Rs 18L of pipeline is going cold this week" — the same book-exposure reporting pattern as doc 14, aimed at pipeline instead of the existing customer base.

## Where/how to get data & components
| Need | Source | Access |
|---|---|---|
| Lead/funnel-stage history | CSV/upload (v1), CRM exports (Salesforce Opportunity/Lead objects, HubSpot Deals, Dynamics) | none for CSV; connectors reuse doc-14's OAuth work |
| Touch/engagement history | Marketing-automation exports (email opens, page visits), CSV | upload |
| Math | `lifelines` (survival), custom Markov + Shapley-sampling implementation (OSS-adjacent) | pip; harden + version, same discipline as doc 14 |

## Monetization
- **Free**: <=100 leads, monthly recompute, lead score only
- **Pro Rs 4,100/$49 mo**: <=1,000 leads, nightly recompute, funnel-stage probabilities, value-at-risk
- **Team Rs 12,400/$149 mo**: <=5,000 leads, ranked call queue delivered (Teams/WhatsApp), Shapley attribution, CRM pulls
- **Bundle with doc 14** as "Revenue Intelligence Suite" ₹20,000/$249 mo combined — one continuous rupee-figure experience from first lead touch through customer lifetime value. This bundle is the strategic endgame: nobody else sells the full lifecycle as one explained, money-framed product.

## India sole-prop notes
Lead/prospect data = personal data under DPDP → same consent/retention discipline as doc 14. INR pricing via Razorpay; large Indian B2B SaaS/services sales-team market is a strong wedge (many run HubSpot/Zoho with zero lead-scoring discipline). Export invoicing under LUT.

## Risks
Same as doc 14's risk profile (one-shot-analysis churn mitigated by the daily queue habit; native CRM lead-scoring encroachment fought via explanation + cross-CRM + money-framing); Shapley computational cost at high lead volume (sampling approximation, documented error bounds); cold-start on new pipelines with few closed deals (honest refuse-with-reason below data thresholds, same as doc 14's BG/NBD gate).

---

## Why this doc took 70% less original thinking than doc 14 (the Generator Pattern working as designed)
Every layer above cites which doc-14 component it repoints. This is the payoff of building doc 14 as a reusable framework rather than a one-off product: **the marginal cost of a sibling MCP drops sharply once the first one's components are named and modular.** The only genuinely new piece was Shapley attribution — everything else (survival modeling, Markov transitions, the money layer, the honesty discipline) was assembly, not invention. Expect the SAME acceleration for any future "Generator Pattern" spin-off named in doc 14 Section 7b (vendor reliability, ticket-escalation risk, content-value ranking) — budget those at 1-2 weeks each rather than 3-4.

---

---

## LISTING & DISCOVERABILITY
*Keep this section current. It drives marketplace search, AI tool selection and the buying decision.*

**Who discovers this MCP:** agent developers, developers and agent builders (Copilot Studio makers, Agentforce, Breeze and Gemini Enterprise builders, Claude/ChatGPT connector users). They find it, test it, and **champion the purchase** to the budget owner. Every field below is written for them first, with a business tagline the manager can understand.

### 1. Names
| Field | Use this | Rule |
|---|---|---|
| **Listing title** | **Lead Scoring & Pipeline Forecasting — Saifs AI** | Capability keyword first; ≤ 60 characters before "— Saifs AI" |
| **Tagline** (for the budget owner) | Which leads will convert, when, and who to call today | Plain business words; ≤ 90 characters |
| **Server / connector ID** | `saifs-lead-scoring` | Lowercase, hyphenated, no coined words |
| **Internal code name** | Lead Conversion & Pipeline Intelligence (doc 38) | Docs and code only, **never** in a listing |

### 2. Description: the first 160 characters decide the click
1. **Developer contract:** Lead rows (lead_id, created_date, stage) → P(convert), time-to-close, call queue, channel credit.
2. **Business value (for the champion):** Conversion up 10–20% from better prioritization.
3. **Methods (specialist search and credibility):** uses survival, Markov funnel, Shapley.
4. **Trust:** stateless; data is processed in the request and never stored; per-user OAuth sign-in. Ranges; observational attribution labelled.

### 3. Keywords
- **Developer search terms (title, description, tags):** lead scoring, pipeline forecasting, conversion prediction, sales prioritization, multi-touch attribution, shapley attribution
- **Business phrases (tagline, FAQ, tool descriptions):** "which leads to call", "pipeline forecast"
- **Method terms (description body):** survival, Markov funnel, Shapley
- **Platform terms (only where actually listed):** MCP server, Copilot Studio connector, Agentforce action, Breeze agent tool, Gemini Enterprise, Amazon Bedrock AgentCore, Claude connector, ChatGPT app

### 4. Tools as shown in the listing (alphabetical)
`lead_pipeline` · `lead_pipeline_get_engine`

**Tool-description rule:** the AI picks tools from these descriptions, so they count as discoverability too.
- Start with a verb and the business noun.
- Name the input fields.
- Say what comes back.
- Say when **not** to use the tool.

### 5. Example prompts (listing, demo video, tool descriptions)
- "Which leads should SDRs call today?"
- "Forecast this quarter's pipeline"
- "Which channels drive conversions?"

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
- Sample JSON payload shaped like a real export from: HubSpot, Salesforce, Zoho, CSV
- One copy-paste example call **and** its response
- A 60–90 second demo video: prompt → tool call → answer
- Link to the docs and the MCP/OpenAPI schema; privacy policy and terms

### 8. Champion kit (for the developer to forward upward)
- **One-line business case:** Conversion up 10–20% from better prioritization. Always as a range, never a bare number.
- **Security note:** stateless, nothing stored, per-user OAuth, DPDP/GDPR-friendly
- **Price:** the tier table from §Monetization, with the free tier stated first

### 9. Don'ts
- Coined names (e.g. "Lead Conversion & Pipeline Intelligence") in titles or tags
- "Best", "#1" or "guaranteed" claims
- Bare money figures
- Promises beyond the guardrail: Ranges; observational attribution labelled
