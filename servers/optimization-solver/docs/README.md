# 29 · Constraint Optimizer — Linear & Mixed-Integer Programming MCP
**Score: 76/100 · IW-Breadth: 62 · Phase: NEAR (the universal operations tool)**
**Classical models: Linear Programming / Simplex (Dantzig, 1947) · Mixed-Integer Programming · Diet/Blending Problem (Stigler, 1945) · Transportation Problem (Hitchcock, 1941)**

## Problem statement
Countless business decisions are "maximize/minimize X subject to constraints": production mix (what to make given machine/material limits), blending (cheapest feed/fuel/alloy meeting specs), workforce scheduling (cover shifts at min cost), transportation (source→destination at min cost), capital allocation. Linear Programming has optimally solved this entire class since 1947 — one of the most valuable algorithms ever invented — yet mid-market/SMB solve these by spreadsheet trial-and-error, leaving money on the table. An LLM cannot solve an LP (needs a simplex/interior-point solver); it can *set one up* but not optimize it.

## Solution
A general constraint-optimization MCP: describe the objective + constraints, get the provably optimal answer. Plus pre-built templates for common problems so users don't need to know LP.

**Tool set (v1):**
- `optimize_production_mix(products, resources, profits, constraints)` → optimal quantities + profit
- `optimize_blend(ingredients, requirements, costs)` → least-cost mix meeting specs (diet/blending)
- `optimize_transportation(sources, destinations, costs, supply, demand)` → min-cost shipping plan
- `optimize_schedule(shifts, staff, rules)` → min-cost coverage
- `solve_custom_lp(objective, constraints)` → general solver for power users
- `explain_solution(model)` → which constraints bind, shadow prices (what each constraint costs you), sensitivity

## THE ALGORITHMS (implementation guide)
### Linear Programming (Simplex, Dantzig 1947 / interior-point)
Maximize `cᵀx` subject to `Ax ≤ b, x ≥ 0`. Simplex or interior-point finds the exact optimum at a vertex of the feasible polytope. **Shadow prices (dual values)** — what one more unit of each constrained resource is worth — are often more valuable than the answer itself, and a naive LLM never surfaces them.
### Mixed-Integer Programming (MIP)
When variables must be whole (people, vehicles, yes/no) — branch-and-bound/cut. Harder but essential for scheduling/assignment.
### Templates (the UX moat)
Most users can't formulate an LP. Pre-built templates (production, blend, transport, schedule) let them fill a table, not write math — this framing layer is where the product value sits, not the solver. Solver: **OR-Tools** (GLOP/SCIP) or **PuLP** (both OSS, free). Reference: Dantzig's work; OR-Tools/PuLP docs.

## Pressure test
G1 (25/30): LP is beyond interpreter without a solver ⚠️; but "set up an LP" is semi-doable — so the moat is the templates + shadow-price explanation + continuity, not raw solving. Native ✅ safe (Excel Solver exists but is manual/single-user; you're the agent-native, templated, explained version). G2 (18/25): moats 7 (solver) + 5 (explained, shadow prices) + mild 4. G3 (19/25): extremely broad applicability but needs good problem-framing UX (the risk). G4 (13/20). **Total 76.**

## Audience & user profiles
Manufacturers (production/blending), operations/supply-chain, workforce planners, agriculture (feed/fertilizer), logistics (transport), finance (allocation — business-level, not personal).
1. **Manufacturer** — agent: "make 340 units of A, 180 of B for max profit given your machine hours; your bottleneck is the CNC line — each extra hour is worth ₹4,200."
2. **Feed producer** — least-cost blend meeting protein/energy specs (Stigler's original problem, still huge in agri/livestock).
3. **Ops scheduler** — min-cost shift coverage meeting labor rules.

## Where/how to get data & components
| Need | Source | Access |
|---|---|---|
| Problem data (costs, constraints) | CSV/template upload, ERP exports | none for CSV |
| Solver | **OR-Tools** (GLOP/SCIP), **PuLP** (OSS) | pip, free |

## Monetization
- **Free**: small problems (≤10 variables), template access
- **Pro ₹4,100/$49 mo**: full templates, shadow-price analysis, medium problems
- **Business ₹16,500/$199 mo**: MIP, large problems, custom LP builder, ERP pulls, re-optimization
- Positioning: "the optimization Excel Solver does — but in your agent, templated, and it tells you what your bottleneck costs."

## India sole-prop notes
Problem data typically non-personal (light DPDP). Agriculture blending + manufacturing = large under-served Indian markets. OR-Tools/PuLP free. Export invoicing under LUT.

## Risks
Problem-framing UX is the make-or-break (invest in templates; general LP builder only for power users); infeasibility handling (clear "no solution because constraint X conflicts with Y" explanations); over-scoping ("optimize anything" is too vague — lead with named templates).

---

---

## LISTING & DISCOVERABILITY
*Keep this section current. It drives marketplace search, AI tool selection and the buying decision.*

**Who discovers this MCP:** agent developers, developers and agent builders (Copilot Studio makers, Agentforce, Breeze and Gemini Enterprise builders, Claude/ChatGPT connector users). They find it, test it, and **champion the purchase** to the budget owner. Every field below is written for them first, with a business tagline the manager can understand.

### 1. Names
| Field | Use this | Rule |
|---|---|---|
| **Listing title** | **Optimization Solver (LP / MIP Scheduling) — Saifs AI** | Capability keyword first; ≤ 60 characters before "— Saifs AI" |
| **Tagline** (for the budget owner) | Solve planning, allocation and scheduling problems optimally | Plain business words; ≤ 90 characters |
| **Server / connector ID** | `saifs-optimization-solver` | Lowercase, hyphenated, no coined words |
| **Internal code name** | Constraint Optimizer (doc 29) | Docs and code only, **never** in a listing |

### 2. Description: the first 160 characters decide the click
1. **Developer contract:** Variables, constraints, objective (JSON) → optimal plan, shadow prices, infeasibility explanation.
2. **Business value (for the champion):** 3–15% cost or capacity gains vs manual plans.
3. **Methods (specialist search and credibility):** uses simplex, MIP, CP-SAT.
4. **Trust:** stateless; data is processed in the request and never stored; per-user OAuth sign-in. Infeasible problems return the conflicting constraints.

### 3. Keywords
- **Developer search terms (title, description, tags):** linear programming, mixed integer programming, optimization solver, scheduling optimization, resource allocation, or-tools, constraint programming
- **Business phrases (tagline, FAQ, tool descriptions):** "best plan within limits", "allocate resources"
- **Method terms (description body):** simplex, MIP, CP-SAT
- **Platform terms (only where actually listed):** MCP server, Copilot Studio connector, Agentforce action, Breeze agent tool, Gemini Enterprise, Amazon Bedrock AgentCore, Claude connector, ChatGPT app

### 4. Tools as shown in the listing (alphabetical)
`explain_solution` · `optimize_blend` · `optimize_production_mix` · `optimize_schedule` · `optimize_transportation` · `solve_custom_lp`

**Tool-description rule:** the AI picks tools from these descriptions, so they count as discoverability too.
- Start with a verb and the business noun.
- Name the input fields.
- Say what comes back.
- Say when **not** to use the tool.

### 5. Example prompts (listing, demo video, tool descriptions)
- "Find the best production mix within capacity"
- "Allocate budget across projects"
- "Schedule jobs on machines"

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
- Sample JSON payload shaped like a real export from: any structured data from the caller
- One copy-paste example call **and** its response
- A 60–90 second demo video: prompt → tool call → answer
- Link to the docs and the MCP/OpenAPI schema; privacy policy and terms

### 8. Champion kit (for the developer to forward upward)
- **One-line business case:** 3–15% cost or capacity gains vs manual plans. Always as a range, never a bare number.
- **Security note:** stateless, nothing stored, per-user OAuth, DPDP/GDPR-friendly
- **Price:** the tier table from §Monetization, with the free tier stated first

### 9. Don'ts
- Coined names (e.g. "Constraint Optimizer") in titles or tags
- "Best", "#1" or "guaranteed" claims
- Bare money figures
- Promises beyond the guardrail: Infeasible problems return the conflicting constraints
