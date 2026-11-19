# 35 · Learning Optimizer — Spaced Repetition & Memory Modeling MCP
**Score: 81/100 · IW-Breadth: 48 · Phase: STRONG NEW (education, high-moat) · Pairs with doc 25**
**Classical models: Ebbinghaus Forgetting Curve (1885) · Leitner System (1972) · SM-2 (Woźniak, 1990) · Half-Life Regression (Settles-Meeder, 2016) · FSRS (2022)**

## Problem statement
Ed-tech platforms, test-prep apps, corporate L&D, and language/medical-education products want learners to *retain* what they study — but nearly all schedule reviews naively (fixed intervals, or not at all), so learners forget and re-cram. The math of *when to review each item for optimal retention* has been developed since Ebbinghaus (1885) through SM-2 (Anki's engine) to modern FSRS, and it's genuinely hard: it models each learner's forgetting curve per item and schedules the minimum reviews for a target retention. Building a correct, personalized spaced-repetition engine is real work most ed-tech teams can't do — they copy a crude SM-2 and stop. An LLM cannot maintain per-learner-per-item memory state across sessions or fit forgetting-curve parameters.

## Solution
A personalized memory-scheduling engine ed-tech products call per learner-item.

**Tool set (v1):**
- `schedule_review(learner_id, item_id, last_result)` → next optimal review time + predicted recall probability
- `due_reviews(learner_id, date)` → today's review queue, priority-ordered
- `predict_retention(learner_id, item_id, at_date)` → P(recall) at a future time
- `optimize_study_plan(learner, exam_date, item_bank)` → full schedule to peak retention by exam day (test-prep killer feature)
- `forgetting_curve(learner_id, item_id)` → the fitted decay curve + parameters
- `explain_schedule(item)` → why this interval, model + confidence

## THE ALGORITHMS (implementation guide)
### Ebbinghaus forgetting curve (1885)
Retention `R = e^{−t/S}` where t=time, S=memory strength/stability. The theoretical base.
### Leitner (1972) & SM-2 (1990)
SM-2 (Anki's engine): per-item ease factor EF (start 2.5), interval I. On review with quality q (0–5): if q≥3, `I₁=1, I₂=6, Iₙ=I_{n−1}·EF`; update `EF = EF + (0.1 − (5−q)(0.08+(5−q)0.02))` (floor 1.3); q<3 resets interval. Simple, proven, the free baseline.
### Half-Life Regression (Settles-Meeder 2016, Duolingo)
Models memory *half-life* `h_θ(x) = 2^{θ·x}` from features (past correct/incorrect, lag); recall `p = 2^{−Δ/h}`. Fit θ by gradient descent on real review data — this is where accumulated data becomes the moat (moat 4).
### FSRS (2022, modern SOTA)
Difficulty-Stability-Retrievability three-component model; open-source, outperforms SM-2. The premium engine.
**Numerical/data notes:** per-learner-per-item state is a big but simple store; HLR/FSRS fitting improves with your accumulated review corpus. OSS: `fsrs` libraries, HLR reference implementations.

## Pressure test
G1 (28/30): per-learner memory state across sessions + curve fitting is pure continuity + accumulation — impossible for a stateless interpreter. Native ✅ safe (no MCP competitor; ed-tech builds crude in-house). G2 (21/25): moats 4 (review-data corpus → better scheduling, compounds hard) + 3 (continuous per-learner scheduling) + 5 (explained). G3 (17/25): ed-tech/L&D buyer — focused vertical, large market. G4 (15/20): the review corpus is a strong compounding moat. **Total 81.**

## Audience & user profiles
Ed-tech platforms, test-prep apps (huge India market), language-learning products, medical/professional certification prep, corporate L&D/compliance training.
1. **Test-prep app** — embeds our engine so students review exactly what they're about to forget, peaking retention on exam day.
2. **Corporate L&D** — compliance knowledge actually retained, not clicked-through-and-forgotten.
3. **Language app** — vocabulary scheduling without building an SRS team.

## Where/how to get data & components
| Need | Source | Access |
|---|---|---|
| Review events (item, result, time) | Customer's app → our API | integration |
| Item bank | Customer metadata | upload/API |
| Math | `fsrs`, HLR/SM-2 references (OSS) | pip; we harden + fit |

## Monetization
- **Free**: SM-2 scheduling, ≤1K items/learner-base
- **Pro ₹4,100/$49 mo**: HLR personalization, study-plan-to-exam, retention prediction
- **Business ₹16,500/$199 mo**: FSRS engine, large learner base, accuracy analytics, API volume
- **Ed-tech platform tier**: per-MAU pricing, white-label — the real revenue (ed-tech embeds it in their product)
- Positioning: "Anki/Duolingo-grade memory scheduling as an API — don't build an SRS team."

## India sole-prop notes
**Learner data often minors → DPDP + child-safety care** (per doc 25); support-framing, no shaming. Massive Indian test-prep/ed-tech market = decisive home wedge; INR pricing. Export invoicing under LUT for foreign ed-tech.

## Risks
Minor-data sensitivity (consent, care); cold-start on personalization (SM-2 works day-one, HLR/FSRS improve with data — honest about the ramp); over-claiming retention gains (ship measured accuracy, cite the research base).

---

---

## LISTING & DISCOVERABILITY
*Keep this section current. It drives marketplace search, AI tool selection and the buying decision.*

**Who discovers this MCP:** agent developers, developers and agent builders (Copilot Studio makers, Agentforce, Breeze and Gemini Enterprise builders, Claude/ChatGPT connector users). They find it, test it, and **champion the purchase** to the budget owner. Every field below is written for them first, with a business tagline the manager can understand.

### 1. Names
| Field | Use this | Rule |
|---|---|---|
| **Listing title** | **Spaced Repetition Scheduling — Saifs AI** | Capability keyword first; ≤ 60 characters before "— Saifs AI" |
| **Tagline** (for the budget owner) | When each learner should review each topic so they remember it | Plain business words; ≤ 90 characters |
| **Server / connector ID** | `saifs-spaced-repetition` | Lowercase, hyphenated, no coined words |
| **Internal code name** | Learning Optimizer (doc 35) | Docs and code only, **never** in a listing |

### 2. Description: the first 160 characters decide the click
1. **Developer contract:** Review history (pseudonymous) → next review dates, recall probability, daily queue.
2. **Business value (for the champion):** Retention up 20–40% for the same study time.
3. **Methods (specialist search and credibility):** uses SM-2, HLR, FSRS.
4. **Trust:** stateless; data is processed in the request and never stored; per-user OAuth sign-in. Pseudonymous learners; support framing.

### 3. Keywords
- **Developer search terms (title, description, tags):** spaced repetition, sm2, fsrs, flashcard scheduling, learning retention, forgetting curve
- **Business phrases (tagline, FAQ, tool descriptions):** "when to revise", "daily revision list"
- **Method terms (description body):** SM-2, HLR, FSRS
- **Platform terms (only where actually listed):** MCP server, Copilot Studio connector, Agentforce action, Breeze agent tool, Gemini Enterprise, Amazon Bedrock AgentCore, Claude connector, ChatGPT app

### 4. Tools as shown in the listing (alphabetical)
`due_reviews` · `explain_schedule` · `forgetting_curve` · `optimize_study_plan` · `predict_retention` · `schedule_review`

**Tool-description rule:** the AI picks tools from these descriptions, so they count as discoverability too.
- Start with a verb and the business noun.
- Name the input fields.
- Say what comes back.
- Say when **not** to use the tool.

### 5. Example prompts (listing, demo video, tool descriptions)
- "What should this student revise today?"
- "Schedule revisions before the exam"
- "Predict recall for this topic"

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
- Sample JSON payload shaped like a real export from: LMS, quiz apps
- One copy-paste example call **and** its response
- A 60–90 second demo video: prompt → tool call → answer
- Link to the docs and the MCP/OpenAPI schema; privacy policy and terms

### 8. Champion kit (for the developer to forward upward)
- **One-line business case:** Retention up 20–40% for the same study time. Always as a range, never a bare number.
- **Security note:** stateless, nothing stored, per-user OAuth, DPDP/GDPR-friendly
- **Price:** the tier table from §Monetization, with the free tier stated first

### 9. Don'ts
- Coined names (e.g. "Learning Optimizer") in titles or tags
- "Best", "#1" or "guaranteed" claims
- Bare money figures
- Promises beyond the guardrail: Pseudonymous learners; support framing
