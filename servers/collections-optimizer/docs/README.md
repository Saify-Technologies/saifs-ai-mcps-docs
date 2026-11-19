# 37 · Collections Optimizer — Propensity-to-Pay & Contact Strategy MCP
**Score: 78/100 · IW-Breadth: 45 · Phase: STRONG REP-DAILY (the missing bank-rep tool)**
**Classical models: Propensity-to-pay (logistic regression) · Survival analysis for cure/roll rates (Kaplan-Meier/Cox, doc 22) · Contact-strategy optimization (Markov decision / uplift) · Champion-challenger testing**
**Primary industry: BANKING / LENDING (NBFC, banks, MFI, BNPL, telecom, utilities)**

## Positioning & scope
B2B, sold to **the creditor's collections operation** — banks, NBFCs, MFIs, BNPL, telecom, utilities, any business with a receivables book and recovery agents. Decision-support for the collections team; the creditor owns all contact conduct and must follow fair-collection regulation (RBI Fair Practices / FDCPA-equivalent). Regulatory-conduct guardrails written in as product spec below.

## Problem statement
Collections agents work overdue accounts every single day — but almost always by a crude rule (oldest-first, or biggest-balance-first), which wastes effort on accounts that would have paid anyway or are unrecoverable, while missing the "moveable middle" where a well-timed contact actually changes the outcome. The math to prioritize — propensity-to-pay scoring + optimal contact timing/channel — has powered sophisticated collections shops for decades but sits in expensive collections platforms (FICO, Experian PowerCurve) priced for big banks. Mid-market lenders/NBFCs/telcos run spreadsheets and gut. An LLM can't hold per-account state, score reliably, or run the daily re-prioritization loop.

## Solution
A daily-refreshed, prioritized collections work-queue with per-account strategy, agent-facing.

**Tool set (v1):**
- `score_propensity(account_data)` → P(will pay) + P(self-cure) + expected recovery
- `prioritize_queue(overdue_accounts)` → ranked daily work-list by *expected recovery uplift from contact* (not just balance)
- `recommend_contact_strategy(account)` → best channel + timing + tone-tier (call / WhatsApp / notice), per account
- `flag_self_cure(accounts)` → accounts likely to pay without contact (don't waste agent effort / avoid over-contacting)
- `predict_roll_rate(portfolio)` → survival-based bucket-to-bucket roll/cure forecasting (feeds provisioning)
- `optimize_agent_allocation(agents, queue)` → assign accounts to agents (Hungarian, doc 28) by skill/success-history
- `explain_priority(account)` → why this account, this strategy (agent trust + audit)
- `watch_queue(portfolio_source)` → nightly re-score; fresh queue each morning

## THE ALGORITHMS (implementation guide)
### Propensity-to-pay (logistic regression)
`P(pay) = 1/(1+e^{−(β₀+Σβᵢxᵢ)})` on features: days-past-due, amount, prior payment behavior, promise-kept history, contactability, product type. Standard scorecard discipline (WOE binning, doc 34 lineage). OSS `scikit-learn`/`optbinning`.
### Self-cure / roll-rate (survival, doc 22 reuse)
Kaplan-Meier cure curves + Cox hazards: P(account self-cures without contact) and bucket-transition (roll) probabilities — the key to NOT wasting effort and to provisioning.
### Contact-strategy optimization (uplift + Markov)
The real sophistication: **uplift modeling** — not "who will pay" but "whose payment probability *increases most* from being contacted" (the moveable middle). Markov decision framing over account states (current→30dpd→60dpd→cured/charged-off) with contact actions and rewards → optimal action per state. Champion-challenger to test strategies live.
### Agent allocation (Hungarian, doc 28)
Optimal account→agent assignment by success-history match.
**Notes:** accumulated contact-outcome history is the moat (moat 4 — uplift models improve with your data). OSS: `scikit-learn`, `lifelines`, `scipy`.

## Pressure test
G1 (27/30): per-account state + daily re-prioritization + uplift modeling + strategy optimization — far beyond a stateless interpreter. Native ✅ safe (big-bank collections platforms are the un-bought competition; mid-market runs spreadsheets). G2 (19/25): moats 4 (contact-outcome history → uplift models compound) + 3 (daily queue refresh) + 5 (explained priority). G3 (18/25): collections-ops buyer, regulated-conduct sale, but clear ROI (recovery ↑, agent-hours ↓). G4 (14/20). **Total 78.**

## Audience & user profiles — REP-DAILY (this is the point)
Collections/recovery agents and their team leads, at banks, NBFCs, MFIs, BNPL, telecom, utilities, and large B2B trade-credit desks.
1. **Collections agent** — opens the agent each morning to a ranked queue: "work these 40 accounts in this order; skip these 15 (likely self-cure); call this segment 6–8pm, WhatsApp the rest." Works it all day. *Pure rep-daily.*
2. **Collections team lead** — allocates accounts to agents by success-fit; tracks strategy performance (champion-challenger).
3. **NBFC recovery head** — roll-rate forecasts feed provisioning; recovery-per-agent-hour rises.

## The regulatory-conduct spec (= product requirements)
- **India:** RBI Fair Practices Code + Digital Lending / recovery-agent guidelines — contact-hour limits (no calls outside permitted hours), no harassment, frequency caps, grievance trail. **Build these as hard constraints:** the optimizer must NEVER recommend contact outside legal hours or above frequency caps; every recommendation logged for audit; tone-tier stays within compliant bounds.
- **US-market:** FDCPA / Reg F — contact frequency limits (7-in-7-day rule), permitted hours, cease-contact honoring. Same hard-constraint treatment.
- **Product consequence:** the compliance guardrails aren't caveats — they're features the collections-compliance officer buys *for*. "Optimizes recovery *within* fair-collection rules, with full audit trail" is the pitch. This also protects you reputationally (see risks).

## Where/how to get data & components
| Need | Source | Access |
|---|---|---|
| Overdue accounts + payment history | Creditor's collections system (CSV/export/API) | integration; strict DPDP |
| Contact-outcome history | Logged calls/WhatsApp/payments | accumulates → uplift moat |
| Math | `scikit-learn`, `lifelines`, `optbinning`, `scipy` (OSS) | pip; harden + govern |
| Outreach execution | Dispatch Bay (doc 06) — compliant WhatsApp/SMS/voice | internal |

## Monetization
- **Pro ₹16,500/$199 mo**: propensity scoring + daily prioritized queue + self-cure flagging (small collections ops)
- **Business ₹66,000/$799 mo**: uplift/contact-strategy optimization, agent allocation, champion-challenger, roll-rate forecasting, compliance-constrained scheduling
- **Per-account or per-recovery metering** on volume; **success-linked pricing** possible (share of uplift) — a compelling collections-industry model
- **Enterprise/bank tier**: private offers, deployment review — post-Pvt-Ltd
- High willingness-to-pay: every 1% recovery improvement on a large book is real money.

## India sole-prop notes — read before building
Debtor data = sensitive personal + financial → DPDP at full strength. **The reputational/ethical stakes are high:** collections touches vulnerable people. Build the fair-conduct constraints as *hard limits*, frame the product as "recover more *within the rules, more humanely*" (fewer wasted/harassing contacts, right-channel-right-time — genuinely better for debtors too), and choose early customers carefully. Enter via B2B trade-credit collections (business debtors, lighter sensitivity) before consumer collections. Regulated-lender tier realistically post-Pvt-Ltd + legal review. Never market as guaranteeing compliance — as *enabling* it.

## Risks
Reputational coupling to collectors' conduct (hard compliance constraints, humane framing, careful customer selection — this is the primary risk and the guardrails ARE the mitigation); debtor-data sensitivity (DPDP-grade); harassment-enablement if misused (frequency/hour caps enforced in-tool, non-negotiable, logged); regulated-buyer credibility bar (Pvt Ltd for the bank/regulated tier).

---

---

## LISTING & DISCOVERABILITY
*Keep this section current. It drives marketplace search, AI tool selection and the buying decision.*

**Who discovers this MCP:** agent developers, developers and agent builders (Copilot Studio makers, Agentforce, Breeze and Gemini Enterprise builders, Claude/ChatGPT connector users). They find it, test it, and **champion the purchase** to the budget owner. Every field below is written for them first, with a business tagline the manager can understand.

### 1. Names
| Field | Use this | Rule |
|---|---|---|
| **Listing title** | **Collections Prioritization — Saifs AI** | Capability keyword first; ≤ 60 characters before "— Saifs AI" |
| **Tagline** (for the budget owner) | Which overdue accounts to contact first and how | Plain business words; ≤ 90 characters |
| **Server / connector ID** | `saifs-collections` | Lowercase, hyphenated, no coined words |
| **Internal code name** | Collections Optimizer (doc 37) | Docs and code only, **never** in a listing |

### 2. Description: the first 160 characters decide the click
1. **Developer contract:** Overdue accounts + payment history (JSON) → priority queue, payment likelihood, best action.
2. **Business value (for the champion):** Recoveries up 5–15% with the same team.
3. **Methods (specialist search and credibility):** uses propensity, roll-rate, uplift.
4. **Trust:** stateless; data is processed in the request and never stored; per-user OAuth sign-in. No harassment patterns; fair-practice compliant.

### 3. Keywords
- **Developer search terms (title, description, tags):** collections prioritization, debt collection, dunning, accounts receivable, roll rate, payment propensity
- **Business phrases (tagline, FAQ, tool descriptions):** "who to chase first", "collections list"
- **Method terms (description body):** propensity, roll-rate, uplift
- **Platform terms (only where actually listed):** MCP server, Copilot Studio connector, Agentforce action, Breeze agent tool, Gemini Enterprise, Amazon Bedrock AgentCore, Claude connector, ChatGPT app

### 4. Tools as shown in the listing (alphabetical)
`explain_priority` · `flag_self_cure` · `optimize_agent_allocation` · `predict_roll_rate` · `prioritize_queue` · `recommend_contact_strategy` · `score_propensity` · `watch_queue`

**Tool-description rule:** the AI picks tools from these descriptions, so they count as discoverability too.
- Start with a verb and the business noun.
- Name the input fields.
- Say what comes back.
- Say when **not** to use the tool.

### 5. Example prompts (listing, demo video, tool descriptions)
- "Who should we chase for payment today?"
- "Which overdue accounts are likely to pay?"
- "Plan this week's collection calls"

### 6. Categories per marketplace
| Marketplace | Category / tags |
|---|---|
| Microsoft (Copilot Studio certified connector / AppSource) | Finance; Analytics; Compliance |
| Salesforce AgentExchange | Finance / Service |
| AWS Marketplace: AI Agents & Tools | Finance & Accounting |
| Google Cloud Marketplace / Gemini Agent Gallery | Finance |
| HubSpot Marketplace (Breeze) | Finance / Operations |

### 7. Developer try-it kit (ship with the listing)
- Free tier that works in under 10 minutes, no sales call
- Sample JSON payload shaped like a real export from: ERP receivables, LMS
- One copy-paste example call **and** its response
- A 60–90 second demo video: prompt → tool call → answer
- Link to the docs and the MCP/OpenAPI schema; privacy policy and terms

### 8. Champion kit (for the developer to forward upward)
- **One-line business case:** Recoveries up 5–15% with the same team. Always as a range, never a bare number.
- **Security note:** stateless, nothing stored, per-user OAuth, DPDP/GDPR-friendly
- **Price:** the tier table from §Monetization, with the free tier stated first

### 9. Don'ts
- Coined names (e.g. "Collections Optimizer") in titles or tags
- "Best", "#1" or "guaranteed" claims
- Bare money figures
- Promises beyond the guardrail: No harassment patterns; fair-practice compliant
# 37 · Collections Optimizer — Propensity-to-Pay & Contact Strategy MCP
**Score: 78/100 · IW-Breadth: 45 · Phase: STRONG REP-DAILY (the missing bank-rep tool)**
**Classical models: Propensity-to-pay (logistic regression) · Survival analysis for cure/roll rates (Kaplan-Meier/Cox, doc 22) · Contact-strategy optimization (Markov decision / uplift) · Champion-challenger testing**
**Primary industry: BANKING / LENDING (NBFC, banks, MFI, BNPL, telecom, utilities)**

## Positioning & scope
B2B, sold to **the creditor's collections operation** — banks, NBFCs, MFIs, BNPL, telecom, utilities, any business with a receivables book and recovery agents. Decision-support for the collections team; the creditor owns all contact conduct and must follow fair-collection regulation (RBI Fair Practices / FDCPA-equivalent). Regulatory-conduct guardrails written in as product spec below.

## Problem statement
Collections agents work overdue accounts every single day — but almost always by a crude rule (oldest-first, or biggest-balance-first), which wastes effort on accounts that would have paid anyway or are unrecoverable, while missing the "moveable middle" where a well-timed contact actually changes the outcome. The math to prioritize — propensity-to-pay scoring + optimal contact timing/channel — has powered sophisticated collections shops for decades but sits in expensive collections platforms (FICO, Experian PowerCurve) priced for big banks. Mid-market lenders/NBFCs/telcos run spreadsheets and gut. An LLM can't hold per-account state, score reliably, or run the daily re-prioritization loop.

## Solution
A daily-refreshed, prioritized collections work-queue with per-account strategy, agent-facing.

**Tool set (v1):**
- `score_propensity(account_data)` → P(will pay) + P(self-cure) + expected recovery
- `prioritize_queue(overdue_accounts)` → ranked daily work-list by *expected recovery uplift from contact* (not just balance)
- `recommend_contact_strategy(account)` → best channel + timing + tone-tier (call / WhatsApp / notice), per account
- `flag_self_cure(accounts)` → accounts likely to pay without contact (don't waste agent effort / avoid over-contacting)
- `predict_roll_rate(portfolio)` → survival-based bucket-to-bucket roll/cure forecasting (feeds provisioning)
- `optimize_agent_allocation(agents, queue)` → assign accounts to agents (Hungarian, doc 28) by skill/success-history
- `explain_priority(account)` → why this account, this strategy (agent trust + audit)
- `watch_queue(portfolio_source)` → nightly re-score; fresh queue each morning

## THE ALGORITHMS (implementation guide)
### Propensity-to-pay (logistic regression)
`P(pay) = 1/(1+e^{−(β₀+Σβᵢxᵢ)})` on features: days-past-due, amount, prior payment behavior, promise-kept history, contactability, product type. Standard scorecard discipline (WOE binning, doc 34 lineage). OSS `scikit-learn`/`optbinning`.
### Self-cure / roll-rate (survival, doc 22 reuse)
Kaplan-Meier cure curves + Cox hazards: P(account self-cures without contact) and bucket-transition (roll) probabilities — the key to NOT wasting effort and to provisioning.
### Contact-strategy optimization (uplift + Markov)
The real sophistication: **uplift modeling** — not "who will pay" but "whose payment probability *increases most* from being contacted" (the moveable middle). Markov decision framing over account states (current→30dpd→60dpd→cured/charged-off) with contact actions and rewards → optimal action per state. Champion-challenger to test strategies live.
### Agent allocation (Hungarian, doc 28)
Optimal account→agent assignment by success-history match.
**Notes:** accumulated contact-outcome history is the moat (moat 4 — uplift models improve with your data). OSS: `scikit-learn`, `lifelines`, `scipy`.

## Pressure test
G1 (27/30): per-account state + daily re-prioritization + uplift modeling + strategy optimization — far beyond a stateless interpreter. Native ✅ safe (big-bank collections platforms are the un-bought competition; mid-market runs spreadsheets). G2 (19/25): moats 4 (contact-outcome history → uplift models compound) + 3 (daily queue refresh) + 5 (explained priority). G3 (18/25): collections-ops buyer, regulated-conduct sale, but clear ROI (recovery ↑, agent-hours ↓). G4 (14/20). **Total 78.**

## Audience & user profiles — REP-DAILY (this is the point)
Collections/recovery agents and their team leads, at banks, NBFCs, MFIs, BNPL, telecom, utilities, and large B2B trade-credit desks.
1. **Collections agent** — opens the agent each morning to a ranked queue: "work these 40 accounts in this order; skip these 15 (likely self-cure); call this segment 6–8pm, WhatsApp the rest." Works it all day. *Pure rep-daily.*
2. **Collections team lead** — allocates accounts to agents by success-fit; tracks strategy performance (champion-challenger).
3. **NBFC recovery head** — roll-rate forecasts feed provisioning; recovery-per-agent-hour rises.

## The regulatory-conduct spec (= product requirements)
- **India:** RBI Fair Practices Code + Digital Lending / recovery-agent guidelines — contact-hour limits (no calls outside permitted hours), no harassment, frequency caps, grievance trail. **Build these as hard constraints:** the optimizer must NEVER recommend contact outside legal hours or above frequency caps; every recommendation logged for audit; tone-tier stays within compliant bounds.
- **US-market:** FDCPA / Reg F — contact frequency limits (7-in-7-day rule), permitted hours, cease-contact honoring. Same hard-constraint treatment.
- **Product consequence:** the compliance guardrails aren't caveats — they're features the collections-compliance officer buys *for*. "Optimizes recovery *within* fair-collection rules, with full audit trail" is the pitch. This also protects you reputationally (see risks).

## Where/how to get data & components
| Need | Source | Access |
|---|---|---|
| Overdue accounts + payment history | Creditor's collections system (CSV/export/API) | integration; strict DPDP |
| Contact-outcome history | Logged calls/WhatsApp/payments | accumulates → uplift moat |
| Math | `scikit-learn`, `lifelines`, `optbinning`, `scipy` (OSS) | pip; harden + govern |
| Outreach execution | Dispatch Bay (doc 06) — compliant WhatsApp/SMS/voice | internal |

## Monetization
- **Pro ₹16,500/$199 mo**: propensity scoring + daily prioritized queue + self-cure flagging (small collections ops)
- **Business ₹66,000/$799 mo**: uplift/contact-strategy optimization, agent allocation, champion-challenger, roll-rate forecasting, compliance-constrained scheduling
- **Per-account or per-recovery metering** on volume; **success-linked pricing** possible (share of uplift) — a compelling collections-industry model
- **Enterprise/bank tier**: private offers, deployment review — post-Pvt-Ltd
- High willingness-to-pay: every 1% recovery improvement on a large book is real money.

## India sole-prop notes — read before building
Debtor data = sensitive personal + financial → DPDP at full strength. **The reputational/ethical stakes are high:** collections touches vulnerable people. Build the fair-conduct constraints as *hard limits*, frame the product as "recover more *within the rules, more humanely*" (fewer wasted/harassing contacts, right-channel-right-time — genuinely better for debtors too), and choose early customers carefully. Enter via B2B trade-credit collections (business debtors, lighter sensitivity) before consumer collections. Regulated-lender tier realistically post-Pvt-Ltd + legal review. Never market as guaranteeing compliance — as *enabling* it.

## Risks
Reputational coupling to collectors' conduct (hard compliance constraints, humane framing, careful customer selection — this is the primary risk and the guardrails ARE the mitigation); debtor-data sensitivity (DPDP-grade); harassment-enablement if misused (frequency/hour caps enforced in-tool, non-negotiable, logged); regulated-buyer credibility bar (Pvt Ltd for the bank/regulated tier).

---

---

## LISTING & DISCOVERABILITY
*Keep this section current. It drives marketplace search, AI tool selection and the buying decision.*

**Who discovers this MCP:** agent developers, developers and agent builders (Copilot Studio makers, Agentforce, Breeze and Gemini Enterprise builders, Claude/ChatGPT connector users). They find it, test it, and **champion the purchase** to the budget owner. Every field below is written for them first, with a business tagline the manager can understand.

### 1. Names
| Field | Use this | Rule |
|---|---|---|
| **Listing title** | **Collections Prioritization — Saifs AI** | Capability keyword first; ≤ 60 characters before "— Saifs AI" |
| **Tagline** (for the budget owner) | Which overdue accounts to contact first and how | Plain business words; ≤ 90 characters |
| **Server / connector ID** | `saifs-collections` | Lowercase, hyphenated, no coined words |
| **Internal code name** | Collections Optimizer (doc 37) | Docs and code only, **never** in a listing |

### 2. Description: the first 160 characters decide the click
1. **Developer contract:** Overdue accounts + payment history (JSON) → priority queue, payment likelihood, best action.
2. **Business value (for the champion):** Recoveries up 5–15% with the same team.
3. **Methods (specialist search and credibility):** uses propensity, roll-rate, uplift.
4. **Trust:** stateless; data is processed in the request and never stored; per-user OAuth sign-in. No harassment patterns; fair-practice compliant.

### 3. Keywords
- **Developer search terms (title, description, tags):** collections prioritization, debt collection, dunning, accounts receivable, roll rate, payment propensity
- **Business phrases (tagline, FAQ, tool descriptions):** "who to chase first", "collections list"
- **Method terms (description body):** propensity, roll-rate, uplift
- **Platform terms (only where actually listed):** MCP server, Copilot Studio connector, Agentforce action, Breeze agent tool, Gemini Enterprise, Amazon Bedrock AgentCore, Claude connector, ChatGPT app

### 4. Tools as shown in the listing (alphabetical)
`explain_priority` · `flag_self_cure` · `optimize_agent_allocation` · `predict_roll_rate` · `prioritize_queue` · `recommend_contact_strategy` · `score_propensity` · `watch_queue`

**Tool-description rule:** the AI picks tools from these descriptions, so they count as discoverability too.
- Start with a verb and the business noun.
- Name the input fields.
- Say what comes back.
- Say when **not** to use the tool.

### 5. Example prompts (listing, demo video, tool descriptions)
- "Who should we chase for payment today?"
- "Which overdue accounts are likely to pay?"
- "Plan this week's collection calls"

### 6. Categories per marketplace
| Marketplace | Category / tags |
|---|---|
| Microsoft (Copilot Studio certified connector / AppSource) | Finance; Analytics; Compliance |
| Salesforce AgentExchange | Finance / Service |
| AWS Marketplace: AI Agents & Tools | Finance & Accounting |
| Google Cloud Marketplace / Gemini Agent Gallery | Finance |
| HubSpot Marketplace (Breeze) | Finance / Operations |

### 7. Developer try-it kit (ship with the listing)
- Free tier that works in under 10 minutes, no sales call
- Sample JSON payload shaped like a real export from: ERP receivables, LMS
- One copy-paste example call **and** its response
- A 60–90 second demo video: prompt → tool call → answer
- Link to the docs and the MCP/OpenAPI schema; privacy policy and terms

### 8. Champion kit (for the developer to forward upward)
- **One-line business case:** Recoveries up 5–15% with the same team. Always as a range, never a bare number.
- **Security note:** stateless, nothing stored, per-user OAuth, DPDP/GDPR-friendly
- **Price:** the tier table from §Monetization, with the free tier stated first

### 9. Don'ts
- Coined names (e.g. "Collections Optimizer") in titles or tags
- "Best", "#1" or "guaranteed" claims
- Bare money figures
- Promises beyond the guardrail: No harassment patterns; fair-practice compliant
