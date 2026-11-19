# 26 · Hiring Fairness & Selection ROI — Adverse-Impact + Utility Analysis MCP
**Score: 72/100 · IW-Breadth: 50 · Phase: NEAR (green-light — the ETHICAL slice of HR only)**
**Classical models: Adverse-Impact / 4-5ths Rule (EEOC, 1978) · Utility Analysis (Brogden-Cronbach-Gleser, 1949–65) · Selection validity (Taylor-Russell, 1939)**

## ⚠️ SCOPE DISCIPLINE (read first)
This MCP deliberately covers ONLY the HR math that **reduces bias and supports fair decisions**. It explicitly does NOT do: predictive employee performance scoring, termination ranking, surveillance, or productivity-monitoring of individuals. Those may pass the moat test but FAIL the values test — bias risk, legal exposure a sole prop can't carry, and harm to people. We build the tools that make hiring *fairer and more measurable*, never the ones that score or rank individuals punitively. This scope line is a product decision, not a limitation to "fix later."

## Problem statement
Companies make hiring and selection decisions with no measurement of two things that matter enormously: (1) is our process **biased** against protected groups (a legal and ethical failure), and (2) is a selection method **worth its cost** (are our assessments/interviews actually improving hire quality)? Adverse-impact analysis (legally required in the US since 1978) and utility analysis have answered these for decades, but the math lives in I/O-psychology consulting most companies can't afford — so they hire blind to both bias and ROI.

## Solution
Fairness and selection-effectiveness math over hiring-process data — bias detection and method ROI, explained.

**Tool set (v1):**
- `check_adverse_impact(hiring_data_by_group)` → 4/5ths-rule analysis: which stages show disparate impact, statistical significance
- `estimate_selection_roi(method_validity, applicant_pool, performance_variance)` → dollar value of a selection method (utility analysis)
- `selection_validity(predictor_scores, outcome_data)` → does this test/interview actually predict success? (correlation + Taylor-Russell)
- `recommend_cutoff(validity, selection_ratio)` → fair, effective score cutoffs
- `explain_impact_analysis(stage)` → which groups, what ratios, legal-reference framing

## THE ALGORITHMS (implementation guide)
### Adverse-Impact / 4/5ths Rule (EEOC 1978)
Selection rate per group; impact ratio = (rate of group) / (rate of highest group). Ratio < 0.80 (4/5ths) flags potential adverse impact. Plus statistical significance (two-proportion z-test, or Fisher's exact for small n) — the 4/5ths rule alone is crude; pairing with significance is the rigorous version.
### Utility Analysis (Brogden-Cronbach-Gleser)
`ΔU = N · T · r_xy · SD_y · Z̄_x − cost` — expected dollar gain from a selection method, where r_xy=validity, SD_y=dollar value SD of performance, Z̄_x=mean standardized predictor score of hires. Reference: I/O psychology texts.
### Taylor-Russell (1939)
Given validity, selection ratio, and base rate → proportion of hires who'll succeed. Tables/formula. OSS: `scipy` for the statistical pieces (there's no single library — we encode the I/O formulas + validate against published examples).

## Pressure test
G1 (25/30): interpreter can compute ratios ⚠️; the legally-framed, significance-tested, versioned analysis + continuity (track fairness over hiring cycles) is the product. Native ✅ safe. G2 (18/25): moats 5 (legally-defensible, explained, versioned) + 3 (track over cycles) + mild 4. G3 (16/25): HR buyer, compliance-driven — narrower, procurement-shaped. G4 (13/20): regulation tailwind (bias/pay-equity laws tightening globally). **Total 72.**

## Audience & user profiles
HR/talent-acquisition teams, DEI officers, compliance, recruitment agencies — mid-market and up (regulated hiring).
1. **TA lead** — agent: "your technical-screen stage shows a 0.68 impact ratio against [group], statistically significant — review the screen for bias."
2. **DEI officer** — quarterly fairness monitoring across the funnel, audit-ready.
3. **HR at a scaling startup** — "is our new assessment worth $X? " → utility analysis says yes/no in dollars.

## Where/how to get data & components
| Need | Source | Access |
|---|---|---|
| Hiring-stage data by group | ATS exports (Greenhouse/Lever APIs), CSV | none for CSV; connectors later |
| Validity/outcome data | Performance + predictor scores, CSV | upload |
| Math | `scipy` + encoded I/O formulas | we build + validate vs. published cases |

## Monetization
- **Free**: single adverse-impact check
- **Pro ₹4,100/$49 mo**: full-funnel impact analysis, significance, utility analysis
- **Business ₹16,500/$199 mo**: continuous fairness monitoring, validity studies, ATS pulls, audit exports
- Positioning: "prove your hiring is fair and your assessments are worth it — audit-ready."

## India sole-prop notes
**Hiring data = sensitive personal data → DPDP consent, careful handling, group-level aggregation for fairness stats (never expose individuals).** Frame strictly as bias-*reduction* and support — never build the punitive/surveillance inverse. Disclaim: analysis, not legal advice (employment law varies). Global buyers (US EEOC framing) = export invoicing under LUT.

## Risks
The ethical inverse (scoring/ranking individuals) — refuse to build it regardless of demand; legal-framing accuracy (disclaim, jurisdiction-aware); small-sample fairness stats (significance + minimum-n gates). This doc's scope discipline IS its risk control.

---

---

## LISTING & DISCOVERABILITY
*Keep this section current. It drives marketplace search, AI tool selection and the buying decision.*

**Who discovers this MCP:** agent developers, developers and agent builders (Copilot Studio makers, Agentforce, Breeze and Gemini Enterprise builders, Claude/ChatGPT connector users). They find it, test it, and **champion the purchase** to the budget owner. Every field below is written for them first, with a business tagline the manager can understand.

### 1. Names
| Field | Use this | Rule |
|---|---|---|
| **Listing title** | **Hiring Fairness & Adverse Impact Analysis — Saifs AI** | Capability keyword first; ≤ 60 characters before "— Saifs AI" |
| **Tagline** (for the budget owner) | Check whether hiring stages treat groups fairly and if tests are worth it | Plain business words; ≤ 90 characters |
| **Server / connector ID** | `saifs-hiring-fairness` | Lowercase, hyphenated, no coined words |
| **Internal code name** | Hiring Fairness & Selection ROI (doc 26) | Docs and code only, **never** in a listing |

### 2. Description: the first 160 characters decide the click
1. **Developer contract:** Group counts per hiring stage (JSON) → impact ratios, significance, like-for-like check, test ROI.
2. **Business value (for the champion):** Compliance risk reduced; better selection value.
3. **Methods (specialist search and credibility):** uses adverse impact, utility analysis, Taylor-Russell.
4. **Trust:** stateless; data is processed in the request and never stored; per-user OAuth sign-in. Group-level only; never scores individuals.

### 3. Keywords
- **Developer search terms (title, description, tags):** adverse impact analysis, four fifths rule, hiring bias, eeoc compliance, selection rate, assessment validity, dei analytics
- **Business phrases (tagline, FAQ, tool descriptions):** "is our hiring biased", "80% rule check"
- **Method terms (description body):** adverse impact, utility analysis, Taylor-Russell
- **Platform terms (only where actually listed):** MCP server, Copilot Studio connector, Agentforce action, Breeze agent tool, Gemini Enterprise, Amazon Bedrock AgentCore, Claude connector, ChatGPT app

### 4. Tools as shown in the listing (alphabetical)
`hiring_fairness` · `hiring_fairness_get_engine`

**Tool-description rule:** the AI picks tools from these descriptions, so they count as discoverability too.
- Start with a verb and the business noun.
- Name the input fields.
- Say what comes back.
- Say when **not** to use the tool.

### 5. Example prompts (listing, demo video, tool descriptions)
- "Is our interview stage fair to women?"
- "Run the 4/5ths rule on our funnel"
- "Is this coding test worth it?"

### 6. Categories per marketplace
| Marketplace | Category / tags |
|---|---|
| Microsoft (Copilot Studio certified connector / AppSource) | Human Resources; Compliance |
| Salesforce AgentExchange | HR / Workforce |
| AWS Marketplace: AI Agents & Tools | Business applications → HR |
| Google Cloud Marketplace / Gemini Agent Gallery | HR |
| HubSpot Marketplace (Breeze) | Operations |

### 7. Developer try-it kit (ship with the listing)
- Free tier that works in under 10 minutes, no sales call
- Sample JSON payload shaped like a real export from: Greenhouse, Lever, Workday, ATS exports
- One copy-paste example call **and** its response
- A 60–90 second demo video: prompt → tool call → answer
- Link to the docs and the MCP/OpenAPI schema; privacy policy and terms

### 8. Champion kit (for the developer to forward upward)
- **One-line business case:** Compliance risk reduced; better selection value. Always as a range, never a bare number.
- **Security note:** stateless, nothing stored, per-user OAuth, DPDP/GDPR-friendly
- **Price:** the tier table from §Monetization, with the free tier stated first

### 9. Don'ts
- Coined names (e.g. "Hiring Fairness & Selection ROI") in titles or tags
- "Best", "#1" or "guaranteed" claims
- Bare money figures
- Promises beyond the guardrail: Group-level only; never scores individuals
