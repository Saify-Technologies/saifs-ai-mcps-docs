# 25 · Assessment Intelligence — IRT, Knowledge Tracing & Test Analytics MCP
**Score: 80/100 · IW-Breadth: 55 · Phase: STRONG NEW DOMAIN (green-light, big India angle)**
**Classical models: Item Response Theory (Lord 1952, Rasch 1960) · Bayesian Knowledge Tracing (Corbett-Anderson 1994) · Cronbach's Alpha (1951) · Computerized Adaptive Testing (built on IRT)**

## Problem statement
Ed-tech companies, coaching institutes, universities, and corporate L&D build tests and courses but evaluate them with raw percentages — which hides everything that matters: is this question actually measuring ability or just confusing everyone? What's each student's *true* ability separate from test difficulty? What has a learner actually mastered? Is this assessment even reliable? Item Response Theory (the math behind SAT/GRE/GMAT) and Knowledge Tracing (the engine inside every adaptive-learning platform) answer these — but they require psychometricians most education businesses can't afford, so they ship un-analyzed tests. An LLM can't fit IRT models reliably (they're numerically demanding) or track mastery across sessions.

## Solution
Psychometric analysis + adaptive-learning intelligence over the customer's assessment data.

**Tool set (v1):**
- `analyze_assessment(response_data)` → per-item difficulty, discrimination, guessing (IRT params) + flags for bad items
- `estimate_ability(student_responses)` → each student's true ability (θ) separate from test difficulty
- `assessment_reliability(response_data)` → Cronbach's alpha + item-total correlations
- `adaptive_next_item(student_ability, item_bank)` → optimal next question (CAT logic)
- `track_mastery(learning_history)` → Bayesian Knowledge Tracing: P(mastered) per skill over time
- `flag_at_risk_learners(cohort)` → students whose mastery/ability trajectory predicts failure
- `explain_item(question_id)` → why an item is good/bad + parameters

## THE ALGORITHMS (implementation guide)
### Item Response Theory (Lord 1952, Rasch 1960)
3-parameter logistic: `P(correct | θ) = c + (1−c)/(1 + e^{−a(θ−b)})` where θ=ability, a=discrimination, b=difficulty, c=guessing. Fit item params + person abilities by marginal maximum likelihood (EM) or MCMC. Rasch (1PL) simpler where appropriate. **Numerical demands (why runtime AI fails):** EM convergence, identifiability constraints, sparse-response handling. OSS: `py-irt`, `girth`.
### Bayesian Knowledge Tracing (Corbett-Anderson 1994)
Hidden Markov model per skill: params P(init), P(learn), P(slip), P(guess); update P(mastered) after each response. The engine behind adaptive tutors. OSS: `pyBKT`.
### Cronbach's Alpha (1951)
`α = (k/(k−1))·(1 − Σσ²ᵢ/σ²_total)` — internal consistency reliability. Simple, universally expected.
### CAT
Given current θ estimate, pick the item with maximum Fisher information at θ; re-estimate after each response. Reference: psychometrics texts; validate against published IRT datasets (golden tests).

## Pressure test
G1 (28/30): IRT/BKT are genuinely hard to fit — interpreter attempts are unreliable; the hardened, validated, mastery-tracking version is a real expertise moat. Native ✅ safe (no platform ships this to mid-market ed-tech). G2 (21/25): moats 5 (validated psychometrics + explanation) + 4 (item bank + learner-history accumulation) + 3 (mastery tracking over time). G3 (17/25): buyer is education-sector — focused vertical, but a large and paying one. G4 (14/20): item-parameter bank compounds. **Total 80.**

## Audience & user profiles
Ed-tech platforms, coaching/test-prep institutes, universities/schools, corporate L&D/certification bodies, assessment publishers.
1. **Test-prep institute** — agent: "Questions 14, 22, 31 have negative discrimination — they're measuring the wrong thing; drop them. Student cohort ability rose 0.4σ this month."
2. **Ed-tech product lead** — adaptive-testing engine without hiring a psychometrician.
3. **Corporate L&D** — mastery tracking flags which employees haven't actually learned the compliance module (vs. just clicked through).

## Where/how to get data & components
| Need | Source | Access |
|---|---|---|
| Response data (who got what right) | CSV/upload, LMS exports (Moodle/Canvas APIs), quiz-platform exports | none for CSV; connectors later |
| Item bank | Customer's question metadata | upload |
| Math | `py-irt`, `girth`, `pyBKT` (OSS) | pip; we harden + validate |

## Monetization
- **Free**: reliability (alpha) + basic item analysis on ≤1 assessment
- **Pro ₹4,100/$49 mo**: full IRT item analysis, ability estimation, multiple assessments
- **Business ₹16,500/$199 mo**: adaptive-testing engine (CAT), knowledge tracing, at-risk-learner flags, LMS pulls
- **Enterprise**: assessment-publisher volume, custom item-bank calibration
- Positioning: "the psychometrics behind the GRE, for your tests, without a psychometrician."

## India sole-prop notes
**Student data is personal data, often of minors → DPDP + heightened child-safety care.** Never expose individual student scores in ways that shame; frame at-risk flags as *support* signals for educators, not rankings. Huge Indian test-prep/ed-tech market = strong home wedge (INR pricing via Razorpay). Export invoicing under LUT.

## Risks
Minor-data sensitivity (consent, support-framing, never punitive/public ranking — this touches child-safety and wellbeing lines); IRT fit quality on small samples (minimum-response thresholds, refuse-with-reason); over-interpretation of ability estimates (always report standard errors).

---

---

## LISTING & DISCOVERABILITY
*Keep this section current. It drives marketplace search, AI tool selection and the buying decision.*

**Who discovers this MCP:** agent developers, developers and agent builders (Copilot Studio makers, Agentforce, Breeze and Gemini Enterprise builders, Claude/ChatGPT connector users). They find it, test it, and **champion the purchase** to the budget owner. Every field below is written for them first, with a business tagline the manager can understand.

### 1. Names
| Field | Use this | Rule |
|---|---|---|
| **Listing title** | **Assessment Scoring & Skill Diagnostics (IRT) — Saifs AI** | Capability keyword first; ≤ 60 characters before "— Saifs AI" |
| **Tagline** (for the budget owner) | Fair test scores, item quality and what each learner has mastered | Plain business words; ≤ 90 characters |
| **Server / connector ID** | `saifs-assessment` | Lowercase, hyphenated, no coined words |
| **Internal code name** | Assessment Intelligence (doc 25) | Docs and code only, **never** in a listing |

### 2. Description: the first 160 characters decide the click
1. **Developer contract:** Responses (pseudonymous learner, item, correct) → abilities, item stats, mastery, next item.
2. **Business value (for the champion):** Test design and analysis time cut 50–80%.
3. **Methods (specialist search and credibility):** uses IRT, BKT, CAT.
4. **Trust:** stateless; data is processed in the request and never stored; per-user OAuth sign-in. Pseudonymous IDs; support framing, never punitive.

### 3. Keywords
- **Developer search terms (title, description, tags):** item response theory, irt, adaptive testing, test analytics, psychometrics, skill diagnostics, bkt
- **Business phrases (tagline, FAQ, tool descriptions):** "score this test fairly", "which questions are bad"
- **Method terms (description body):** IRT, BKT, CAT
- **Platform terms (only where actually listed):** MCP server, Copilot Studio connector, Agentforce action, Breeze agent tool, Gemini Enterprise, Amazon Bedrock AgentCore, Claude connector, ChatGPT app

### 4. Tools as shown in the listing (alphabetical)
`adaptive_next_item` · `analyze_assessment` · `assessment_reliability` · `estimate_ability` · `explain_item` · `flag_at_risk_learners` · `track_mastery`

**Tool-description rule:** the AI picks tools from these descriptions, so they count as discoverability too.
- Start with a verb and the business noun.
- Name the input fields.
- Say what comes back.
- Say when **not** to use the tool.

### 5. Example prompts (listing, demo video, tool descriptions)
- "Score this exam with IRT"
- "Which questions are too hard?"
- "What has each student mastered?"

### 6. Categories per marketplace
| Marketplace | Category / tags |
|---|---|
| Microsoft (Copilot Studio certified connector / AppSource) | Education; Analytics |
| Salesforce AgentExchange | Education |
| AWS Marketplace: AI Agents & Tools | Industry → Education |
| Google Cloud Marketplace / Gemini Agent Gallery | Education |
| HubSpot Marketplace (Breeze) | Service |

### 7. Developer try-it kit (ship with the listing)
- Free tier that works in under 10 minutes, no sales call
- Sample JSON payload shaped like a real export from: LMS, test platforms
- One copy-paste example call **and** its response
- A 60–90 second demo video: prompt → tool call → answer
- Link to the docs and the MCP/OpenAPI schema; privacy policy and terms

### 8. Champion kit (for the developer to forward upward)
- **One-line business case:** Test design and analysis time cut 50–80%. Always as a range, never a bare number.
- **Security note:** stateless, nothing stored, per-user OAuth, DPDP/GDPR-friendly
- **Price:** the tier table from §Monetization, with the free tier stated first

### 9. Don'ts
- Coined names (e.g. "Assessment Intelligence") in titles or tags
- "Best", "#1" or "guaranteed" claims
- Bare money figures
- Promises beyond the guardrail: Pseudonymous IDs; support framing, never punitive
