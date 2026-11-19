<!-- DRAFT for review. Do not replace intro.md until this file is accepted.
FIELD BLOCK (Microsoft validated fields). Confirm before submission.

Title (27 characters): Route & Logistics Optimizer
Description (435 characters):
Helps dispatch teams decide who goes where, in what order, and in which vehicle. Send stops and vehicles, or a road distance matrix. Returns each vehicle's ordered stops with arrival and leave times, distance, load, and stops that cannot be served. The same stops, vehicles, seed, and settings return the same plan. It recommends a plan. It does not dispatch drivers or message customers. Large files can run in your own agent sandbox.

Operation summary (36 characters): Vehicle routes assignments and loads
Operation description:
Plans ordered vehicle routes, a worker-to-job assignment, and a load plan from stops and vehicles. It does not dispatch drivers.
-->

# Route & Logistics Optimizer - Saifs AI

**Route & Logistics Optimizer - Saifs AI** helps dispatch and field teams decide **who goes where, in what order, and in which vehicle**. You send stops and vehicles, and a road distance matrix when you have one. It returns each vehicle's ordered stops with arrival and leave times, distance, load, map links, and GeoJSON, plus stops that cannot be served, savings versus the current plan, a worker-to-job assignment, and a load plan that uses as few vehicles as the sizes allow.

It is built for fleets that still route from a whiteboard, a spreadsheet, or driver habit, and who need a plan they can rerun this morning.

| If you are | Start with |
| --- | --- |
| A dispatch or fleet lead | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [Where the data comes from](#where-the-data-comes-from) |
| An administrator or security reviewer | [Data handling and security](#data-handling-and-security), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [For developers](#for-developers) |
| An operations reviewer checking the method | [Inside the calculation](#inside-the-calculation) |

## The problem it solves

Most fleets still sequence stops by habit, by the order on the sheet, or by driver instinct. A weak sequence burns time. A missed window loses the delivery. The same sheet can produce a different story each time someone asks an agent to "just plan the route".

Route & Logistics Optimizer makes that plan available to any AI agent:

- **Who goes where**: an ordered stop list for each vehicle.
- **When they arrive**: arrival and leave times from travel time plus service time.
- **What does not fit**: stops that cannot be served, with the reason.
- **Whether it is shorter than today**: savings versus the routes you already run, versus list order, and versus a simple baseline.
- **Who does which job**: a worker-to-job assignment from the costs, skills, and limits you supply.
- **How the goods fit**: a load plan that uses as few vehicles as weight and volume allow.
- **What changes at mid-day**: a new plan from each vehicle's current position when new stops arrive.

## Why choose it: the same plan every time

Route & Logistics Optimizer is deterministic for a fixed search. The same stops, vehicles, seed, and settings produce the same stop order, the same arrival and leave times, the same unserved stops, and the same savings versus the plan you already run. Results come from routing, assignment, and packing methods. They are not written afresh by a language model each time you ask.

That matters because dispatch acts on a sequence and a clock time. They need a plan they can rerun. The agent's wording around the plan may vary. The stop order does not, when the seed and settings stay the same.

| What it does | Established method |
| --- | --- |
| Plans many stops across vehicles, with time windows, capacity, and shift limits | Vehicle routing |
| Orders the visits for one vehicle | A single-route tour |
| Assigns workers to jobs at the lowest total cost you supply | Assignment |
| Loads items into as few vehicles as the sizes allow | Bin packing |
| Compares the new plan with the routes you already run | Savings against `current_routes`, list order, and a simple baseline |
| Re-plans from where each vehicle is now | The same routing model, from current position and the time now |

**It does not guess a stop into the plan.** Stops that do not fit a window, a skill, a shift, or a capacity come back as unserved, with a reason.

**It recommends and does not act.** It never dispatches a driver, never messages a customer, and never writes to a transport system, an ERP, or Dataverse. The search is time-boxed. You receive the plan found within `time_limit_seconds` (default 20, maximum 45). That is not a promise that every large instance is a proven shortest tour, and it is not a promised fuel saving.

## Who should use this

| Role | How it helps |
| --- | --- |
| **Dispatch managers** in last-mile and courier fleets | A morning plan: which vehicle, which order, which windows, and which stops do not fit |
| **Field-service coordinators** | Assign jobs by the costs and skills you supply, then route each technician |
| **Distributor fleet managers** | Respect capacity and shift limits, and pack the load before the vans leave |
| **Sales-territory managers** | One representative, many visits, in a shorter order than the list on the sheet |
| **D2C brands with their own delivery** | Daily routes from the order sheet, and a mid-day re-plan when new orders land |
| **Small fleet owners** | A few vehicles and a spreadsheet of stops |
| **Agent makers and developers** | Add a tested routing plan to an agent |

**Best fit:** last-mile and courier fleets, field-service companies, distributors with their own vehicles, sales-territory teams, and D2C brands that deliver their own orders.

**Not required:** an enterprise transport suite. A stop file with coordinates, or a road distance matrix, is enough to start.

## Use cases

Each use case below can be built as an agent. The agent types are listed in [Agents you can build](#agents-you-can-build).

**1. Morning multi-vehicle dispatch**
Scenario: a dispatcher has today's drops, a depot, and several vans with capacity and shift times.
Outcome: one ordered route per vehicle, with arrival and leave times.
Ask: "Who goes where this morning, and in which vehicle?"

**2. One driver, many visits**
Scenario: a single van or a sales representative has a sheet of stops and no fleet to split.
Outcome: a visit order for that one route. One vehicle is assumed when you do not send a fleet.
Ask: "Order these stops for one driver."

**3. Windows, shifts, and locked stops**
Scenario: some drops must fall inside a window, one stop must stay on a named vehicle, and the shift has a hard end.
Outcome: those constraints are honoured when you supply them. Stops that still cannot be served come back with a reason.
Ask: "Plan the routes and tell me which stops do not fit."

**4. Savings versus the plan you already run**
Scenario: drivers already have a sequence, or the sheet is in list order.
Outcome: pass `current_routes`. The result shows the distance, time, or cost difference versus that plan, versus list order, and versus a simple baseline.
Ask: "How does this plan compare with the routes we already run?"

**5. Mid-day re-plan**
Scenario: new orders arrive after the vans have left.
Outcome: send each vehicle's current position and the time now, plus the new stops. The plan is rebuilt from where the fleet actually is.
Ask: "Re-plan from where the vehicles are now."

**6. Assign jobs to workers**
Scenario: technicians and jobs have skills, and each person can take only so many jobs.
Outcome: an assignment from the costs you supply. It does not judge skill beyond those inputs.
Ask: "Assign these jobs to workers who have the right skills."

**7. Pack the load**
Scenario: goods must fit weight and volume, in as few vehicles as the sizes allow.
Outcome: a load plan against `bin_capacity` and, when you send it, `bin_volume`.
Ask: "How few vehicles does this load need?"

**8. A road matrix instead of straight-line distance**
Scenario: you have road distances and travel times from your own router.
Outcome: pass `distance_matrix`, `time_matrix`, and `matrix_ids`. The plan uses those figures. If you omit them, travel is estimated and the result says so.
Ask: "Route these stops with this road distance matrix."

**9. A large stop file**
Scenario: more than about 300 stops.
Outcome: the same engine runs in the agent's sandbox. Stop data stays there.
Ask: "Plan this large stop file in the sandbox."

## Agents you can build

Each agent is one agent in your agent host with this connector added as a tool. The last column shows what else to add.

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Morning dispatch agent | Answers who goes where, in what order, and in which vehicle | Stops, vehicles, and a depot | 1, 3 |
| Single-route agent | Orders one driver's or one representative's visits | A stop list. One vehicle is assumed if none is sent | 2 |
| Savings agent | Compares the new plan with the routes already being run | `current_routes` | 4 |
| Mid-day agent | Rebuilds the plan from current vehicle positions | Current coordinates and the time now | 5 |
| Assignment agent | Matches workers to jobs from costs, skills, and a job cap | `workers`, `jobs`, and optional `assignment_costs` | 6 |
| Load agent | Packs items into as few vehicles as weight and volume allow | Item sizes and bin capacity | 7 |
| Large-file agent | Runs the local engine above about 300 stops | Code execution on the agent host | 9 |
| Order-system agent | Reads stops from the order or transport system, calculates the plan, and hands it back for a person to release | That system's own MCP server. Pass coordinates or matrix ids, not names or phone numbers | 1, 5 |
| Scheduled morning agent | Sends the day's plan without anyone asking | A flow with a recurrence trigger, and email or Teams | 1 |

The tools run on request. Scheduling comes from the flow, not from the connector. A person still releases the work.

## At a glance

| Item | Detail |
| --- | --- |
| Tools | `route_optimizer` (hosted calculation) and `route_optimizer_get_engine` (local engine) |
| Minimum input | Stops with coordinates, or ids that match a distance matrix. One vehicle is assumed when no fleet is sent |
| Hosted tool size | About 300 stops, about 100 vehicles, about 300 workers or jobs, and about 2,000 load items |
| Search limit | `time_limit_seconds` default 20, maximum 45. `seed` default 0 |
| Larger files | Local engine, runs in your agent's code sandbox, needs only `numpy` |
| Writes to your systems | None |
| Pricing | [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment) |

<!-- TODO Saifs AI: confirm this marketplace URL is the pricing page for this offer. -->

## Where the data comes from

Route & Logistics Optimizer does not connect to your systems, and it does not call a live traffic service. The agent brings stops, vehicles, and an optional matrix. The connector needs only rows. It has not been certified against each system named below.

| Data source | How the agent gets the data | Best route |
| --- | --- | --- |
| Stop spreadsheet or order export from a courier, field-service, or store system | The user uploads the file | Local engine above about 300 stops, hosted tool below that |
| A road distance and time matrix from your own router | The user or agent passes `distance_matrix`, `time_matrix`, and `matrix_ids` | Either route |
| A transport or ERP system that has its own MCP server | The agent reads stop coordinates and vehicles, then passes the rows | Same size rule |
| Dataverse tables | The Dataverse MCP server or the Dataverse connector | Same size rule |
| A scheduled flow | A flow reads today's orders and calls this connector | Hosted tool |

**Works alongside your transport system's MCP server.** That server gives the agent the orders and the fleet. This connector adds the route, the assignment, and the load plan. A dispatcher still releases the work.

**What to pull from the source**

| Data in your system | Field it fills | Why it matters |
| --- | --- | --- |
| Stop id and coordinates | `stops`: `id`, `lat`, `lng` | Required, unless the id exists in `matrix_ids` |
| Demand and volume | `demand`, `volume` | Capacity |
| Service and window | `service_minutes`, `window_start`, `window_end` | Arrival feasibility. Service defaults to 5 minutes |
| Skills and a locked vehicle | `skills`, `vehicle` | Constraints. Applied only when you supply them |
| Fleet | `vehicles`: capacity, shift, start and end, skills, max stops, max km, costs | Who can serve the stops. Default is one vehicle |
| Depot | `depot` | Start and, by default, return |
| Existing sequence | `current_routes` | Savings versus the plan you already run |
| Workers and jobs | `workers`, `jobs`, `assignment_costs` | The assignment layer |
| Load lines | `items`, `bin_capacity`, `bin_volume` | The packing layer |

Send coordinates or matrix ids. Do not send customer names, phone numbers, or street addresses. Coordinates can still be personal data. Use only data you are authorised to process.

**File formats for the local engine.** The script reads stop files and optional vehicle, item, worker, and job files. It writes `route_plan.json` and `route_plan.geojson`. Pass depot, time limit, and other settings in `params`.

## How it works

The connector offers two routes to the same calculation. The agent picks the route by the size of the data.

| | Hosted tool | Local engine |
| --- | --- | --- |
| Tool name | `route_optimizer` | `route_optimizer_get_engine` |
| Use when | About 300 stops or fewer | More than about 300 stops, or a file already in the sandbox |
| What the agent sends | The rows and settings | File paths and settings only |
| What comes back | The full plan | A ready-to-run Python script |
| Where the calculation runs | Saifs AI endpoint | Your agent's code-execution sandbox |
| Where stop data goes | To the Saifs AI endpoint for calculation | Nowhere. It stays in the sandbox |
| Requires | Nothing extra | An agent host with code execution, and `numpy` |

**Local engine, step by step**

1. The user uploads or points the agent to a stop file.
2. The agent calls `route_optimizer_get_engine` with the file path and settings.
3. The connector returns the calculation script.
4. The agent runs the script unchanged in its sandbox.
5. The script prints the decision cards and a line per vehicle, and writes `route_plan.json` and `route_plan.geojson`.
6. The agent reports the plan. It does not dispatch.

### Inside the connector

1. The user asks who goes where, in what order, and in which vehicle.
2. The agent chooses a tool and calls the connector, which passes the call to the Saifs AI MCP server over HTTPS at `https://route-optimizer-mcp-microsoft.saifs.ai/mcp`.
3. The server checks the signed-in account.
4. The hosted tool runs the engine and returns the plan. The engine tool returns the same engine as a script.
5. The agent explains the plan in plain language. The stop order and times come from the engine. The wording comes from the agent.

### Inside the calculation

Both routes run the same engine.

| Step | What the engine does | Method |
| --- | --- | --- |
| 1. Read stops and vehicles | Uses coordinates or matrix ids. Assumes one vehicle when none is sent | Supplied rows |
| 2. Travel time | Uses your road matrix when you send one. Otherwise estimates from straight-line distance times `road_factor` (default 1.3) at `speed_kmh` (default 25). The result says which source it used. This is not live traffic | Matrix, or straight-line estimate |
| 3. Search | Improves the routes until `time_limit_seconds` (default 20, maximum 45) or the iteration cap. `seed` defaults to 0. The same seed and settings repeat | Time-boxed routing search |
| 4. Constraints | Honours windows, weight, volume, shift start and end, max stops, max km, skills, and locked vehicles when you supply them. `day_start` defaults to 08:00 and `day_end` to 20:00. `default_service_minutes` is 5. `return_to_depot` defaults to true. `minimize_vehicles` defaults to true | Constraint routing |
| 5. Objective | Minimises `distance` (default), `time`, or `cost`. Cost needs `cost_per_km` or `cost_per_hour` on the request or the vehicle | Stated objective |
| 6. Unserved stops | Returns stops that do not fit, with the reason | Feasibility check |
| 7. Savings | Compares with `current_routes`, with list order, and with a simple baseline | Difference versus a stated baseline |
| 8. Assignment | If workers and jobs are sent, assigns at the costs you supply, with skills and `max_jobs` | Assignment |
| 9. Load | If items and a bin capacity are sent, packs into as few vehicles as the sizes allow | Bin packing |
| 10. Map output | Returns map links and GeoJSON when `include_geojson` is true (default) | Geometry for the host to draw |

**Why the answer does not change.** The same stops, vehicles, seed, and settings return the same plan. Changing the time limit or the seed can change which plan is found inside the limit. The agent's wording may vary.

**What happens when data is missing.** A missing matrix is estimated, and the result says so. A missing fleet becomes one vehicle. Windows, capacity, skills, and costs apply only when supplied. Currency defaults to INR and is a display label. There is no conversion between currencies. `distance_unit` defaults to km. `time_unit` defaults to `auto`.

**Where the working is shown.** Unserved stops carry a reason. The savings block names the baseline. The plan states whether distance was estimated.

## Data handling and security

| Question | Local engine | Hosted tool |
| --- | --- | --- |
| Does stop data reach Saifs AI? | No | Yes, the rows passed in the call |
| Where is it processed? | Your agent host's code sandbox | `route-optimizer-mcp-microsoft.saifs.ai`, over HTTPS |
| What happens to the data after the call? | It never left your sandbox | <!-- TODO Saifs AI: confirm retention for this server. --> |
| What does Saifs AI record? | Only that the engine script was requested, if your facts sheet confirms that | <!-- TODO Saifs AI: confirm the audit and billing log. --> |

**What "sandbox" means.** The sandbox is the code-execution environment of your own agent host. It belongs to your tenant, not to Saifs AI. Your host's data policies continue to apply.

**Use the local engine only.** If stop coordinates must never leave your environment, turn off the hosted tool. In Copilot Studio, open the agent's **Tools** tab, select the Route & Logistics Optimizer server, turn off **Allow all**, and switch off `route_optimizer`. Leave `route_optimizer_get_engine` on. This setting is per agent.

**Sign-in.** Users sign in with Microsoft single sign-on (OAuth 2.0). No API key is created, shared, or stored.

**No write access.** The connector does not dispatch drivers, message customers, or write to a transport system, an ERP, or Dataverse.

## Security and compliance

This section separates what an independent party has reviewed from what Saifs AI states about itself.

**Reviewed by Microsoft**

| Item | What it covers |
| --- | --- |
| Microsoft connector certification | Microsoft reviewed and tested this connector before listing it as a certified connector. The review covers the connector's function and packaging, not Saifs AI's internal systems. |

**Self-assessed by Saifs AI**

Self-assessments are statements Saifs AI makes about its own practices. They are published so that you can check them, and they are not independent audits.

<!-- PUBLISH EACH ROW ONLY AFTER IT IS COMPLETED. Until then, keep the row inside this comment.

| Self-assessment | What it is | Where to read it |
| --- | --- | --- |
| Microsoft Publisher Attestation | Saifs AI's answers to Microsoft's questionnaire. Microsoft publishes the answers and does not verify them. Renewed every year. | [link] |
| CSA STAR Level 1 | Saifs AI's completed CAIQ on the public STAR Registry. Updated every year. | [link] |
| CSA STAR for AI Level 1 | Saifs AI's completed AI-CAIQ on the same registry. | [link] |

-->

| Design fact | Detail |
| --- | --- |
| Customer data stored | <!-- TODO Saifs AI: confirm hosted-tool retention. The local engine keeps rows in your sandbox. --> |
| Passwords held | None. Sign-in is Microsoft single sign-on. |
| Write access to your systems | None |
| Live traffic | None. A missing matrix is a straight-line estimate, and the result says so |
| Local engine | Plain Python, one dependency, no network calls, open to review |

**Independent audits.** Saifs AI does not yet hold an independently audited certificate such as SOC 2 or ISO 27001. If your review needs a completed security questionnaire, request it through the [contact page](https://saifs.ai/contact-us).

## Token usage and cost

With the hosted tool, token use grows with every stop, because the model sends the stops out and reads the routes back. With the local engine, the stops stay in the sandbox, so that row cost does not grow with the file.

| Stop file | Hosted tool: tokens and cost per run | Local engine: first run | Local engine: each later run | Comparison |
| --- | --- | --- | --- | --- |
| 25 stops, 3 vehicles | about 2,600 tokens, about $0.02 | Script not measured | about 90 tokens, under $0.01 | Hosted tool. One small call |
| 300 stops, 20 vehicles | about 29,000 tokens, about $0.19 | Script not measured | about 90 tokens, under $0.01 | Hosted tool is still within the size cap |
| 2,000 stops, 50 vehicles | Not the hosted route. Sending the stops alone is about 114,000 tokens | Script not measured | about 90 tokens, under $0.01 | Local engine only |

**Estimate your own file**

- **Hosted tool, to send:** about 56 tokens per stop and about 31 per vehicle on this sample. A depot object is extra.
- **Hosted tool, to read:** about 40 tokens per stop in the returned route, including arrival, leave, and coordinates. GeoJSON in a live result can be longer.
- **Local engine, later run:** about 90 tokens for a short decision print.

**Which route to choose**

- **A few dozen stops:** the hosted tool. Twenty-five stops on this sample are about $0.02.
- **Up to about 300 stops:** the hosted tool is still the allowed route. Three hundred stops and 20 vehicles are about $0.19 on this sample.
- **More than about 300 stops:** the local engine. It is the only route.
- **Coordinates must stay in your environment:** the local engine, with the hosted tool switched off.

**Hosts that bill per action.** Some agent hosts charge a fixed amount for each tool call and do not bill by token. On those hosts the measured token saving does not change the bill. Choose the local engine for file size and for keeping coordinates in your environment.

How these figures were measured: token counts use the `o200k_base` tokenizer on generated sample data, with rows written as JSON, on 16 November 2026. Costs use $1.25 per million input tokens and $10 per million output tokens. Stops the model sends are output tokens. The route it reads is input tokens. Route records are representative objects from the documented fields, not a captured engine file. The local-engine script is not in this folder, so the first local run has no dollar figure. Do not copy the Inventory script cost onto this connector.

## Prerequisites

- A Saifs AI account, created by signing in with Microsoft single sign-on
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Access to Copilot Studio, Microsoft 365 Copilot, Azure AI Foundry, Power Automate, Power Apps, or Azure Logic Apps
- Stops with coordinates, or ids that match a distance matrix
- For the local engine: code execution enabled on the agent host

## How to get credentials

No keys or secrets are needed. When you create a connection, you are redirected to sign in with your Microsoft account and approve access. If the connection later expires, sign in again from the connection page.

## Get started with your connector

1. In Copilot Studio, open your agent and go to **Tools**, then **Add a tool**.
2. Search for **Route & Logistics Optimizer** and select it.
3. Create a connection and sign in.
4. Add this to the agent's instructions: "For any question about routes, stop order, vehicle assignment, or loads, call the Route & Logistics Optimizer tool. Do not invent a route yourself. Do not send customer names, phone numbers, or street addresses."
5. Provide stops with coordinates, or a distance matrix. Include vehicles, windows, and the current plan when you have them.
6. Ask in plain language, for example "Who goes where this morning, and in which vehicle?"

Treat results as decision support. A dispatcher still releases the work. If distance was estimated, say so.

## For administrators

Enabling this connector gives the organisation one approved way to decide who goes where, in what order, and in which vehicle.

**Why this is an administrator decision.** Without a calculation tool, makers ask an agent to plan today's routes from a spreadsheet. A language model can give a different sequence on each run, and the result cannot be compared morning to morning. A governed connector uses one method, and it names stops that do not fit.

- **You choose who can connect it.** Tenant and environment permissions apply.
- **You keep your system of record.** The connector recommends a plan. It does not dispatch drivers or write back to any system.
- **You can check the sequence.** The same stops, vehicles, seed, and settings can be rerun. Unserved stops are named, with a reason.
- **You keep identity out of the request.** Coordinates or matrix ids. No names, phones, or street addresses.
- **You control where data goes.** Turn off the hosted tool per agent to keep coordinates in your environment.
- **You set data policy as usual.** Place the connector in the Business, Non-business, or Blocked group.
- **There are no secrets to rotate.** Expired connections are renewed by signing in again.

**Review checklist**

- Decide whether the hosted tool is allowed, or local engine only
- Confirm stop files will not contain names, phones, or street addresses
- Decide whether a road matrix is required, or whether a labelled straight-line estimate is acceptable
- Assign the connector to the correct data policy group
- Review the privacy policy and terms
- Read the self-assessments under Security and compliance, once they are published
- Review the local engine script if your policy requires it

## For developers

Stops are the main input. No field list is marked required in the schema. A useful call sends stop ids and coordinates, or matrix ids.

### Stop and vehicle fields

| Field | Meaning | Needed for |
| --- | --- | --- |
| `stops` | `id`, `lat`, `lng`, or an id in `matrix_ids`. Optional demand, volume, service, window, skills, locked `vehicle`, priority, status | The route. Up to 300 on the hosted tool |
| `vehicles` | Capacity, volume, shift, start and end, skills, max stops, max km, costs, `count` | A multi-vehicle plan. Up to 100. Default is one vehicle |
| `depot` | `{lat, lng}` or a matrix id | Where routes start. Return is on by default |
| `distance_matrix`, `time_matrix`, `matrix_ids` | Road kilometres and minutes | Replaces the straight-line estimate |
| `current_routes` | `{vehicle, stops: [ids in order]}` | Savings versus today's plan |
| `workers`, `jobs`, `assignment_costs` | Assignment inputs. Up to 300 workers and 300 jobs | Who does which job |
| `items`, `bin_capacity`, `bin_volume` | Load lines. Up to 2,000 items | How few vehicles the goods need |

### Options and defaults

| Option | Default | Purpose |
| --- | --- | --- |
| `objective` | `distance` | Also `time` or `cost` |
| `minimize_vehicles` | true | Use as few vehicles as possible, then shorten |
| `return_to_depot` | true | Routes return to the depot |
| `road_factor` | 1.3 | Straight-line multiplier when no matrix is sent. Range 1 to 3 |
| `speed_kmh` | 25 | Speed for that estimate. Range 1 to 200 |
| `day_start` | 08:00 | Default day start |
| `day_end` | 20:00 | Default day end |
| `default_service_minutes` | 5 | Service time when a stop has none |
| `time_limit_seconds` | 20 | Search limit. Maximum 45 |
| `seed` | 0 | Keep it fixed when you need the same plan again |
| `include_geojson` | true | Include GeoJSON |
| `distance_unit` | `km` | Display unit |
| `time_unit` | `auto` | Also `minutes` or `hours` |
| `currency` | `INR` | Display label only. No conversion |

### Sample request

```json
{
  "depot": { "lat": 19.076, "lng": 72.877 },
  "stops": [
    { "id": "S1", "lat": 19.089, "lng": 72.865, "demand": 2, "window_start": "09:00", "window_end": "12:00" },
    { "id": "S2", "lat": 19.055, "lng": 72.900, "demand": 1, "window_start": "10:00", "window_end": "14:00" }
  ],
  "vehicles": [
    { "id": "V1", "capacity": 10, "shift_start": "08:00", "shift_end": "17:00" }
  ],
  "time_limit_seconds": 20,
  "seed": 0
}
```

### Sample response (shortened)

<!-- TODO Saifs AI: replace this note with a shortened response from a real call. Do not publish an invented stop order as if it were measured. -->

A live shortened response is not included in this draft. Capture one call before you publish the page. The sample should show ordered stops, times, and any unserved stop with its reason.

### Response sections

| Section | Use it for |
| --- | --- |
| Routes | Ordered stops per vehicle, with arrival and leave times, distance, and load |
| Unserved stops | What did not fit, and why |
| Savings | Difference versus the current plan, list order, and a simple baseline |
| Assignment | Worker-to-job matching, when workers and jobs were sent |
| Load plan | How items pack, when items and a bin capacity were sent |
| Map output | Map links and GeoJSON, when included |
| Distance source | Whether a matrix was used or distance was estimated |

## Known issues and limitations

- **Decision support only.** It does not dispatch drivers, message customers, or write to any system.
- **Not a proven shortest tour.** The search stops at the time limit. The same seed and settings repeat. A different limit can return a different plan.
- **Not live traffic.** Without a matrix, distance is straight-line times 1.3 at 25 km/h, and the result says so.
- **Coordinates or matrix ids only.** Do not send names, phones, or street addresses. Coordinates can still be personal data.
- **Constraints apply only when supplied.** One vehicle is assumed when no fleet is sent.
- **Hosted tool size.** About 300 stops, 100 vehicles, 300 workers or jobs, and 2,000 load items.
- **Assignment** uses the costs, skills, and limits you send. It does not judge skill beyond those inputs.
- **Currency.** Display label only. Default INR. No conversion between currencies.
- **Regions.** Not available in US Government (GCC, GCC High, DoD) or China (21Vianet) clouds.

<!-- TODO Saifs AI: confirm the excluded-cloud sentence for this offer. -->

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| The tool cannot run, or asks you to sign in again | The connection has expired | Open the connection and sign in again |
| Travel looks like a straight line | No road matrix was sent | Pass a distance and time matrix, or tell the user the estimate was used |
| A stop is unserved | It does not fit a window, skill, shift, or capacity | Read the reason. Widen the constraint or add a vehicle |
| The plan changed between two runs | The seed or the time limit changed | Keep `seed` and `time_limit_seconds` the same |
| The agent invents a route | Instructions do not require the tool | Add the instruction in Get started |
| The local engine does not run | Code execution is not enabled | Enable it, or use the hosted tool under 300 stops |
| Not permitted, or a subscribe link | The account cannot call the tool | Paste the full Marketplace link exactly as returned |

## FAQ

**Do I need a specialist to use this?**
No. A dispatcher asks in plain language. The plan runs inside the tool. A person still releases the vehicles.

**What is the least data I need?**
Stop ids and coordinates, or ids that match a matrix. One vehicle is assumed if you send no fleet.

**Does my data leave my environment?**
Not with the local engine. With the hosted tool, the rows in the call are sent for calculation. You can turn the hosted tool off.

**Will it change anything in my systems?**
No. It does not dispatch, message customers, or write to a transport system or Dataverse.

**Our transport system already has an MCP server. Why add this?**
That server gives the agent the orders and the fleet. This connector adds the route, the assignment, and the load plan.

**Why do I get the same answer every time?**
The same stops, vehicles, seed, and settings return the same plan. The search is time-boxed, so a different time limit can change the plan. The agent's wording may vary.

**What happens if my data is incomplete?**
Missing windows and capacities are not invented as constraints. A missing matrix is estimated, and the result says so. Stops that do not fit are returned, not dropped in silence.

**Can I check how a result was produced?**
Yes. Unserved stops carry a reason, savings name the baseline, and the plan states whether distance was estimated.

**How large a file can it handle?**
About 300 stops on the hosted tool. Larger files use the local engine.

**Can it run on a schedule?**
Yes, from a scheduled flow. The connector itself runs on request. It does not dispatch.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment). Microsoft Premium connector licensing applies separately.

## Support

**Route & Logistics Optimizer - Saifs AI** is published by Saifs AI.

- Website: [https://saifs.ai](https://saifs.ai)
- Learn more: [https://saifs.ai/route-logistics-optimizer](https://saifs.ai/route-logistics-optimizer)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Privacy: [https://saifs.ai/privacy_policy/route-&-logistics-optimizer](https://saifs.ai/privacy_policy/route-&-logistics-optimizer)
- Terms: [https://saifs.ai/eula](https://saifs.ai/eula)
- Sign-in: Microsoft single sign-on (OAuth 2.0). No API keys.
- MCP endpoint: `https://route-optimizer-mcp-microsoft.saifs.ai/mcp`

<!-- OPEN QUESTIONS
1. Confirm the marketplace offer URL.
2. Confirm hosted-tool retention and the audit log.
3. Capture a shortened live response, including an unserved stop if the sample has one.
4. Hosted send and read tokens are measured on generated JSON. The local-engine script is still not tokenized. A live route file, including GeoJSON, would replace the representative routes.
5. Confirm excluded clouds.
6. The live tool text mentions a benchmark gap to best-known instances. That figure is not on this page, because it is not a named-customer result. Confirm if a measured, publishable benchmark exists.
7. Publisher Attestation and STAR rows stay commented until completed and linked.
-->
