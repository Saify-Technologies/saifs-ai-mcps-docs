# 36 · Mastery Pathing — Knowledge Graph & Learning-Sequence MCP
**Score: 79/100 · IW-Breadth: 45 · Phase: NEW (education, high-moat) · Completes the ed-suite with 25+35**
**Classical models: Knowledge Space Theory (Doignon-Falmagne, 1985) · Bayesian Knowledge Tracing (Corbett-Anderson, 1994) · Elo/Glicko rating (1978/1995) · Learning Factors Analysis (Cen-Koedinger, 2006) · Prerequisite-graph / topological sequencing**

## Problem statement
Adaptive-learning products need to answer: "what should THIS learner study NEXT?" — which requires modeling (a) the *structure* of the subject (which concepts are prerequisites for which), (b) the learner's current *mastery* of each, and (c) the optimal *sequence* to advance them. Knowledge Space Theory (1985) and knowledge tracing (1994) solve this; leading platforms (Math Academy, Khan) run on exactly this math. But it's genuinely hard — a prerequisite graph + per-skill mastery model + pathing algorithm — so most ed-tech ships linear content or crude "you got 3 wrong, try again." An LLM can't hold per-learner mastery state across a knowledge graph or compute optimal paths.

## Solution
A knowledge-graph + mastery-tracking + next-best-topic engine ed-tech products call per learner.

**Tool set (v1):**
- `build_knowledge_graph(topics, prerequisites)` → validated prerequisite DAG (cycle detection, gap flags)
- `estimate_mastery(learner, responses)` → per-skill mastery via BKT/Elo across the graph
- `next_best_topic(learner)` → the "knowledge frontier": what they're ready to learn now (unlocked prerequisites, not yet mastered)
- `diagnose_gaps(learner, target_topic)` → prerequisite gaps blocking a goal + remediation path
- `optimal_sequence(learner, goal)` → topological path from current state to goal, mastery-aware
- `rate_difficulty(item_responses)` → Elo-based dynamic item/learner rating on one scale
- `explain_recommendation(learner)` → why this topic next + mastery evidence

## THE ALGORITHMS (implementation guide)
### Knowledge Space Theory (Doignon-Falmagne 1985)
Represents a domain as a knowledge structure: which sets of concepts are "learnable states"; the *outer fringe* of a learner's current state = what they're ready to learn. The "knowledge frontier" concept.
### Prerequisite graph + topological sort
DAG of concept dependencies; topological ordering gives valid learning sequences; a learner's frontier = topics whose prerequisites are all mastered. `networkx` for DAG ops.
### Bayesian Knowledge Tracing (Corbett-Anderson 1994)
Per-skill HMM (P-init, P-learn, P-slip, P-guess) updating P(mastered) after each response — same engine as doc 25, applied across the graph. OSS `pyBKT`.
### Elo/Glicko for adaptive difficulty (Doebler 2015 multivariate)
Rate learner ability and item difficulty on ONE scale, updating both after each response — a lighter, real-time alternative to full IRT for pathing. `R_new = R + K(actual − expected)`.
### Learning Factors Analysis (Cen-Koedinger 2006)
Improves the cognitive model by discovering which skills a task actually exercises.
**Notes:** the knowledge graph is partly authored (customer's curriculum) + partly learnable from data (LFA). The mastery state store + graph = the moat. OSS: `networkx`, `pyBKT`.

## Pressure test
G1 (28/30): per-learner mastery across a knowledge graph + optimal pathing = deep state + hard computation, far beyond a stateless interpreter. Native ✅ safe. G2 (20/25): moats 4 (mastery data + refined knowledge graph accumulate) + 3 (continuous per-learner state) + 5 (explained pathing). G3 (16/25): ed-tech platform buyer — focused, but this is the *core engine* they'd otherwise build. G4 (15/20): graph + mastery corpus compound. **Total 79.**

## Audience & user profiles
Adaptive-learning platforms, intelligent tutoring systems, test-prep with large curricula, corporate skills-training platforms, universities building adaptive courses.
1. **Adaptive math/coding platform** — our engine IS their "what next" brain (like Math Academy's, without building it).
2. **Test-prep with a big syllabus** — diagnoses each student's gaps and sequences remediation.
3. **Corporate skills platform** — maps employee skill gaps to learning paths.

## Where/how to get data & components
| Need | Source | Access |
|---|---|---|
| Curriculum/prerequisites | Customer's content structure | authoring UI / upload |
| Response data | Customer app → API | integration |
| Math | `networkx`, `pyBKT`, Elo (OSS) | pip; we harden |

## Monetization
- **Free**: knowledge graph (≤50 topics) + basic mastery
- **Pro ₹8,200/$99 mo**: BKT mastery, next-best-topic, gap diagnosis, larger graphs
- **Business ₹24,900/$299 mo**: full pathing engine, Elo adaptivity, LFA, large learner base
- **Platform tier**: per-MAU, white-label — ed-tech embeds it as their core engine (highest revenue)
- Positioning: "the adaptive-learning brain (Math Academy-style) as an API — don't build a learning-science team."

## India sole-prop notes
Minor learner data → DPDP + child-safety (per doc 25/35). Large Indian adaptive-ed-tech market; INR pricing. Curriculum IP stays the customer's; our engine is the reasoning layer. Export invoicing under LUT.

## Risks
Minor-data care (consent, support-framing); knowledge-graph authoring friction (guided templates + LFA-assisted discovery); cold-start mastery (Elo works fast, BKT needs data — honest ramp); over-claiming pedagogical outcomes (cite research, measure).

---

## THE EDUCATION SUITE (docs 25 + 35 + 36 together)
Three high-moat, math-model education MCPs forming a complete stack — each hard-to-build (the difficulty IS the moat), each serving the large Indian + global ed-tech/test-prep/L&D market:
- **25 Assessment Intelligence** (IRT/BKT) — *evaluate*: are the questions good, what's the learner's ability?
- **35 Learning Optimizer** (spaced repetition) — *retain*: when should they review to not forget?
- **36 Mastery Pathing** (knowledge graph/BKT/Elo) — *sequence*: what should they learn next?
Sold as the **"Adaptive Learning Engine"** suite — the three math brains ed-tech companies otherwise spend years and learning-science teams building. Bundle pricing mirrors doc 23's Decision-Math suite model. Scores 79–80 each; combined, a genuinely defensible education-infrastructure play.

---

## BUILD PLAN (added 18 October 2026 — now in progress)

### Architecture: stateless, like the rest of the portfolio
We do not store learner data. Mastery state is per-learner and long-lived, so the **caller holds it**:
- The caller sends the knowledge graph + the learner's response history (or our previously returned `mastery_snapshot`).
- We compute, return an updated `mastery_snapshot` blob, and discard everything.
- Next call, the caller sends the snapshot back. They keep the memory; we keep the math.
This keeps minors' data off our servers entirely — the strongest possible answer to DPDP and to ed-tech security reviews.

### Input contract (v1)
```json
{
  "graph": {
    "topics": [{"id":"frac_add","name":"Adding fractions"},
               {"id":"frac_basics","name":"Fraction basics"}],
    "prerequisites": [{"topic":"frac_add","requires":"frac_basics"}]
  },
  "learner_id": "pseudonymous-id-only",
  "responses": [
    {"topic":"frac_basics","item":"q12","correct":true,"ts":"2026-10-17T10:02Z"},
    {"topic":"frac_basics","item":"q15","correct":false,"ts":"2026-10-17T10:05Z"}
  ],
  "mastery_snapshot": null,
  "goal_topic": "frac_add",
  "params": {"mastery_threshold": 0.95}
}
```
`mastery_snapshot` optional; `responses` may be only the new ones since the last snapshot.

### Output contract (v1)
```json
{
  "mastery": [{"topic":"frac_basics","p_mastered":0.71,"status":"learning"}],
  "frontier": ["frac_basics"],
  "next_best_topic": {"topic":"frac_basics","why":"prerequisite of goal, not yet mastered"},
  "gaps_to_goal": ["frac_basics"],
  "path_to_goal": ["frac_basics","frac_add"],
  "mastery_snapshot": "<opaque blob — caller stores it>",
  "model_version": "mp-1.0.0",
  "confidence_note": "BKT on 2 responses; estimate will sharpen with more"
}
```

### Build order (≈3–4 weeks)
| Week | Deliverable | Detail |
|---|---|---|
| 1 | **Graph engine** | Validate prerequisite DAG; cycle detection with the offending cycle named; topological sort; frontier computation (topics whose prerequisites are all mastered and which are not yet mastered) |
| 1–2 | **BKT engine** | Per-topic Bayesian Knowledge Tracing; default parameters (P(L0)=0.3, P(T)=0.1, P(G)=0.2, P(S)=0.1) with per-topic overrides; reject degenerate params (P(G)+P(S) ≥ 1) |
| 2 | **Pathing** | `next_best_topic` (frontier ∩ ancestors of goal, lowest mastery first); `diagnose_gaps`; `optimal_sequence` as topological path through unmastered prerequisites |
| 3 | **Elo difficulty** | Learner and item ratings on one scale; used for item selection within a topic and as a fast cold-start signal before BKT has data |
| 3 | **Snapshot round-trip** | Versioned, signed snapshot blob; reject tampered or wrong-version snapshots |
| 4 | **Golden tests + MCP wiring** | Benchmark file tests T36.1 (BKT posterior 0.65853659 → next 0.69268293) and T36.2 (Elo 1520.48208), plus invariants; tool descriptions; entitlement |

### Reuse
- BKT is shared with doc 25 (Assessment Intelligence) — build it once as a shared module.
- Survival/state-tracking discipline and refusal thresholds follow doc 14.
- Doc 35 (Learning Optimizer) later adds *when to review*; Mastery Pathing decides *what to learn next*. Together with 25 they form the Adaptive Learning Engine.

### Guardrails (non-negotiable)
- **Pseudonymous IDs only** — no names, ages or school details in the payload; say so in the tool description.
- **Support framing** — outputs describe what to learn next, never rank or label children ("weak student"). No leaderboards.
- **Honest confidence** — report how many responses back each mastery estimate; refuse to declare mastery below a minimum response count.
- **Graph is the customer's** — curriculum and prerequisites are their IP; we reason over it, never redistribute it.

### First customer path
Sell to ed-tech platforms and coaching institutes as an embedded engine (per-learner-per-month or per-MAU), not to individual students. Pilot with one test-prep or K-12 platform using a single subject graph (e.g. Class 8 maths) before broadening.

---

---

## LISTING & DISCOVERABILITY
*Keep this section current. It drives marketplace search, AI tool selection and the buying decision.*

**Who discovers this MCP:** agent developers, developers and agent builders (Copilot Studio makers, Agentforce, Breeze and Gemini Enterprise builders, Claude/ChatGPT connector users). They find it, test it, and **champion the purchase** to the budget owner. Every field below is written for them first, with a business tagline the manager can understand.

### 1. Names
| Field | Use this | Rule |
|---|---|---|
| **Listing title** | **Adaptive Learning Path Engine — Saifs AI** | Capability keyword first; ≤ 60 characters before "— Saifs AI" |
| **Tagline** (for the budget owner) | The next topic each learner should study based on prerequisites | Plain business words; ≤ 90 characters |
| **Server / connector ID** | `saifs-learning-path` | Lowercase, hyphenated, no coined words |
| **Internal code name** | Mastery Pathing (doc 36) | Docs and code only, **never** in a listing |

### 2. Description: the first 160 characters decide the click
1. **Developer contract:** Topic graph + responses (JSON) → mastery map, next topics, path to goal.
2. **Business value (for the champion):** Time to mastery cut 20–40%.
3. **Methods (specialist search and credibility):** uses Knowledge Space Theory, BKT, Elo.
4. **Trust:** stateless; data is processed in the request and never stored; per-user OAuth sign-in. Child-data care; pseudonymous IDs.

### 3. Keywords
- **Developer search terms (title, description, tags):** adaptive learning, learning path, knowledge space theory, prerequisite graph, personalized learning, mastery learning
- **Business phrases (tagline, FAQ, tool descriptions):** "what should the student learn next"
- **Method terms (description body):** Knowledge Space Theory, BKT, Elo
- **Platform terms (only where actually listed):** MCP server, Copilot Studio connector, Agentforce action, Breeze agent tool, Gemini Enterprise, Amazon Bedrock AgentCore, Claude connector, ChatGPT app

### 4. Tools as shown in the listing (alphabetical)
`build_knowledge_graph` · `diagnose_gaps` · `estimate_mastery` · `explain_recommendation` · `next_best_topic` · `optimal_sequence` · `rate_difficulty`

**Tool-description rule:** the AI picks tools from these descriptions, so they count as discoverability too.
- Start with a verb and the business noun.
- Name the input fields.
- Say what comes back.
- Say when **not** to use the tool.

### 5. Example prompts (listing, demo video, tool descriptions)
- "What should this learner study next?"
- "Path to master fractions"
- "Find gaps blocking algebra"

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
- Sample JSON payload shaped like a real export from: LMS, assessment data
- One copy-paste example call **and** its response
- A 60–90 second demo video: prompt → tool call → answer
- Link to the docs and the MCP/OpenAPI schema; privacy policy and terms

### 8. Champion kit (for the developer to forward upward)
- **One-line business case:** Time to mastery cut 20–40%. Always as a range, never a bare number.
- **Security note:** stateless, nothing stored, per-user OAuth, DPDP/GDPR-friendly
- **Price:** the tier table from §Monetization, with the free tier stated first

### 9. Don'ts
- Coined names (e.g. "Mastery Pathing") in titles or tags
- "Best", "#1" or "guaranteed" claims
- Bare money figures
- Promises beyond the guardrail: Child-data care; pseudonymous IDs
