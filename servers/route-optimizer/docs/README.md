# Route & Logistics Optimizer

[SAIFS AI](https://saifs.ai) (`saifs.ai`) publishes this as a remote MCP server under the registry name `ai.saifs/route-optimizer`. You send today’s stops, the vehicles, and the distances, and it returns who goes where, in what order.

People search for this as route optimization, the vehicle routing problem, VRP, a delivery route planner, TSP, fleet routing, or last mile. The questions are “plan delivery routes” and “optimize van routes.” The short install page is the [server README](../README.md). This page is what a route is, why a language model cannot order forty stops, and how to send a distance you measured.

| | |
|---|---|
| **Registry name** | `ai.saifs/route-optimizer` |
| **Endpoint** | `https://route-optimizer-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## What it is

A fleet, a field team, or a sales territory asks the same question every morning: who goes where, in what order, in which vehicle. The server answers it from the stops and the limits you send.

`optimize_routes` orders many vehicles. Time windows, capacities, and a maximum shift length are constraints, not suggestions. `optimize_single_route` is the one-person version, a traveling-salesman order of the stops. `assign_jobs` decides which worker takes which job. `pack_loads` decides what fits in the vehicle. `reoptimize_midday` rebuilds the rest of the day from where people are now, plus orders that just landed. `route_map` draws a route you already have. `explain_route` shows the cost, which limits bound the plan, and the saving against visiting the stops in the order they arrived.

Distances and travel times are yours. The server does not look up a road network and does not invent a kilometre. A plan is only as good as the matrix you sent.

Against a route built by hand, or by taking the stops in sheet order, this class of plan is associated with about 10–30% less fuel and drive time. That is a range across fleets, not a promise for one morning. The figure in `explain_route` is the comparison on your matrix.

A production mix, a blend, or a shift roster with no stops belongs on [Optimization Solver (LP / MIP Scheduling)](../../optimization-solver/README.md). How many units to reorder belongs on [Inventory Intelligence & Optimizer](../../inventory-intelligence-optimizer/README.md).

## Why a whiteboard order burns the day

The traveling salesman problem, formalized in the 1930s, is the shortest tour of a list of stops. The vehicle routing problem, stated by Dantzig and Ramser in 1959, is that tour split across a fleet, with capacities. Add a delivery window and a shift length and the search is still the same family. Forty stops with several vans is not something a language model can order reliably. The number of sequences grows too fast, and a plausible-looking list is not a checked one.

What the solver needs from you is the matrix. Stop A to stop B takes the minutes you send. The search then honors the windows, the capacity, and the shift. Kuhn’s Hungarian method (1955) does a different job: given a cost for every worker–job pair, it assigns each job once so the total cost is the minimum. That assignment is exact for the cost table. It does not by itself put the jobs in driving order.

A large day may return the best route found within the solve, rather than a tour proved shortest. The reply says when that is the case. A small assignment does not need that label. It is the minimum of the costs you sent.

## How to use it

Connect the endpoint in the [install README](../README.md) and sign in when the client asks. Send stops, vehicles, and the travel times between them.

```json
{
  "vehicles": [
    {"id": "van-1", "capacity": 40, "max_shift_minutes": 480}
  ],
  "stops": [
    {"id": "depot", "kind": "depot"},
    {"id": "s1", "demand": 4, "window": ["09:00", "11:00"]},
    {"id": "s2", "demand": 6, "window": ["11:00", "13:00"]}
  ],
  "travel_minutes": [
    {"from": "depot", "to": "s1", "minutes": 20},
    {"from": "s1", "to": "s2", "minutes": 25},
    {"from": "s2", "to": "depot", "minutes": 30}
  ]
}
```

What comes back is an ordered list per vehicle, the distance and time added up from your matrix, and any stop that could not be placed. A stop with no travel time to the others is returned for a fix. It is not dropped into the tour at a guessed minute. A coordinate you mark as low confidence is flagged the same way. A bad pin in the matrix becomes a bad kilometre in the plan.

A dispatch reading of a larger file looks like this: six vans, each with an ordered list, delivery windows held, and fewer kilometres than the order the sheet arrived in. One illustration of that sentence is about 18% fewer kilometres. Your stops produce their own comparison in `explain_route`.

### Several vehicles

`optimize_routes` is the morning plan. Each vehicle gets a sequence. A window that cannot be met with the fleet and the minutes you sent is named. The reply does not quietly break the window. Capacity is in the units you send, cases, kilograms, or stops. A maximum shift is a limit on the minutes in your matrix, plus any service time you include on the stop.

Lock a stop to a vehicle, or lock the stops a driver has already finished, when the plan has to stay practical. The solver keeps those locks.

### One person

`optimize_single_route` orders one vehicle or one sales rep. Use it when there is no fleet to split. The same rule applies: travel times come from you.

### Who does the job

`assign_jobs` takes workers, jobs, and a cost for each pair. Skill is a cost or a ban you send. A pair you mark forbidden is not assigned. The result minimizes the total of the costs in the table. It does not invent a skill, and it does not sequence the day’s driving. Assign first, then route each person’s jobs, or send the fleet straight to `optimize_routes` when the split and the order should be decided together.

### What fits

`pack_loads` fills vehicles by the capacity you send. A small list can be packed exactly. A large list uses a fast decreasing fit, and the reply says so. The classic bound on that fit is about 11/9 as many bins as a perfect pack. Packing says what is on the vehicle. It does not say the stop order.

### Midday

`reoptimize_midday` starts from the state you send: where each vehicle is, which stops are already done, and the new orders. It does not track drivers and it does not poll. Finished stops stay finished. Locks stay locked. The new plan covers the remainder.

### The map and the explanation

`route_map` draws a route you have already solved. It is a picture of that sequence, not a turn-by-turn navigation app, and not a live traffic feed.

`explain_route` splits the cost, names the constraints that bound the plan (a window, a capacity, a shift), and compares the plan with the naive order, the stops in the order you listed them. The saving is that comparison on your matrix.

### Prompts

- “Plan routes for 4 vans and 60 deliveries.”
- “Optimize my sales rep’s visits.”
- “Assign drivers to orders.”
- “Here are today’s 40 stops and 6 vans. Build routes that honour delivery windows.”
- “Assign these field jobs to technicians by skill and travel cost, then route each person.”
- “Three new orders just landed. Re-optimise from where the drivers are now.”

## Tools

| Tool | What you send | What comes back | Do not use it when |
|---|---|---|---|
| `assign_jobs` | Workers, jobs, and a cost for each allowed pair | One job per worker, total cost minimized for that table | You need the driving order. This call assigns. It does not sequence stops |
| `explain_route` | A route you just built | Cost split, which constraints bind, and the saving versus the stops in listed order | You have no route yet, or you want a saving that ignores the matrix you sent |
| `optimize_routes` | Stops, vehicles, travel times you supply, and limits (windows, capacity, shift) | An ordered route per vehicle, with totals from your matrix, and any stop that would not fit | There is only one person. Use `optimize_single_route`. Also skip it when you have not supplied the travel times |
| `optimize_single_route` | One vehicle or rep, stops, and travel times | One ordered tour | Several vehicles must split the work. Use `optimize_routes` |
| `pack_loads` | Items and vehicle capacities | What goes in which vehicle | You need the stop order. Packing is not a route |
| `reoptimize_midday` | Where each vehicle is, stops already done, locks, and the new orders, with travel times | A route for the rest of the day | You are building the morning plan from scratch. Use `optimize_routes` |
| `route_map` | A finished route | A map of that sequence | You want live navigation or a road-time lookup. Distances stay caller-supplied |

## How to read a result

- **The minutes are yours.** Totals are sums of the travel times and service times you sent. A wrong pin shows up as a wrong total.
- **A window, a capacity, or a shift either holds or is named.** An unmet limit is a stop left unplaced, with the reason, not a route that breaks it.
- **Best-found is a label, not a failure.** On a large fleet the reply says when the search stopped at the best plan found within the solve. The Hungarian assignment does not use that label. It is the minimum of the cost table.
- **The saving is against the naive order.** `explain_route` compares your plan with the stops in the order listed. The 10–30% range is the general case against hand-built routes. It is not a field the server pastes onto every file.
- **Locks are honored.** A finished stop and a vehicle you pinned stay where you put them.
- **A flagged address waits for a person.** Low-confidence coordinates come back for a manual fix. The next call uses the corrected matrix.

## Data and what this will not do

A delivery address is personal data. Send a stop id and the travel times when that is enough, and send a name or a phone only when the route needs them. The server processes the request and does not store it. It does not follow vehicles between calls. Each user signs in with OAuth. Your own duties under India’s Digital Personal Data Protection Act, the GDPR, or any other law that applies stay with you.

The server will not invent a distance, will not fetch live traffic, and will not present a best-found route as a proved shortest tour unless the solve says so. It will not drop a locked stop. It will not treat the map as navigation.

Privacy policy: https://saifs.ai/privacy_policy

## Plans

| Tier | What it covers |
|---|---|
| **Free** | 1 vehicle, up to 20 stops a day |
| **Pro** | ₹4,100 / $49 per month — 5 vehicles, 200 stops a day, and time windows |
| **Business** | ₹16,500 / $199 per month — 25 vehicles, midday re-routing, load packing, and route maps |
| **Fleet** | Per vehicle beyond the Business cap, priced for that fleet |

The free tier is enough to order one vehicle’s day, with no sales call. Current terms are on [saifs.ai](https://saifs.ai).

## Support

- Install and client setup: [server README](../README.md)
- A plan that is not a road route (production, blending, a roster): [Optimization Solver (LP / MIP Scheduling)](../../optimization-solver/README.md)
- How many units to reorder: [Inventory Intelligence & Optimizer](../../inventory-intelligence-optimizer/README.md)
- Website: https://saifs.ai
- Registry: https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
