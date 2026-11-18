# Dynamic Pricing & Overbooking

[SAIFS AI](https://saifs.ai) (`saifs.ai`) publishes this as a remote MCP server under the registry name `ai.saifs/revenue-optimization`. It recommends a price, how much capacity to hold for higher-paying demand, and how far to overbook, for anything that expires unsold: a room, a seat, a ticket, a rental day, an ad slot, a desk, a cover, or an appointment.

People search for this as dynamic pricing, revenue management, overbooking, yield management, EMSR, or price optimization. The questions are “what price to charge” and “how much to overbook.” The short install page is the [server README](../README.md). This page is what a protection level means, why a static rate leaves capacity unsold or underpriced, and how to send a booking curve.

| | |
|---|---|
| **Registry name** | `ai.saifs/revenue-optimization` |
| **Endpoint** | `https://revenue-optimizer-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## What it is

You send remaining capacity, days to the deadline, the prices you have charged, and how bookings have arrived. You get back a price band, nested protection levels, an elasticity when the history can support one, and an overbooking limit that is always printed next to the probability of turning someone away.

Nine tools cover that. The price is not written back to the property system. The operator posts it.

| Tool | What you ask |
|---|---|
| `optimize_price` | What should we charge now, given what is left and how long until it expires? |
| `protection_levels` | How many units do we hold for the higher fare? |
| `estimate_elasticity` | How did quantity move when price moved? |
| `show_rate_estimate` | What share of bookings actually show, by segment, with a confidence band? |
| `optimal_overbooking` | What booking limit maximises expected profit at that show rate? |
| `recommend_overbooking` | What is the safer limit once denial cost and a cap are included? |
| `denied_service_risk` | If we accept this many bookings, how often do we turn someone away, and how many? |
| `watch_booking_pace` | Is today’s pace ahead of or behind the curve we stored? |
| `explain_recommendation` | Which rule, which assumption, and what happens if we do not take it? |

The call is stateless. You keep the booking curve and send it again. Nothing is stored. Literature on revenue management puts the revenue difference versus a static price at about 3–8%. That is a range from that literature, not a promise for one property.

The demand curve this server prices against can be the forecast from [Demand Forecasting](../../demand-forecasting/README.md). A quote for a negotiated B2B deal, rather than a perishable unit, is [Quote Price Optimization](../../quote-pricing/README.md).

## Why a fixed rate misses both sides

A room, a seat, or a slot that is empty at the deadline earns nothing. A unit sold early at the low rate is a unit you cannot sell later to someone who would have paid more. Littlewood (1972) and Belobaba’s EMSR (1987/89) are the rules airlines and hotels have used for that tradeoff. Beckmann (1958) is the overbooking balance between an empty unit and the cost of denying someone who did show.

Most properties still post one rate and take every booking until the house is full. A language model can evaluate one of these formulas from a prompt. It will not refit when you send tonight’s pace, refuse an elasticity when the price never moved, and refuse to hide the chance that an overbooked clinic turns a patient away.

## How to use it

Connect the endpoint in the [install README](../README.md) and sign in when the client asks. Export capacity, prices, and bookings from the PMS or the booking sheet. Send counts and fares. Do not send guest names.

```json
{
  "product": "deluxe-weekend",
  "capacity": 40,
  "remaining": 18,
  "days_to_deadline": 5,
  "fare_classes": [
    {"code": "corporate", "fare": 9000},
    {"code": "advance", "fare": 6500}
  ],
  "pace": {"booked_to_date": 22, "curve_to_date": 17}
}
```

A hotel reading in that shape is: pace is ahead of the stored curve, so the current rate moves up, on the order of 12% in an illustration of that size, and a few rooms stay protected for the late corporate fare. Your curve produces its own band. `explain_recommendation` names the pace gap, the protection, and the assumption. You change the rate in the PMS.

### Price now

`optimize_price` uses remaining capacity, days left, and the demand curve. The reply is a band, not a single rate to post blindly. Inside the band, the point is the price the marginal calculation prefers today. `watch_booking_pace` compares bookings so far with the curve you stored. Ahead of the curve, the next price call sits higher. Behind it, the band sits lower. The watcher does not change the rate. It tells you the pace moved.

### How much to hold back

For two fares, Littlewood protects high-fare demand up to the largest y where

```text
P(demand_high > y) × fare_high = fare_low
```

Sell the low fare while that inequality still favours it. Stop when the expected value of holding the unit for the high fare equals the low fare in hand. EMSR-b nests more than two classes: higher fares are pooled into one equivalent demand, and Littlewood is applied down the ladder. `protection_levels` returns those nested limits. A fare bucket for next week is that list.

### Elasticity

```text
E = (% change in quantity) / (% change in price)
```

is fit from a log-log regression on a history where price actually varied. If the price was flat, there is nothing to estimate. The tool says so and does not return a confident elasticity. When |E| is at or below 1, raising price does not have a finite revenue maximum in that fit, and the reply says the history does not support a revenue-maximizing price. `estimate_elasticity` is an input to the price band. It is not itself a rate change.

### Overbooking

Shows S out of b accepted bookings are modelled as binomial with show probability q.

```text
E = fare · min(S, capacity) − denied_cost · max(S − capacity, 0)
```

`optimal_overbooking` raises the booking limit while the next acceptance still adds expected profit. `recommend_overbooking` is the safer limit: the same costs, plus a cap you set. `show_rate_estimate` gives q by route, day, or segment. A thin segment borrows strength (a Beta-Binomial shrinkage) and the band on q is wide. `denied_service_risk` is mandatory reading beside either limit: the probability that shows exceed capacity, and the expected number of people turned away.

For a clinic, set a hard cap and do not overbook an urgent or critical appointment type. The tools will not recommend overbooking those types. A denied appointment is not the same cost as a denied leisure seat, and the reply keeps the probability visible so you can reject the profit number.

### Prompts

- “How many rooms should we overbook?”
- “Set fare buckets for next week.”
- “What price should we charge for the remaining rooms?”
- “Booking pace is ahead of the curve. What does that do to this weekend’s rate, and how many rooms do we hold for the late fare?”
- “At a booking limit of 44 on a house of 40, what is the chance we turn someone away?”

## Tools

| Tool | What you send | What comes back | Do not use it when |
|---|---|---|---|
| `denied_service_risk` | A booking limit, capacity, and a show rate | The probability shows exceed capacity, and the expected number turned away | You want the limit itself. This is the risk at a limit you name |
| `estimate_elasticity` | Price history and sales history | Elasticity, and whether a revenue-maximizing price exists in that fit | Price barely moved. The tool refuses a confident number |
| `explain_recommendation` | A product and the recommendation just made | The rule, the pace or the protection, and the assumptions | You have not run a price, protection, or overbooking call |
| `optimal_overbooking` | Capacity, show rate or history, fare, and the cost of a denial | The booking limit that maximises expected profit, with the denial risk beside it | The inventory is an urgent or critical clinic appointment. Those types are not overbooked |
| `optimize_price` | Product, remaining capacity, days to the deadline, and a demand curve | A price band for now | You want the rate written into the PMS. You post it. A flat price history cannot support the elasticity inside this call |
| `protection_levels` | Fare classes and a demand forecast | Nested units to protect for higher fares (Littlewood for two classes, EMSR-b for more) | You have one fare. There is nothing to protect |
| `recommend_overbooking` | No-show rate, capacity, denial cost, and any cap | A safer booking limit than the uncapped profit maximum | You have not looked at `denied_service_risk` for that limit, or the slot is urgent or critical |
| `show_rate_estimate` | A segment and show/no-show history | Show probability and a confidence band. Thin segments are shrunk toward the wider pool and labelled | You want a booking limit. This is only q |
| `watch_booking_pace` | Product, bookings to date, and the curve you stored | Ahead, behind, or on pace | You expect the server to watch overnight. Send the latest counts. Pace does not change the live rate by itself |

## How to read a result

- **A price band is the recommendation.** The point inside it is the rate the calculation prefers today. You decide what to post. An aggressive step that the explanation cannot justify is a reason to stay nearer the current rate.
- **Protection is unsold on purpose.** Those units are held for the higher fare. If the high fare does not arrive, they can still be released later. The level is not a block you forget.
- **Pace ahead** supports a higher band. **Pace behind** supports a lower one. The curve you stored is the reference. A curve from a different season will mis-state the gap.
- **No elasticity means no confident price response.** Flat history is a refusal, not a zero.
- **Every overbooking limit comes with a denial probability and an expected count.** A higher expected profit that hides that probability is not a complete result. For a clinic, a cap and a ban on urgent types outrank the profit.
- **The operator posts the rate and the booking limit.** Nothing in the reply sells a room or moves a patient.

## Data and what this will not do

Booking extracts can contain personal data. Send counts, fares, and dates. The server processes the request and does not store it. Each user signs in with OAuth. Your own duties under India’s Digital Personal Data Protection Act, the GDPR, or any other law that applies to the export stay with you.

The server will not post a rate, will not overbook an urgent or critical appointment, will not hide the chance of turning a customer away, and will not invent an elasticity from a price that never changed. It will not watch the PMS unless you send the next pace.

Privacy policy: https://saifs.ai/privacy_policy

## Plans

| Tier | What it covers |
|---|---|
| **Free** | A price recommendation for one product |
| **Pro** | ₹4,100 / $49 per month — protection levels, elasticity, and up to 10 products |
| **Business** | ₹16,500 / $199 per month — multi-class EMSR, overbooking with denial risk, booking-pace checks, and PMS export pulls |
| **Enterprise** | More than one property, or a large inventory, on a private offer |

The free tier is enough to price one product from a capacity and a demand curve, with no sales call. Current terms are on [saifs.ai](https://saifs.ai).

## Support

- Install and client setup: [server README](../README.md)
- The demand curve and a forecast of how many units will be requested: [Demand Forecasting](../../demand-forecasting/README.md)
- A negotiated B2B quote, rather than a perishable rate: [Quote Price Optimization](../../quote-pricing/README.md)
- Website: https://saifs.ai
- Registry: https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
