# Optimization Solver - Saifs AI

**Optimization Solver - Saifs AI** helps operations teams decide **the best plan within the limits** they already have. You send the tables you have. It returns the plan, the bottleneck, what one more unit of that limit is worth, and how the plan compares with today.

It is built for manufacturers, logistics teams, workforce planners, and finance leads who must allocate a scarce resource.

**In plain language:** you give it products, a recipe, routes, shifts, or projects, plus the limits. It returns the plan, and it names the bottleneck.

**For technical readers:** the calculation is linear programming or mixed-integer programming. Whole units use branch and bound. The value of one more unit of a bottleneck is a shadow price.

**Publisher:** Saifs AI
**Website:** [https://saifs.ai](https://saifs.ai)
**Learn more:** [https://saifs.ai/optimization-solver](https://saifs.ai/optimization-solver)
**Pricing:** [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
**Contact and help:** [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
**Privacy:** [https://saifs.ai/privacy_policy/optimization-solver](https://saifs.ai/privacy_policy/optimization-solver)
**Terms:** [https://saifs.ai/eula](https://saifs.ai/eula)
**Sign-in:** Microsoft single sign-on (OAuth 2.0). No API keys.
**MCP endpoint:** [https://optimization-solver-mcp-microsoft.saifs.ai/mcp](https://optimization-solver-mcp-microsoft.saifs.ai/mcp)

| If you are | Start with |
| --- | --- |
| A business owner, operations or planning lead | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [Where the data comes from](#where-the-data-comes-from) |
| An administrator or security reviewer | [Data handling and security](#data-handling-and-security), [Security and compliance](#security-and-compliance), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [Where the data comes from](#where-the-data-comes-from), [For developers](#for-developers) |
| A planner checking the method | [Inside the calculation](#inside-the-calculation) |

## The problem it solves

Most teams still try quantities until the limits stop breaking. A spreadsheet can test a few mixes. It cannot prove which plan is optimal, and it rarely says what the bottleneck is worth. The same gap shows up in feed blends, shipping plans, shift cover, and budget choices. A language model can write the problem down. It cannot solve a linear program exactly.

Optimization Solver makes that calculation available to any AI agent:

- **What to make:** a production mix that respects machine hours, material, and demand.
- **What to put in the recipe:** the least-cost blend that still meets every spec.
- **How to ship:** the cheapest plan from sources to customers.
- **Who works when:** shift cover, a named roster, or jobs loaded on machines.
- **What to fund:** resource allocation across projects, inside one budget or several.
- **What the limit is worth:** the bottleneck, and what one more unit of it is worth.
- **What to relax:** when no plan exists, which limits conflict, and the smallest change.

## Why choose it: the same plan every time

Optimization Solver returns the best plan within the limits from a solver, not from a fresh guess. The same tables and the same settings return the same plan. Purchasing, operations, and finance act on that plan. They need a result they can rerun and compare with last week. The agent's wording around the numbers may vary. The numbers do not.

A whole-number search can stop early. It then returns the best plan found so far and the remaining gap. It does not fill the gap with a guessed quantity.

| What it does | Established method |
| --- | --- |
| Chooses quantities under resource limits | Linear programming |
| Forces whole units, people, or yes/no choices | Mixed-integer programming, solved by branch and bound |
| Builds the cheapest blend that meets a spec | Linear programming on a blending model |
| Builds the cheapest shipments from sources to customers | A transportation model, solved as a linear program |
| States what one more unit of a bottleneck is worth, and the range that value holds over | Shadow price |
| States what a price or cost must become before the plan changes | Sensitivity of the plan to that coefficient |
| When no plan exists, names the conflicting limits and the smallest change | Infeasibility analysis |

**It does not guess.** Missing inputs are reported. They are not replaced with a made-up plan. When the limits cannot all be met, the result names the conflict and the smallest change. It does not invent a feasible plan.

**It recommends and does not act.** It never changes a production order, a recipe, a shipment, a roster, or a budget in your systems. A person approves the plan.

## Who should use this

| Role | How it helps |
| --- | --- |
| **Production planners** at manufacturers | A production mix from profit, machine hours, and material, with the bottleneck named |
| **Feed, fuel, and alloy formulators** in agriculture and process plants | A least-cost blend that meets protein, energy, or other specs |
| **Logistics and distribution planners** | A shipping plan from depots or plants to customers, at minimum transport cost |
| **Workforce planners** | Shift cover at minimum cost, or a named roster with skills, hours, and days off |
| **Plant schedulers** | Jobs placed on machines, with late jobs called out |
| **Finance leads** | Resource allocation across projects, inside a business budget. This is not personal financial advice. |
| **Operations and supply-chain managers** | One method for mix, blend, shipping, staffing, and budget choices |
| **Agent makers and developers** | A tested linear programming and mixed-integer tool, so nobody has to ship their own solver |

**Best fit:** manufacturers, agriculture and feed producers, logistics teams, workforce planners running scheduling optimization, and finance teams that allocate a business budget from tables they already export.

**Not required:** a planning module, an operations-research team, or a pre-written model. A table of limits is enough to start.

## Use cases

Each use case below can be built as an agent. The agent types are listed in [Agents you can build](#agents-you-can-build).

**1. Production mix**
Scenario: a plant can sell two or more products, and machine hours or material will run out first.
Outcome: how many of each product to make, the profit, and which resource is the bottleneck.
Ask: "Find the best production mix within capacity."

**2. Least-cost blend**
Scenario: a feed mill, a fuel blender, or an alloy plant must hit a spec at the lowest ingredient cost.
Outcome: the recipe, the cost, and which spec is binding.
Ask: "Give me the least-cost feed blend that meets the protein spec."

**3. Shipping plan**
Scenario: several depots can supply several customers, and each route has a cost.
Outcome: how much to ship on each route, and the total transport cost.
Ask: "What is the cheapest way to ship from these depots to these customers?"

**4. Shift coverage**
Scenario: each day needs a minimum crew, and shifts cover different days.
Outcome: how many people to put on each shift so cover is met at the lowest cost.
Ask: "Cover next week's shifts at the lowest cost."

**5. Named roster**
Scenario: people have skills, hour limits, and days they cannot work.
Outcome: a roster a supervisor can review, with unfilled places called out when cover is short.
Ask: "Build a roster for named staff, with skills and days off."

**6. Jobs on machines**
Scenario: each job has a duration, and only some machines can run it.
Outcome: which machine runs which job, and which jobs finish late.
Ask: "Schedule these jobs on the machines."

**7. Budget allocation**
Scenario: more projects exist than the budget, the headcount, or the quarter can hold.
Outcome: which projects to fund, including groups and dependencies, and which budget binds.
Ask: "Allocate the budget across these projects."

**8. Compare with today's plan**
Scenario: the team already runs a mix, a set of routes, or a roster.
Outcome: the gain of the optimal plan over that current plan, when you pass it in.
Ask: "How much better is the optimal plan than what we do today?"

**9. Large models and MPS files**
Scenario: the model is past the hosted caps, or it already exists as an MPS file.
Outcome: the same plan, from the local engine, with the tables staying in the sandbox.
Ask: "Solve this MPS file in the sandbox."

## Agents you can build

Each agent is one agent in your agent host with this connector added as a tool. The last column shows what else to add.

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Production planning agent | Returns quantities, profit, and the bottleneck resource | A product and resource file, or a data source from [Where the data comes from](#where-the-data-comes-from) | 1 |
| Blend formulation agent | Returns the least-cost recipe that meets each spec | An ingredient file and a spec file | 2 |
| Shipping plan agent | Returns route quantities and total transport cost | Source, destination, and route-cost files | 3 |
| Workforce schedule agent | Covers shifts, or builds a named roster with skills and days off | A shift file and, for a named roster, a staff file | 4, 5 |
| Machine loading agent | Assigns jobs to machines and flags late jobs | A job file and a machine list | 6 |
| Budget allocation agent | Chooses projects inside one or more budgets | A project file and the budget figures | 7 |
| Plan review agent | Compares the optimal plan with the plan in use today | Today's quantities in `current_plan` | 8 |
| System-of-record planning agent | Reads items, capacity, routes, or projects from the system of record, then returns a plan for a person to approve | That system's own MCP server | 1, 3, 7 |
| Scheduled replan agent | Runs the plan daily or weekly without anyone asking | A Power Automate flow with a recurrence trigger, and email or Teams if the plan should be sent | 1, 4 |
| Large-model agent | Runs the local engine on a big table or an MPS file | Code execution on the agent host | 9 |

The tools run on request. Scheduling comes from the flow, not from the connector.

## At a glance

| Item | Detail |
| --- | --- |
| Tools | `optimization_solver` (hosted calculation) and `optimization_solver_get_engine` (local engine) |
| Templates | `production_mix`, `blend`, `transportation`, `schedule`, `allocation`, `custom`. Leave `problem_type` out and it is detected from the tables. |
| Minimum input | Nothing is required up front. Send the tables you have. The result reports what was missing. |
| Hosted tool size | About 3,000 variables or constraints or fewer, and a built model up to 5,000 variables. Row caps include 500 products, 200 resources, 1,000 ingredients, 2,000 destinations, 300 staff, 500 jobs, 2,000 projects, and 3,000 variables. Time limit 60 seconds. |
| Larger files | Local engine, runs in your agent's code sandbox, needs only `numpy` |
| Writes to your systems | None |
| Local result file | `optimization_result.json` |

## Where the data comes from

Optimization Solver does not connect to your systems. The agent brings the tables to it, from any source that can supply costs, capacities, and limits.

| Data source | How the agent gets the data | Best route |
| --- | --- | --- |
| Spreadsheet or CSV export from accounting, production, or planning software (for example Excel, Tally, or an ERP item and capacity report) | The user uploads the file to the agent | Local engine for large files, hosted tool for small ones |
| An ERP or planning system that has its own MCP server (for example Dynamics 365, Business Central, or SAP) | The agent reads items, capacity, routes, or projects, then passes the rows on | Hosted tool inside the row caps above. Above those caps, export a file and use the local engine. |
| Dataverse tables | The Dataverse MCP server or the Dataverse connector | Same as above |
| SQL database, Excel workbook online, or SharePoint list | A Power Platform connector on the same agent or flow | Same as above |
| A scheduled flow | A Power Automate flow reads the source on a schedule and calls this connector | Hosted tool |
| An MPS model already on disk | Pass the file path to the local engine | Local engine |

The connector needs only rows. Any other tool that returns those rows can feed it the same way. These systems are examples of where the rows live. The connector is not certified against each one.

**Works alongside your system of record.** That system's MCP server gives the agent the records. This connector adds the planning calculation. No planning module has to be configured. The system supplies the tables, this connector returns the plan, and a person approves it.

**What to pull from the source**

| Data in your system | Field it fills | Why it matters |
| --- | --- | --- |
| Product, profit or price, and cost | `products` | Sets the production mix and the margin |
| Machine, line, or material capacity | `resources` | The limits the mix cannot pass |
| Use per unit | Columns named like the resources, `uses`, or `usage` | Ties each product to each limit |
| Ingredient cost and nutrients | `ingredients` | The blend cost and the spec math |
| Spec minimum and maximum | `requirements` | What the blend must hit |
| Depot supply and customer demand | `sources`, `destinations` | What can ship, and what must arrive |
| Cost per route | `costs` | The objective of the shipping plan |
| Crew required by period, and shift patterns | `periods`, `shifts` | Shift coverage |
| People, skills, and days off | `staff` | A named roster |
| Job time and allowed machines | `jobs`, `machines` | Machine loading |
| Project value, cost, and rules | `projects`, `budget` or `budgets`, `groups` | Resource allocation |
| Objective and constraints as text, or an MPS file | `objective`, `constraints`, `mps` | A custom linear or mixed-integer model |
| The plan in use today | `current_plan` | The gain over today |

**Column names can stay as they are.** You do not need to rename headers before the call. The match is reported with the result.

<!-- TODO Saifs AI: name the exact response field that lists column matches. A signed-in call on 17 November 2026 returned 401, so that field name is not invented here. -->

| Template | Headers recognized (examples) |
| --- | --- |
| Product | product, sku, item; profit, margin, contribution; price, selling price; cost, unit cost; min qty, committed; demand, max demand |
| Resource | resource, machine, material; capacity, hours available, stock |
| Ingredient | ingredient, food, material; cost per kg, unit cost; min amount; max amount, stock; min pct, max pct |
| Spec | nutrient, property, spec; minimum; maximum |
| Source and destination | depot, plant, supplier, origin; customer, site; supply, capacity; demand, required |
| Shift and period | period, day, slot; staff required; shift, pattern; wage, pay; periods covered |
| Staff | cost per shift, hourly rate; max shifts, min shifts, max hours; off, leave; skills |
| Job | duration; machines; due date |
| Project | project, option, initiative; benefit, npv, return, score; must, mandatory; exclude, blocked; partial, fractional |

**File formats for the local engine.** CSV, TSV, or JSON for each table. A custom model can also be an MPS file, passed as `mps_path`. Save an Excel sheet as CSV first, or ask the agent to convert it in the sandbox.

## How it works

The connector offers two routes to the same calculation. The agent picks the route from the size of the model.

| | Hosted tool | Local engine |
| --- | --- | --- |
| Tool name | `optimization_solver` | `optimization_solver_get_engine` |
| Use when | About 3,000 variables or constraints or fewer, a built model up to 5,000 variables, inside the row caps, and a time limit of 60 seconds is enough | A larger model, an MPS file, a run longer than 60 seconds, or when the tables must stay in your environment |
| What the agent sends | The tables and settings | File paths and settings only |
| What comes back | The plan and the explanation | A ready-to-run Python script |
| Where the calculation runs | Saifs AI endpoint | Your agent's code-execution sandbox |
| Where your planning tables go | To the Saifs AI endpoint for calculation | Nowhere. They stay in the sandbox. |
| Requires | Nothing extra | An agent host with code execution, and `numpy` |

**Local engine, step by step**

1. The user uploads the tables, or points the agent at files already in the sandbox.
2. The agent calls `optimization_solver_get_engine` with the file paths and any settings.
3. The connector returns the calculation script.
4. The agent saves that script exactly as given and runs it in its sandbox against the files.
5. The script prints the decision cards and the plan, and writes the full detail to `optimization_result.json`.
6. The agent reports those results to the user.

Do not shorten, retype, or rewrite the script. Do not replace it with other code. If the agent cannot run code, it should say so. It should not guess a plan.

### Inside the connector

1. The user asks for the best plan within the limits.
2. The agent chooses a tool and calls the connector. The call goes to the Saifs AI MCP server over HTTPS at `https://optimization-solver-mcp-microsoft.saifs.ai/mcp`.
3. The server checks the signed-in account.
4. The hosted tool runs the engine on the tables and returns the result. The engine tool returns the same engine as a script. The agent runs that script in its sandbox.
5. The agent reads the result and explains it in plain language. The plan comes from the engine. The wording comes from the agent.

### Inside the calculation

Both routes run the same engine. The steps below are the ones the live tool describes, with the defaults from its schema.

| Step | What the engine does | Method |
| --- | --- | --- |
| 1. Choose the template | Uses `problem_type` when you set it. Otherwise it detects the template from the tables. Aliases such as diet, shipping, roster, knapsack, and lp are understood. | The six templates: production mix, blend, transportation, schedule, allocation, custom |
| 2. Read the columns | Accepts the headers in the table above. Extra numeric columns on a product or ingredient row are kept. The result reports what was missing. | Alias matching. Nothing is required before the first call. |
| 3. Build a production mix | Maximizes profit, or price minus cost, subject to each resource and to min and max on each product. | Linear programming |
| 4. Build a blend | Minimizes ingredient cost subject to each nutrient minimum and maximum, share limits, and batch size. | Linear programming |
| 5. Build a shipping plan | Minimizes route cost subject to supply and demand. Shortage can be costed when you allow it. | Transportation model |
| 6. Build a schedule | Covers periods with shifts, builds a named roster, or loads jobs on machines. | Mixed-integer programming when people or assignments must be whole |
| 7. Build an allocation | Maximizes value subject to budgets, groups, dependencies, and must or exclude flags. | Linear programming, or mixed-integer programming when a project is all or nothing |
| 8. Build a custom model | Reads an objective such as `maximize 3x + 5y`, constraint text such as `2x + 3y <= 10`, or a whole MPS file. | Linear programming or mixed-integer programming, from the variable types you set |
| 9. Decide fractions or whole units | `auto` keeps fractions when the best fractional plan is already whole. Otherwise it switches to whole numbers. `true` always uses whole numbers. `false` allows fractions. | Branch and bound for whole numbers |
| 10. Stop a long whole-number search | Stops at the time limit, the gap, or the node limit, and reports the gap. | Hosted default 20 seconds, cap 60. Local engine up to 600 seconds. Gap default 0.0001, which is 0.01%. Node default 200,000. |
| 11. Explain the plan | Names the bottleneck, what one more unit is worth and the range that value holds over, what a price or cost must become to change the plan, and the gain over `current_plan` when you sent one. | Shadow price and sensitivity |
| 12. Explain an impossible plan | Names the limits that conflict and the smallest change that would allow a plan. | Infeasibility analysis |
| 13. List the rows | Spells out up to `max_listed` rows in the summary. Default 500. The full plan is still computed. | A display cap, not a solve cap |

**Why the answer repeats.** Each step applies the model you sent. The server describes no random step. The same tables and the same settings return the same plan. If a whole-number search stops on the time limit, the result is the best plan found so far and the remaining gap. The agent's wording around those numbers may vary.

**What happens when data is missing.** The call still runs. The result says what was missing. These defaults apply when you omit the setting:

- Currency label is INR. It does not convert money.
- `whole_units` is `auto`.
- Hosted time limit is 20 seconds, and cannot exceed 60. The local engine accepts up to 600.
- `mip_gap` is 0.0001. `max_nodes` is 200,000. `max_listed` is 500.
- A shift with no cost is costed at 1.
- A 7-period week with no shifts uses a shift length of 5. Shifts may wrap the week unless you set `cyclic` to false.
- Blend requirements are read per unit when `batch_size` is set, and as totals otherwise. If the basis is per unit and `batch_size` is omitted, 100 is used.
- A custom variable with no lower bound starts at 0.
- Unmet transport demand is not allowed unless `allow_shortage` is true.
- A roster may leave a place unfilled unless you set `allow_unfilled` to false. One person is not given two shifts on the same day unless you turn that rule off.

**Where the working is shown.** The result carries the plan and the decision cards. It also carries the bottleneck, what one more unit is worth, and the price or cost change that would change the plan. When you sent a current plan, it carries the gain over today. When no plan exists, it carries the conflicting limits. The local engine prints the decision cards and the plan. It writes the full detail to `optimization_result.json`.

<!-- TODO Saifs AI: capture one signed-in response and name the exact JSON fields for the plan, the bottleneck, the shadow price and its range, the sensitivity, the gain over current_plan, and the infeasibility detail. The endpoint returned 401 without a token, so those field names are not invented here. -->

## Data handling and security

| Question | Local engine | Hosted tool |
| --- | --- | --- |
| Do the planning tables reach Saifs AI? | No. The tool returns a script. The rows stay in the file you point at. | Yes. The rows passed in the call are sent for calculation. |
| Where are they processed? | Your agent host's code sandbox | `optimization-solver-mcp-microsoft.saifs.ai`, over HTTPS |
| What happens to the rows after the call? | They never left your sandbox | They are processed in the request and are not stored as your planning tables. |
| What does Saifs AI record? | Only that the engine script was requested | The account used to sign in, for access and billing. Not the planning rows. |

<!-- TODO Saifs AI: confirm the hosted-tool retention line for this server, including whether call time, tool name, or a hash of the request is logged. The sentence above matches the public product statement that rows are processed in the request and not stored. It does not copy Inventory Intelligence's audit-hash wording, because that detail is not confirmed here. -->

**What "sandbox" means.** The sandbox is the code-execution environment of your own agent host, such as Copilot Studio or Azure AI Foundry. It belongs to your tenant, not to Saifs AI, and your host's data policies continue to apply to it.

**Use the local engine only.** If your policy requires that planning tables never leave your environment, turn off the hosted tool. In Copilot Studio, open the agent's **Tools** tab, select the Optimization Solver server, turn off **Allow all**, and switch off `optimization_solver`. Leave `optimization_solver_get_engine` on. The agent can then use only the local engine, and any tool added to the server later stays off by default. This setting is per agent.

**Sign-in.** Users sign in with Microsoft single sign-on. Saifs AI holds only the account details needed for sign-in and billing. No API key is created, shared, or stored.

**No write access.** The connector has no action that creates, changes, or deletes records in an ERP, a planning system, a roster system, or Dataverse.

**Named staff.** A roster can include people's names, skills, and days off. That is personal data. Send only the fields the roster needs. Do not add contact details or other personal fields the plan does not use.

**The script is open to review.** The local engine script is plain Python and needs only `numpy`. It does not need `scipy`. Your security team can read it before allowing it to run. Run it as it was returned.

<!-- TODO Saifs AI: confirm whether the returned script makes any network call. The live tool description does not say. Do not claim "no network calls" until that is confirmed. -->

## Security and compliance

This section separates what an independent party has reviewed from what Saifs AI states about itself.

**Reviewed by Microsoft**

| Item | What it covers |
| --- | --- |
| Microsoft connector certification | Microsoft reviews and tests this connector before listing it as a certified connector. The review covers the connector's function and packaging, not Saifs AI's internal systems. |

**Self-assessed by Saifs AI**

Self-assessments are statements Saifs AI makes about its own practices. They are published so that you can check them, and they are not independent audits.

<!-- PUBLISH EACH ROW ONLY AFTER IT IS COMPLETED. Until then, keep the row inside this comment. Add the public link when you have it.

| Self-assessment | What it is | Where to read it |
| --- | --- | --- |
| Microsoft Publisher Attestation | Saifs AI's answers to Microsoft's questionnaire on security, data handling and compliance. Microsoft publishes the answers and does not verify them. Renewed every year. | [link to the Microsoft app compliance page for this offer] |
| CSA STAR Level 1 | Saifs AI's completed Cloud Security Alliance questionnaire (CAIQ), published on the public STAR Registry. Updated every year. | [link to the STAR Registry entry] |
| CSA STAR for AI Level 1 | Saifs AI's completed AI questionnaire (AI-CAIQ) on the same registry. | [link to the STAR Registry entry] |

-->

| Design fact | Detail |
| --- | --- |
| Customer planning rows stored | None on the hosted tool, which processes them in the request. The local engine keeps rows in your sandbox. |
| Passwords held | None. Sign-in is Microsoft single sign-on. |
| Write access to your systems | None |
| Local engine | Plain Python, one dependency (`numpy`), returned as a script you can read |

**Independent audits.** Saifs AI does not yet hold an independently audited certificate such as SOC 2 or ISO 27001. If your review needs a completed security questionnaire, request it through the [contact page](https://saifs.ai/contact-us).

## Token usage and cost

With the hosted tool, the model has to send every row, so token use grows with the file. With the local engine, the rows stay in the sandbox. The model handles the script and the printed summary, not the file.

The figures below count only the tokens used to send the rows to the hosted tool. They do not include the tool's reply, and they do not include the local engine script. Those two counts have not been measured on this server, so this page states no run cost and no savings multiplier.

| Planning file | Hosted tool: tokens to send the rows | Local engine |
| --- | --- | --- |
| 4 products, 2 resources | about 240 tokens | Not measured. The script has not been captured from a signed-in call. |
| 500 products, 8 resources (the hosted product cap) | about 54,000 tokens | Not measured |
| 2,000 products, 8 resources | Not accepted. The rows alone are about 217,000 tokens. | The local engine is the route. Script tokens are not measured. |

**Estimate the rows you would send.** A product row with a name, a profit, a minimum, a maximum, and 8 resource columns is about 108 tokens. A shipping route with a source, a destination, and a cost is about 39 tokens. Names longer than "Product 12" or "Machine 3" add tokens.

**Which route to choose, on file size and privacy**

- **A small table:** the hosted tool. It is one call, and the row payload is small.
- **Above the row caps, above about 3,000 variables or constraints, a built model over 5,000 variables, an MPS file, or a run longer than 60 seconds:** the local engine. It is the only route.
- **The tables must not leave your environment:** the local engine, with the hosted tool switched off.
- **A repeated plan in one session:** the local engine keeps the script in the sandbox. The token saving is not stated here, because the script size is not measured.

**Hosts that bill per action.** Some agent hosts charge a fixed amount for each tool call and do not bill by token. On those hosts the token counts above do not change the bill. Choose the local engine for file size and for where the rows are processed.

How these figures were measured: the `o200k_base` tokenizer, on generated JSON for a production mix, with default JSON spacing. Counts vary with column names and with how many resources each product uses. Measured on 17 November 2026. Reply tokens and script tokens are excluded on purpose.

<!-- TODO Saifs AI: call optimization_solver_get_engine, count the script with the same tokenizer, call optimization_solver on the 4-product sample, count the reply, then add the cost table at $1.25 per million input tokens and $10 per million output tokens (November 2026 reference). Do not publish a dollar figure before those two counts exist. -->

## Prerequisites

- A Saifs AI account, created by signing in with Microsoft single sign-on
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Access to Copilot Studio, Microsoft 365 Copilot, Azure AI Foundry, Power Automate, Power Apps, or Azure Logic Apps
- The tables for the plan you want: products and resources, ingredients and specs, routes, shifts, projects, or a custom model
- For the local engine: code execution enabled on the agent host

## How to get credentials

No keys or secrets are needed. When you create a connection, you are redirected to sign in with your Microsoft account and approve access. If the connection later expires, sign in again from the connection page.

## Get started with your connector

1. In Copilot Studio, open your agent and go to **Tools**, then **Add a tool**.
2. Search for **Optimization Solver** and select it.
3. Create a connection and sign in.
4. Add this to the agent's instructions: "For any question about production mix, blending, shipping, staffing, rosters, machine scheduling, budget allocation, or a linear or mixed-integer plan, call the Optimization Solver tool. Do not guess quantities, routes, rosters, or bottleneck values, and do not write your own solver. If a file is large, an MPS file, or already in the sandbox, call optimization_solver_get_engine and run the script exactly as returned."
5. Provide the tables by pasting them, uploading them, or pointing the agent to a file. Include today's plan if you want the gain over today.
6. Ask in plain language, for example "Find the best production mix within capacity" or "Allocate the budget across these projects."

Treat the result as decision support. A person approves the plan before anyone changes a system.

## For administrators

Enabling this connector gives the organization one approved way to decide the best plan within the limits, inside the agents your makers already build.

**Why this is an administrator decision.** Without a calculation tool, makers ask an agent to try quantities from a spreadsheet. A language model can give a different plan on each run, and the result cannot be compared week to week. A governed connector replaces that with one method for every agent and team. Scheduling optimization and resource allocation then use the same solver.

- **You choose who can connect it.** Tenant and environment permissions apply. Only users who can create a connection can call the tools.
- **You keep your system of record.** The connector recommends and explains. It does not change production orders, shipments, rosters, or budgets.
- **You can check the number.** The same tables and settings can be rerun. Missing inputs and conflicting limits are reported, not assumed.
- **You control where data goes.** Turn off the hosted tool per agent to keep planning tables in your environment.
- **You set data policy as usual.** Place the connector in the Business, Non-business, or Blocked group with Power Platform data loss prevention policies.
- **There are no secrets to rotate.** Expired connections are renewed by signing in again.

**Review checklist**

- Decide whether the hosted tool is allowed, or local engine only
- Assign the connector to the correct data policy group
- Review the privacy policy and terms
- Read the self-assessments listed under Security and compliance, once they are published
- Review the local engine script if your policy requires it
- If a roster will name people, confirm the staff fields are limited to what the plan needs

## For developers

Nothing is required on the first call. Send the tables the user has, with their column names unchanged. Set `problem_type` only when the tables could match more than one template. The response reports what was missing.

There are two tools. `optimization_solver` solves the model. `optimization_solver_get_engine` returns the script for a model the hosted tool should not run. Explanation is part of the solve. There is no separate explain tool.

### Production mix

| Field | Meaning | Needed for |
| --- | --- | --- |
| `products` | Up to 500 rows. Each needs a name and a profit, or a price and a cost. | The quantities to choose |
| `resources` | Up to 200 rows. Each needs a name and `available`. | The limits |
| `usage` | Optional. Up to 20,000 rows of product, resource, and amount, when use is not already on the product. | The per-unit consumption |
| `min`, `max` on a product | Least that must be made, and most that can be sold | Bounds. `max` aliases include demand. |

Per-unit use can sit in a `uses` object, or in columns named like the resources.

### Blend

| Field | Meaning | Needed for |
| --- | --- | --- |
| `ingredients` | Up to 1,000 rows. Each needs a name and a cost. Other numeric columns are nutrients. | The recipe |
| `requirements` | Up to 300 rows. Nutrient name plus min, max, or both. Names must match the nutrient columns. | The spec |
| `batch_size` | Total amount of one batch. Omit it for a diet stated in totals. | Per-unit specs |
| `basis` | `per_unit` or `total`. Defaults to `per_unit` when `batch_size` is set, otherwise `total`. | How to read the spec |
| `min_share`, `max_share` | Share of the batch. 0.05 and 5% both work. | Inclusion limits |

### Transportation

| Field | Meaning | Needed for |
| --- | --- | --- |
| `sources` | Up to 500 rows. Name and supply. | What can ship |
| `destinations` | Up to 2,000 rows. Name and demand. | What must arrive |
| `costs` | Rows of source, destination, cost, and optional route capacity. A column per destination, or a nested map, also works. Up to 100,000 entries. | The objective |
| `allow_shortage` | Default false. Set true when demand may exceed supply. | A plan instead of an infeasible result |
| `shortage_cost` | Cost of each unmet unit. Used only when shortage is allowed. | The penalty |

### Schedule

Three shapes share `problem_type` `schedule`.

| Field | Meaning | Needed for |
| --- | --- | --- |
| `periods` | Up to 1,000 rows. Name and staff required. | Shift coverage |
| `shifts` | Up to 1,000 rows. Name, cost, and the periods covered, or a start and a length. Cost defaults to 1. | The patterns |
| `shift_length` | Builds one pattern for every run of this many periods, when shifts are not listed. A 7-period week with no shifts defaults to 5. | Generated patterns |
| `cyclic` | Default true. A shift may wrap from the end of the horizon back to the start. | A repeating week |
| `staff` | Up to 300 people. Cost, max shifts, max hours, unavailable days, and skills. | A named roster |
| `allow_unfilled` | Default true. A shift may stay short, at `unfilled_penalty`. | Cover that cannot be met |
| `one_shift_per_day` | Default true. | Stopping a double shift |
| `forbidden_pairs` | Up to 5,000 pairs of shift names one person must not work both of. | Rules such as night then early |
| `jobs` | Up to 500 rows. Duration, allowed machines, and an optional due time. | Machine loading |
| `machines` | Up to 100 names. | The machines that can take work |

### Allocation

| Field | Meaning | Needed for |
| --- | --- | --- |
| `projects` | Up to 2,000 rows. Name and value. Cost is one number, or a column per budget. | The options |
| `budget` | A single budget the chosen projects must fit inside. | One-limit cases |
| `budgets` | Up to 50 rows. Name and available. | Money, people, and a period together |
| `groups` | Up to 500 groups, each with members and a min or max count. | Mutually exclusive sets |
| `must`, `exclude`, `requires` | Forced in, forced out, or dependent on another project | Rules |
| `divisible` | True when a fraction of the project can be funded. Otherwise it is all or nothing. | Whole projects |

### Custom model

| Field | Meaning | Needed for |
| --- | --- | --- |
| `variables` | Up to 3,000. Name, lower (default 0), upper, and type `continuous`, `integer`, or `binary`. | Named decisions |
| `objective` | Text such as `maximize 3x + 5y`, or an object with sense and terms. | What to maximise or minimise |
| `constraints` | Up to 3,000 rules, as text or as terms. Extra rules can also be sent with any template. | The limits |
| `mps` | A whole MPS model as text, up to 2,000,000 characters. A file on disk uses `mps_path` on the local engine. | An existing model |

### Options and defaults

| Option | Default | Purpose |
| --- | --- | --- |
| `problem_type` | Detected from the tables | Force a template when detection could go either way |
| `current_plan` | None | Measure the gain over the plan in use today |
| `whole_units` | `auto` | `true` forces whole numbers. `false` allows fractions. `auto` switches only when the fractional plan is not already whole. |
| `currency` | INR | Display label only. No conversion. |
| `currency_symbol` | Taken from `currency` | The symbol printed with money amounts |
| `time_limit_seconds` | 20 | Stop a whole-number search and report the gap. Hosted maximum 60. Local engine maximum 600. Minimum 0.1. |
| `mip_gap` | 0.0001 | Stop once the remaining gap is within this fraction of the best possible. 0.0001 is 0.01%. Maximum 0.5. |
| `max_nodes` | 200,000 | Most branch and bound nodes before reporting the plan found so far and the gap. Maximum 2,000,000. |
| `max_listed` | 500 | Most rows spelled out in the summary. Maximum 100,000. The full plan is still computed. |

### Sample request

```json
{
  "problem_type": "production_mix",
  "products": [
    {"name": "Chair", "profit": 40, "hours": 2, "wood": 3, "max": 100},
    {"name": "Table", "profit": 50, "hours": 1, "wood": 4, "max": 80}
  ],
  "resources": [
    {"name": "hours", "available": 200, "unit": "hours"},
    {"name": "wood", "available": 300, "unit": "kg"}
  ],
  "whole_units": "auto"
}
```

The two products show the shape only. Resource use sits in columns named `hours` and `wood`, matching the resource names. Currency is omitted, so the label stays INR.

### Sample response (shortened)

<!-- TODO Saifs AI: replace this note with a shortened response from a signed-in call to optimization_solver. Do not invent fields. The call on 17 November 2026 returned 401 Unauthorized. -->

A live response is not printed here, because the endpoint requires sign-in and this page does not invent result fields. What the tool contract says you get back:

- Decision cards, and the plan
- The bottleneck, what one more unit of it is worth, and the range that value holds over
- What a price or cost would have to become to change the plan
- How much better the plan is than `current_plan`, when you sent one
- When no plan exists, the limits that conflict and the smallest change that fixes it
- A note of anything that was missing from the tables
- The remaining gap, when a whole-number search stops early

The local engine writes the same detail to `optimization_result.json`.

### Response sections

| Section | Use it for |
| --- | --- |
| Decision cards | The plain-language action for this plan |
| Plan | Quantities, recipe amounts, shipments, shifts, or chosen projects |
| Bottleneck | Which limit binds, what one more unit is worth, and the range that value holds over |
| Sensitivity | The price or cost change that would change the plan |
| Gain over today | The improvement versus `current_plan`, only when you sent one |
| Infeasibility | The conflicting limits and the smallest change, when no plan exists |
| Gap | How far a stopped whole-number search may be from the best possible plan |
| `optimization_result.json` | The full local-engine output, written in the sandbox |

<!-- TODO Saifs AI: if the live response includes an engine version, name that field here. It is not in the published tool schema. -->

## Known issues and limitations

- **Decision support only.** It does not change a production order, a recipe, a shipment, a roster, or a budget. A person approves the plan.
- **On request.** The tools do not replan by themselves. A scheduled flow can call them again.
- **Hosted tool size.** About 3,000 variables or constraints or fewer, a built model up to 5,000 variables, the row caps in At a glance, and a time limit of 60 seconds. Use the local engine beyond that.
- **Whole-number gap.** If branch and bound stops on the time limit, the node limit, or `mip_gap`, the result is the best plan found so far and the remaining gap.
- **Missing inputs.** The call still runs. The result says what was missing. It does not invent a plan to fill the hole.
- **Impossible limits.** The result names the conflict and the smallest change. It does not return a plan that breaks a limit.
- **Out of scope.** Demand forecasting, nonlinear models, stochastic models, and vehicle routing with time windows are not this tool.
- **Currency.** The default label is INR. There is no conversion between currencies.
- **Named staff.** Send only roster fields. A person's name, skills, and days off are personal data.
- **Script.** The local engine script must be run as returned. Do not rewrite it. It needs `numpy` only.
- **Display cap.** `max_listed` defaults to 500 rows in the summary. The full plan is still computed.
- **Regions.** Not available in US Government (GCC, GCC High, DoD) or China (21Vianet) clouds.

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| The tool cannot run, or asks you to sign in again | The connection has expired, or the call has no token. An unsigned call to the endpoint returns 401. | Open the connection and sign in again |
| The result says no plan exists, and names limits that conflict | The limits cannot all be met | Relax the limit the result names, or accept the smallest change it reports |
| The plan includes a gap | A whole-number search hit the time limit, the node limit, or `mip_gap` | Raise `time_limit_seconds` within the cap, or run the local engine for up to 600 seconds |
| The result lists inputs that were missing | Those columns were not in the tables | Add the columns. Do not expect the tool to guess them. |
| The agent states a plan without calling the tool | The agent instructions do not require the tool | Add the instruction in Get started |
| The local engine does not run | Code execution is not enabled, or the script was rewritten | Enable code execution and run the script exactly as returned |
| The file is over a hosted cap | For example more than 500 products, 2,000 destinations, 60 seconds, or an MPS file on disk | Call `optimization_solver_get_engine` and point it at the files |

## FAQ

**Do I need an operations-research specialist to use this?**
No. A user asks in plain language and sends the tables they have. Linear programming and mixed-integer programming run inside the tool.

**What is the least data I need?**
Whatever table you already have. A production mix needs products and resources. A blend needs ingredients and specs. Nothing else is required before the first call. The result reports what was missing.

**Does my data leave my environment?**
Not with the local engine. With the hosted tool, the rows in the call are sent to Saifs AI for calculation. You can turn the hosted tool off.

**Will it change anything in my systems?**
No. It does not write to an ERP, a roster, a transport system, or Dataverse. It recommends a plan. A person approves it.

**I already have an ERP. Why add this?**
That system holds the items, the capacity, and the orders. This connector adds the planning calculation: production mix, blend, shipping, staffing, and resource allocation. It does not replace the system of record.

**Why do I get the same answer every time?**
The same tables and the same settings return the same plan. The numbers come from the solver. The agent's wording around them may vary. A whole-number search that stops early also returns the remaining gap, so you can see it did not claim a finished proof.

**What happens if my data is incomplete?**
The tool still runs and reports the missing pieces. When the limits cannot all be met, it names the conflict and the smallest change. It does not guess a feasible plan.

**Can I check how a result was produced?**
Yes. The result includes the plan, the bottleneck, what one more unit is worth, and the conflicting limits when no plan exists. The local engine writes the same detail to `optimization_result.json`.

**How large a file can it handle?**
The hosted tool stays inside the caps in At a glance, including 500 products and a 60-second limit. Larger files, MPS files, and longer runs use the local engine.

**Can it run on a schedule?**
Yes, by calling it from a scheduled Power Automate flow. The connector itself has no timer. That is the supported way to repeat a production mix or a shift plan.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment). Microsoft Premium connector licensing applies separately.

## Support

**Optimization Solver - Saifs AI** is published by Saifs AI.

- Website: [https://saifs.ai](https://saifs.ai)
- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Learn more: [https://saifs.ai/optimization-solver](https://saifs.ai/optimization-solver)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Privacy: [https://saifs.ai/privacy_policy/optimization-solver](https://saifs.ai/privacy_policy/optimization-solver)
- Terms: [https://saifs.ai/eula](https://saifs.ai/eula)
- Sign-in: Microsoft single sign-on (OAuth 2.0). No API keys.
- MCP endpoint: [https://optimization-solver-mcp-microsoft.saifs.ai/mcp](https://optimization-solver-mcp-microsoft.saifs.ai/mcp)
