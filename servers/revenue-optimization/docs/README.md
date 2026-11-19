# 30 · Revenue Optimizer — Yield Management & Dynamic Pricing MCP
**Score: 78/100 · IW-Breadth: 55 · Phase: NEAR (high-value, underserved)**
**Classical models: Littlewood's Rule (1972) · EMSR-a/b (Belobaba, 1987/89) · Price elasticity estimation · Overbooking models (Beckmann, 1958)**

## Problem statement
Any business selling perishable capacity — hotel rooms, flight seats, event tickets, rental equipment, ad slots, appointment/clinic slots, restaurant covers, coworking desks — faces the same question airlines answered decades ago: how much capacity to hold back for higher-paying late buyers vs. sell now, and how to price dynamically as the deadline approaches. Revenue management math (the science that makes airlines profitable) has existed since the 1970s but lives in expensive RM systems only big hotels/airlines run — everyone else prices statically and leaves 5–20% of revenue uncaptured. An LLM can't run the continuous optimization or hold the demand/booking-curve history.

## Solution
Yield-management math over the customer's booking, capacity, and price data — for any perishable-capacity business.

**Tool set (v1):**
- `optimize_price(product, remaining_capacity, days_to_deadline, demand_curve)` → recommended price now
- `protection_levels(fare_classes, demand_forecast)` → how much capacity to protect for higher-value segments (Littlewood/EMSR)
- `estimate_elasticity(price_history, sales_history)` → how demand responds to price
- `recommend_overbooking(no_show_rate, capacity, costs)` → safe overbooking level
- `watch_booking_pace(product)` → alert when pace runs ahead/behind, triggering price moves
- `explain_recommendation(product)` → the RM logic + assumptions

## THE ALGORITHMS (implementation guide)
### Littlewood's Rule (1972) — two-class
Protect capacity for high-fare class up to the point where `P(demand_high > protection) × fare_high = fare_low`. Sell low-fare until that marginal condition flips.
### EMSR-b (Belobaba 1989) — multi-class
Expected Marginal Seat Revenue heuristic: nested protection levels across many fare classes. The airline-standard method, generalizes to any multi-segment capacity.
### Price elasticity
`E = (%Δ quantity)/(%Δ price)` estimated via log-log regression on price/sales history → optimal price where marginal revenue is maximized.
### Overbooking (Beckmann 1958)
Balance expected spoilage (empty perishable capacity) vs. denied-service cost given no-show distribution → optimal overbooking limit. Reference: revenue-management texts (Talluri-van Ryzin). Math is mostly `scipy` + our encoding.

## Pressure test
G1 (26/30): interpreter runs one calc ⚠️; continuous booking-pace watching + protection-level updates + elasticity tracking is the product. Native ✅ safe (RM systems are enterprise-hotel/airline only; everyone else prices static — you serve them). G2 (19/25): moats 4 (booking-curve/demand history) + 3 (continuous pace watching + price moves) + 5 (explained). G3 (20/25): clear ROI (revenue uplift measurable), broad perishable-capacity audience, self-serve. G4 (14/20): demand history compounds. **Total 78.**

## Audience & user profiles
Hotels/B&Bs, event organizers, equipment/vehicle rental, coworking, parking, ad-inventory sellers, clinics (slot-based), restaurants, tour operators (pairs with your travel data).
1. **Boutique hotel owner** — agent: "raise this weekend's rate 12% — booking pace is 30% ahead of the demand curve; hold 4 rooms for late corporate bookings."
2. **Event organizer** — dynamic ticket-tier pricing as the date approaches.
3. **Equipment rental** — price + overbooking given no-show history.

## Where/how to get data & components
| Need | Source | Access |
|---|---|---|
| Booking/capacity/price history | PMS/booking-system exports, CSV | none for CSV; connectors later |
| Demand forecast | pairs with doc 18 | internal |
| Math | `scipy`, `statsmodels` + encoded RM formulas | we build + validate |

## Monetization
- **Free**: single-product price recommendation
- **Pro ₹4,100/$49 mo**: protection levels, elasticity, ≤10 products
- **Business ₹16,500/$199 mo**: multi-class EMSR, overbooking, booking-pace watchers, PMS pulls
- **Enterprise**: multi-property/large inventory. Positioning: "the pricing science airlines use — measurable revenue uplift — for your rooms/seats/slots."

## India sole-prop notes
Booking data may include personal data → DPDP consent. Large Indian hospitality/events/rental market; pairs naturally with your travel/data moat. Export invoicing under LUT.

## Risks
Elasticity estimation needs price variation in history (guide customers to enable it; refuse confident answers on flat-price history); over-aggressive pricing damages customer relationships (recommend bands + explain, don't auto-execute); demand-forecast dependency (bundle doc 18).

---

## MODULE: Overbooking Optimizer (added — not a standalone product)
**Classical models: Beckmann (1958) · Rothstein (1971) · binomial show-up model · links to Littlewood's rule above**
**Standalone initial moat would be only 66/100** (the core math is simple enough for a chatbot to approximate), so it ships as a module of this MCP, where it becomes defensible once per-segment show-rate history exists.

**Problem:** airlines, hotels, buses, clinics and event venues lose revenue to no-shows. Selling a few extra seats/slots recovers it — selling too many creates denied-service costs and angry customers.

**Tools:**
- `optimal_overbooking(capacity, show_rate_or_history, fare, denied_cost)` → booking limit maximising expected profit
- `denied_service_risk(booking_limit)` → probability and expected number of customers turned away
- `show_rate_estimate(segment, history)` → show-up rate by route/day/segment with confidence

**Algorithm:** bookings b; shows S ~ Binomial(b, q). Expected profit `E[fare · min(S, C) − denied_cost · max(S − C, 0)]`. Increase b while the marginal expected gain is positive. Show rate q estimated per segment (Beta-Binomial shrinkage for thin segments).

**Guardrails:** always report the probability of denying service alongside the profit gain; for clinics, cap overbooking and never apply it to urgent or critical appointment types.

---

---

## LISTING & DISCOVERABILITY
*Keep this section current. It drives marketplace search, AI tool selection and the buying decision.*

**Who discovers this MCP:** agent developers, developers and agent builders (Copilot Studio makers, Agentforce, Breeze and Gemini Enterprise builders, Claude/ChatGPT connector users). They find it, test it, and **champion the purchase** to the budget owner. Every field below is written for them first, with a business tagline the manager can understand.

### 1. Names
| Field | Use this | Rule |
|---|---|---|
| **Listing title** | **Dynamic Pricing & Overbooking — Saifs AI** | Capability keyword first; ≤ 60 characters before "— Saifs AI" |
| **Tagline** (for the budget owner) | Set prices and booking limits by demand to fill capacity profitably | Plain business words; ≤ 90 characters |
| **Server / connector ID** | `saifs-revenue-optimization` | Lowercase, hyphenated, no coined words |
| **Internal code name** | Revenue Optimizer (doc 30) | Docs and code only, **never** in a listing |

### 2. Description: the first 160 characters decide the click
1. **Developer contract:** Demand, capacity, fares (JSON) → price/booking limits, overbooking level, expected revenue.
2. **Business value (for the champion):** Revenue up 3–8% (revenue-management literature).
3. **Methods (specialist search and credibility):** uses Littlewood, EMSR, binomial overbooking.
4. **Trust:** stateless; data is processed in the request and never stored; per-user OAuth sign-in. Ranges; denied-service probability shown.

### 3. Keywords
- **Developer search terms (title, description, tags):** dynamic pricing, revenue management, overbooking, yield management, emsr, price optimization
- **Business phrases (tagline, FAQ, tool descriptions):** "what price to charge", "how much to overbook"
- **Method terms (description body):** Littlewood, EMSR, binomial overbooking
- **Platform terms (only where actually listed):** MCP server, Copilot Studio connector, Agentforce action, Breeze agent tool, Gemini Enterprise, Amazon Bedrock AgentCore, Claude connector, ChatGPT app

### 4. Tools as shown in the listing (alphabetical)
`denied_service_risk` · `estimate_elasticity` · `explain_recommendation` · `optimal_overbooking` · `optimize_price` · `protection_levels` · `recommend_overbooking` · `show_rate_estimate` · `watch_booking_pace`

**Tool-description rule:** the AI picks tools from these descriptions, so they count as discoverability too.
- Start with a verb and the business noun.
- Name the input fields.
- Say what comes back.
- Say when **not** to use the tool.

### 5. Example prompts (listing, demo video, tool descriptions)
- "How many rooms should we overbook?"
- "Set fare buckets for next week"
- "What price maximizes revenue?"

### 6. Categories per marketplace
| Marketplace | Category / tags |
|---|---|
| Microsoft (Copilot Studio certified connector / AppSource) | Travel & Hospitality; Data |
| Salesforce AgentExchange | Service |
| AWS Marketplace: AI Agents & Tools | Industry → Travel & hospitality |
| Google Cloud Marketplace / Gemini Agent Gallery | Travel |
| HubSpot Marketplace (Breeze) | Service |

### 7. Developer try-it kit (ship with the listing)
- Free tier that works in under 10 minutes, no sales call
- Sample JSON payload shaped like a real export from: PMS, booking engine exports
- One copy-paste example call **and** its response
- A 60–90 second demo video: prompt → tool call → answer
- Link to the docs and the MCP/OpenAPI schema; privacy policy and terms

### 8. Champion kit (for the developer to forward upward)
- **One-line business case:** Revenue up 3–8% (revenue-management literature). Always as a range, never a bare number.
- **Security note:** stateless, nothing stored, per-user OAuth, DPDP/GDPR-friendly
- **Price:** the tier table from §Monetization, with the free tier stated first

### 9. Don'ts
- Coined names (e.g. "Revenue Optimizer") in titles or tags
- "Best", "#1" or "guaranteed" claims
- Bare money figures
- Promises beyond the guardrail: Ranges; denied-service probability shown
