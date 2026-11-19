# 28 · Route & Logistics Optimizer — VRP/TSP MCP
**Score: 80/100 · IW-Breadth: 58 · Phase: STRONG NEW BUILD (extends the travel/logistics moat)**
**Classical models: Traveling Salesman (1930s formalization) · Vehicle Routing Problem (Dantzig-Ramser, 1959) · Assignment Problem (Kuhn's Hungarian method, 1955) · Bin Packing**

## Problem statement
Every delivery fleet, field-service team, and sales-territory operation answers "who goes where, in what order, in which vehicle?" daily — almost always by hand or driver instinct. Suboptimal routing burns 10–30% excess fuel/time. The math (VRP with time windows, capacities, multi-vehicle) has been solved-in-principle since 1959 and industrial-grade OSS solvers exist, but the tooling lives in enterprise TMS suites SMB fleets never buy. An LLM cannot solve a 40-stop VRP reliably (NP-hard, needs a real solver) and cannot re-optimize daily as orders land.

## Solution
Daily-optimized routing over the customer's stops/fleet data, agent-delivered.

**Tool set (v1):**
- `optimize_routes(stops[], vehicles[], constraints)` → per-vehicle ordered routes (time windows, capacities, max shift length)
- `optimize_single_route(stops[])` → TSP for one vehicle/rep
- `assign_jobs(workers[], jobs[], costs)` → Hungarian-method optimal assignment
- `pack_loads(items[], vehicle_capacities)` → bin-packing load plans
- `reoptimize_midday(current_state, new_orders)` → dynamic re-route
- `route_map(route_id)` → branded route map (reuses Map Foundry doc 15)
- `explain_route(route_id)` → cost breakdown, constraint bindings, savings vs. naive order

## THE ALGORITHMS (implementation guide)
### VRP/TSP via OR-Tools (the industrial answer)
Do NOT hand-roll: Google **OR-Tools routing library** (OSS, Apache-2.0) is the production-grade solver — supports time windows (VRPTW), capacities (CVRP), pickup-delivery, multiple depots. Model: distance/time matrix (from OSRM self-host, doc 15 stack) → `RoutingModel` with dimension constraints → search with guided local search metaheuristic + time limit.
### Assignment (Kuhn 1955)
Hungarian algorithm: optimal worker→job assignment minimizing total cost, O(n³). `scipy.optimize.linear_sum_assignment`.
### Bin packing
First-fit-decreasing heuristic (fast, ~11/9 OPT bound) or OR-Tools CP-SAT for exact small instances.
**Engineering notes:** the distance-matrix pipeline (geocode → OSRM matrix) is the real infrastructure; cache matrices; solver time limits with best-found-so-far returns; idempotent job semantics (TECH-00 §4) for big instances.

## Pressure test
G1 (28/30): NP-hard optimization + daily re-solve + live distance data — fully beyond runtime interpreters. Native ✅ safe (enterprise TMS is the un-bought competition; you fight the whiteboard). G2 (20/25): moats 7 (solver infrastructure) + 3 (daily/dynamic re-optimization) + 6 (your live transport data enriches ETAs) + 5 (explained savings). G3 (19/25): clear fleet/field-service buyer, self-serve CSV-first, visible ROI (fuel/time saved = easy sell). G4 (13/20). **Total 80.**

## Audience & user profiles
Last-mile delivery fleets, field-service companies, distributors with own vehicles, sales-territory managers, courier SMBs.
1. **Distributor dispatch manager** — morning agent output: 6 vehicles' optimized routes honoring delivery windows, ~18% fewer km.
2. **Field-service coordinator** — technicians assigned + routed by skill and geography.
3. **D2C brand with own delivery** — daily routes from the order sheet CSV.

## Where/how to get data & components
| Need | Source | Access |
|---|---|---|
| Stops/orders | CSV/upload, e-comm/OMS exports | none for CSV |
| Distance/time matrix | Self-hosted OSRM (free, doc 15 stack) or Mapbox Matrix API | server / API key |
| Solver | Google OR-Tools (OSS) | pip |
| Maps output | Doc 15 Map Foundry | internal |

## Monetization
- **Free**: 1 vehicle, ≤20 stops/day
- **Pro ₹4,100/$49 mo**: 5 vehicles, 200 stops/day, time windows
- **Business ₹16,500/$199 mo**: 25 vehicles, dynamic re-routing, load packing, route maps, watchers
- **Fleet/Enterprise**: per-vehicle pricing beyond. ROI-framed sales page ("pays for itself in fuel within days").

## India sole-prop notes
Delivery addresses = personal data → DPDP consent + retention policy. India last-mile market is enormous and largely un-optimized (huge home wedge); INR via Razorpay. Map compliance per doc 15's India border rules. Export invoicing under LUT.

## Risks
Bad address/geocode quality in India (validation + confidence flags, manual-fix loop); solver runtime on big instances (time-boxed best-found + async jobs); driver adoption (routes must be practical — allow locks/manual overrides).

---

---

## LISTING & DISCOVERABILITY
*Keep this section current. It drives marketplace search, AI tool selection and the buying decision.*

**Who discovers this MCP:** agent developers, developers and agent builders (Copilot Studio makers, Agentforce, Breeze and Gemini Enterprise builders, Claude/ChatGPT connector users). They find it, test it, and **champion the purchase** to the budget owner. Every field below is written for them first, with a business tagline the manager can understand.

### 1. Names
| Field | Use this | Rule |
|---|---|---|
| **Listing title** | **Route Optimization (VRP) — Saifs AI** | Capability keyword first; ≤ 60 characters before "— Saifs AI" |
| **Tagline** (for the budget owner) | Plan delivery and visit routes that cut distance and time | Plain business words; ≤ 90 characters |
| **Server / connector ID** | `saifs-route-optimization` | Lowercase, hyphenated, no coined words |
| **Internal code name** | Route & Logistics Optimizer (doc 28) | Docs and code only, **never** in a listing |

### 2. Description: the first 160 characters decide the click
1. **Developer contract:** Stops, vehicles, time windows (JSON) → optimized routes, sequence, distance and time.
2. **Business value (for the champion):** Fuel and drive time cut 10–30%.
3. **Methods (specialist search and credibility):** uses VRP, TSP, Hungarian.
4. **Trust:** stateless; data is processed in the request and never stored; per-user OAuth sign-in. Uses caller-supplied distances.

### 3. Keywords
- **Developer search terms (title, description, tags):** route optimization, vehicle routing problem, vrp, delivery route planner, tsp, fleet routing, last mile
- **Business phrases (tagline, FAQ, tool descriptions):** "plan delivery routes", "optimize van routes"
- **Method terms (description body):** VRP, TSP, Hungarian
- **Platform terms (only where actually listed):** MCP server, Copilot Studio connector, Agentforce action, Breeze agent tool, Gemini Enterprise, Amazon Bedrock AgentCore, Claude connector, ChatGPT app

### 4. Tools as shown in the listing (alphabetical)
`assign_jobs` · `explain_route` · `optimize_routes` · `optimize_single_route` · `pack_loads` · `reoptimize_midday` · `route_map`

**Tool-description rule:** the AI picks tools from these descriptions, so they count as discoverability too.
- Start with a verb and the business noun.
- Name the input fields.
- Say what comes back.
- Say when **not** to use the tool.

### 5. Example prompts (listing, demo video, tool descriptions)
- "Plan routes for 4 vans and 60 deliveries"
- "Optimize my sales rep's visits"
- "Assign drivers to orders"

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
- Sample JSON payload shaped like a real export from: caller-supplied addresses/distances, TMS
- One copy-paste example call **and** its response
- A 60–90 second demo video: prompt → tool call → answer
- Link to the docs and the MCP/OpenAPI schema; privacy policy and terms

### 8. Champion kit (for the developer to forward upward)
- **One-line business case:** Fuel and drive time cut 10–30%. Always as a range, never a bare number.
- **Security note:** stateless, nothing stored, per-user OAuth, DPDP/GDPR-friendly
- **Price:** the tier table from §Monetization, with the free tier stated first

### 9. Don'ts
- Coined names (e.g. "Route & Logistics Optimizer") in titles or tags
- "Best", "#1" or "guaranteed" claims
- Bare money figures
- Promises beyond the guardrail: Uses caller-supplied distances
