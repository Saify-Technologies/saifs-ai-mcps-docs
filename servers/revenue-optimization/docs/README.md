# Dynamic Pricing & Overbooking - Saifs AI

**Dynamic Pricing & Overbooking - Saifs AI** helps teams that sell perishable capacity decide **what price to charge and how much to overbook**. You send the capacity left and the price now. Add bookings, price history, or fare classes when you have them. It returns a price, protection levels, booking limits, and an overbooking limit, with the chance of turning a customer away.

It is built for hotels, airlines, events, rentals, restaurants, and clinics that still price from a rate card and need a recommendation they can rerun.

It recommends a price and a booking limit. It does not change either one in your systems. It is not financial, legal, or clinical advice.

**Publisher:** Saifs AI
**Website:** [https://saifs.ai](https://saifs.ai)
**Learn more:** [https://saifs.ai/dynamic-pricing-overbooking](https://saifs.ai/dynamic-pricing-overbooking)
**Pricing:** [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
**Contact and help:** [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
**Privacy:** [https://saifs.ai/privacy_policy/dynamic-pricing-overbooking](https://saifs.ai/privacy_policy/dynamic-pricing-overbooking)
**Terms:** [https://saifs.ai/eula](https://saifs.ai/eula)
**Sign-in:** Microsoft single sign-on (OAuth 2.0). No API keys.

| If you are | Start with |
| --- | --- |
| A business owner or revenue lead | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [Where the data comes from](#where-the-data-comes-from) |
| An administrator or security reviewer | [Data handling and security](#data-handling-and-security), [Security and compliance](#security-and-compliance), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [Where the data comes from](#where-the-data-comes-from), [For developers](#for-developers) |
| A revenue manager checking the method | [Inside the calculation](#inside-the-calculation) |

## The problem it solves

A room, seat, ticket, or slot earns nothing once the date has passed. Many teams still sell that capacity from a fixed rate card, or from last week's price. Sell too early and the higher fare is gone. Hold back too much and the capacity expires empty.

Overbooking has the same tradeoff. Too little, and no-shows leave you empty. Too much, and you turn customers away. The yield management math for this has been standard in revenue management for years. It usually sits in systems that only large airlines and hotel groups run.

Dynamic pricing makes that calculation available to any agent:

- **What price to charge now**: a price inside your floor, ceiling, and maximum change.
- **How much to hold for later buyers**: protection levels and booking limits by fare class.
- **How demand moves with price**: price elasticity, or a refusal when the history cannot support it.
- **How much to overbook**: a booking limit, with the chance of turning anyone away.
- **Whether pace is ahead or behind**: booking pace against comparable past dates, and demand still to come.

## Why choose it: the same answer every time

The same capacity, bookings, and settings return the same price, protection levels, and overbooking limit. The figures come from established revenue management and yield management methods. They are not written afresh by a language model each time you ask.

That matters because a rate change and an overbooking limit move money and guests. You need a figure you can rerun and compare with last week. The agent's wording around the numbers may vary. The numbers do not.

**In plain language:** you give it capacity left, the current price, and history when you have it. It tells you what price to charge and how much to overbook.

**For technical readers:** demand still to come is an additive-pickup forecast. Price elasticity is a log-log regression. Protection levels use Littlewood, EMSR-a, and EMSR-b. Overbooking uses a binomial model and Beta-Binomial show rates.

| What it does | Established method |
| --- | --- |
| Forecasts demand still to come, with a range | Additive-pickup from comparable past dates |
| Sets the price that raises expected contribution, inside a band | Pickup demand times the price response |
| Estimates how units move when price moves | Log-log regression, pooled across products, with a confidence interval |
| Protects capacity for a higher fare | Littlewood for two classes; EMSR-a and EMSR-b for many classes |
| Compares that policy with selling in arrival order | Expected revenue against first-come-first-served |
| Sets how far to overbook, and the chance of a turn-away | Binomial shows, with Beta-Binomial show rates by segment |

**It does not guess.** Price elasticity is refused when the price never changed, or when demand drove the price. A stand-in elasticity is used only if you supply one, and it is marked as assumed. Overbooking is not produced for a critical clinic type. A figure the engine cannot support is withheld, and the result says why.

**It recommends and does not act.** It never sets the price, the protection level, or the overbooking limit in a booking system. A person decides what to publish.

## Who should use this

| Role | How it helps |
| --- | --- |
| **Boutique hotel owners and revenue managers**, including B&Bs | What price to charge for a date, how many rooms to hold for late buyers, and how many to overbook |
| **Airline and seat analysts** | Protection levels and booking limits across fare classes, and expected revenue against first-come-first-served |
| **Event organizers and tour operators** | Ticket and departure prices as the date nears, and how many seats to hold for a higher tier |
| **Equipment, vehicle, and coworking rental operators** | A price and an overbooking limit from capacity and no-show history |
| **Restaurant managers** | A price and a booking limit for covers, using show history by segment |
| **Clinic and appointment managers** | A slot price with increases off unless you allow them, and no overbooking of critical types |
| **Parking operators and ad-slot sellers** | A price for capacity that expires at a deadline, from remaining units and pace |
| **Agent makers and developers** | A tested yield management calculation, so the agent does not invent a rate |

**Best fit:** hotels, airlines, events, rentals, restaurants, clinics, and other businesses that sell perishable capacity and revisit the price as the date approaches.

**Not required:** a revenue management system, a data science team, or a property management system. A capacity file is enough to start. Booking history makes the answer sharper.

## Use cases

Each use case below can be built as an agent. The agent types are listed in [Agents you can build](#agents-you-can-build).

**1. Price for a date that is close**
Scenario: a hotel still has rooms, and the weekend is a few days away.
Outcome: a price inside the floor, the ceiling, and the maximum change, with the reason on the decision card.
Ask: "What price should we charge for this weekend?"

**2. Hold capacity for a higher fare**
Scenario: you sell two or more prices for the same rooms, seats, or tickets.
Outcome: protection levels and booking limits, from Littlewood or EMSR, plus expected revenue against selling in arrival order.
Ask: "How many rooms should we hold for late corporate bookings?"

**3. How much to overbook**
Scenario: no-shows leave rooms, seats, or slots empty.
Outcome: an overbooking limit and the chance of turning anyone away.
Ask: "How many rooms should we overbook?"

**4. Booking pace against past dates**
Scenario: this week is filling faster or slower than the same point on past dates.
Outcome: booking pace versus those dates, and an additive-pickup forecast of demand still to come, with a range.
Ask: "Is this weekend ahead of booking pace?"

**5. How demand responds to price**
Scenario: the price has moved, and you want the response from your own history.
Outcome: price elasticity with a confidence interval, or a refusal when the history is flat or demand-driven.
Ask: "How does demand respond when we change the price?"

**6. Fare buckets for an event**
Scenario: an event organizer has several ticket tiers and a fixed house.
Outcome: nested protection levels and a booking limit for each tier.
Ask: "Set fare buckets for next week's event."

**7. Rental or restaurant capacity**
Scenario: vehicles, desks, or Saturday covers go unsold when people do not show.
Outcome: a price and an overbooking limit from capacity and show history.
Ask: "What price and overbooking limit should we set for Saturday?"

**8. Clinic slots, with critical types left alone**
Scenario: a clinic prices appointment slots. Some types are urgent.
Outcome: a price with increases off by default, and no overbooking limit on any row marked critical.
Ask: "Price these clinic slots, and do not overbook critical appointments."

**9. A full booking export**
Scenario: the file has more than 500 products, 20,000 bookings, or 5,000 price-history rows.
Outcome: the same calculation runs in the agent's own sandbox through the local engine. The booking file stays there.
Ask: "Run this on our full booking export."

## Agents you can build

Each row is one agent with this connector added as a tool. The last column is what else to add. The tools run on request. A schedule comes from a flow, not from the connector.

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Price agent | Recommends the price for a date, inside the band you set | A capacity file, or a source from [Where the data comes from](#where-the-data-comes-from) | 1 |
| Protection-level agent | Sets how much capacity to hold for higher fares, and the booking limit for each class | Fare classes on the request | 2, 6 |
| Overbooking agent | Sets the overbooking limit and shows the chance of a turn-away | Show history, or a show rate on the product | 3, 7 |
| Booking-pace agent | Compares pace with past dates and forecasts demand still to come | Booking history for the same product | 4 |
| Price-response agent | Estimates price elasticity, or says the history cannot support it | Price history with more than one price | 5 |
| Clinic slot agent | Prices slots with increases off, and skips overbooking on critical types | `industry` set to `clinic`, and `critical` on urgent types | 8 |
| Large-file agent | Runs the local engine on a full export | Code execution on the agent host | 9 |
| Property-system agent | Reads capacity and bookings from the system of record, then calculates. A person publishes the rate. | That system's own MCP server | 1, 3 |
| Forecast-to-price agent | Uses a demand forecast as demand still to come | A demand forecasting tool, passed in `expected_remaining_demand` | 1 |
| Scheduled pricing agent | Runs the same check each day without anyone asking | A Power Automate flow with a recurrence trigger, and email or Teams | 1, 4 |

## At a glance

| Item | Detail |
| --- | --- |
| Tools | `revenue_optimizer` (hosted calculation) and `revenue_optimizer_get_engine` (local engine) |
| Minimum input | No field is required. A useful start is one product row: `product`, `capacity`, `sold`, `days_out` or a date, and `price`. |
| Hosted tool size | Up to 500 products, 20,000 bookings, 5,000 price-history rows, 2,000 fare classes, and 2,000 show-history rows |
| Larger files | Local engine, in your agent's code sandbox. It needs `numpy`. Use it above 500 products, 20,000 bookings, or 5,000 price-history rows. |
| Writes to your systems | None. It does not set a price, a protection level, or an overbooking limit. |
| Local result file | `revenue_plan.json` in the sandbox |

<!-- TODO Saifs AI: confirm the response field that carries the engine version. The tool schema does not name it. -->

## Where the data comes from

Dynamic Pricing & Overbooking does not connect to your systems. The agent brings the rows. Any source that can supply capacity, bookings, or prices can feed it.

| Data source | How the agent gets the data | Best route |
| --- | --- | --- |
| A spreadsheet or CSV export from a property management system, booking engine, reservation system, or ticketing export | The user uploads the file | Local engine for a large file. Hosted tool for a small one. |
| A system that has its own MCP server, such as a property management system or a reservation system | The agent reads the records, then passes the rows | Hosted tool up to the size limits. Above those limits, export a file and use the local engine. |
| Dataverse tables | The Dataverse MCP server or the Dataverse connector | Same as above |
| A SQL database, an Excel workbook online, or a SharePoint list | A connector on the same agent or flow | Same as above |
| A scheduled flow | A Power Automate flow reads the source on a schedule and calls this connector | Hosted tool, if the file is within the size limits |
| A demand forecasting tool | Pass its figure in `expected_remaining_demand` on the product row | Either route |

The connector needs only rows. It has not been certified against each source system. Any other tool that returns the same rows can feed it.

**Works alongside your system of record.** That system's MCP server gives the agent the records. This connector adds the price, the protection level, and the overbooking limit. It does not replace the system that publishes the rate. A person, or a separate step you approve, publishes the change.

**What to pull from the source**

| Data in your system | Field it fills | Why it matters |
| --- | --- | --- |
| Product and date, such as a room type on a night or a flight on a day | `product`, `days_out` or the stay or event date | Identifies the capacity that expires |
| Capacity and units already sold | `capacity`, `sold` | Shows what is left |
| Price now, and the lowest and highest you will allow | `price`, `price_floor`, `price_ceiling` | Keeps the recommendation inside your band |
| Cost of the unit, and the cost of turning one customer away | `unit_cost`, `denied_cost` | Turns the choice into contribution, not just revenue |
| Fare classes still to sell | `class`, `fare`, `mean_demand`, `sd_demand` | Unlocks protection levels and booking limits |
| Past prices and units sold | Price history: `product`, `date`, `price`, units sold | Unlocks price elasticity |
| Reservations | Bookings: `product`, date, `booking_date` or `lead_time`, `status`, `class`, `price`, `segment`, `units` | Unlocks booking pace and the pickup forecast |
| Who showed up | Show history: `segment`, `booked`, `showed` | Unlocks show rates and the overbooking limit |

<!-- TODO Saifs AI: confirm the exact JSON keys for the stay or event date, the cancel date, and units sold on price history. The tool description names them in words, not as one key each. Do not rename columns in the sample until those keys are confirmed. -->

**Send the field names above.** The tool description does not list alternate headers. This page does not claim that an export header is recognized on its own.

<!-- TODO Saifs AI: if the engine maps common export headers to these fields, add that table from a live response and name the field that lists the matches. -->

**File formats for the local engine.** CSV, TSV, and JSON, as stated on `revenue_optimizer_get_engine`. Save a spreadsheet as one of those before the script reads it. You can pass a path for products, fare classes, price history, bookings, and show history. Each of those files is optional.

## How it works

The connector offers two routes to the same calculation. Pick the route from the size of the file, and from whether the rows may leave your environment.

| | Hosted tool | Local engine |
| --- | --- | --- |
| Tool name | `revenue_optimizer` | `revenue_optimizer_get_engine` |
| Use when | Up to 500 products, 20,000 bookings, and 5,000 price-history rows | Anything larger, or when booking data must stay in your environment |
| What the agent sends | The rows and the settings | File paths and settings only |
| What comes back | The full result, decision cards first | A ready-to-run Python script |
| Where the calculation runs | Saifs AI endpoint | Your agent's code-execution sandbox |
| Where your booking data goes | To the Saifs AI endpoint for the calculation | It stays in the sandbox. The tool states that no booking data reaches the server on this path. |
| Requires | Nothing extra | Code execution, and `numpy` |

**Local engine, step by step**

1. The user uploads the files, or points the agent at paths already in the sandbox.
2. The agent calls `revenue_optimizer_get_engine` with those paths and any settings.
3. The connector returns the calculation script. Save it exactly as given. Do not shorten it, retype it, or rewrite it.
4. The agent runs the script in its sandbox. The script reads the local files. It needs only `numpy`.
5. The script prints the decision cards and one line per product. It writes the full result to `revenue_plan.json`.
6. The agent reports the decisions. The numbers come from the script. The wording comes from the agent.

### Inside the connector

1. The user asks what price to charge, what to hold back, or how much to overbook.
2. The agent chooses a tool and calls this connector. The call goes to the Saifs AI MCP server over HTTPS at `https://revenue-optimizer-mcp.saifs.ai/mcp`.
3. The server checks the signed-in account.
4. The hosted tool runs the engine on the rows and returns the result. The engine tool returns the same engine as a script, for the agent to run in its sandbox.
5. The agent reads the result and explains it. The numbers come from the engine. The wording comes from the agent.

### Inside the calculation

Both routes run the same revenue management engine. It works through the rows in fixed steps. Decision cards are first. This is the yield management calculation behind the price and the overbooking limit.

| Step | What the engine does | Method |
| --- | --- | --- |
| 1. Read the rows | Takes one row per product and date, plus optional bookings, price history, fare classes, and show history. | The fields in [For developers](#for-developers) |
| 2. Booking pace | Compares units on the books with comparable past dates at the same days out. A gap wider than `pace_alert_pct` is flagged in the result. The default is 15%. | Comparison with past dates at the same days out |
| 3. Demand still to come | Adds the bookings that comparable past dates still took during the days that remain. Returns a range. | Additive-pickup forecast |
| 4. Price | Chooses the price with the highest expected contribution. Demand is the pickup forecast, adjusted for how units have moved with price. The price stays inside `price_floor`, `price_ceiling`, and `max_price_change_pct`. | Expected contribution inside the band |
| 5. Price elasticity | Fits how units move with price, pools products, and returns a confidence interval. Refuses a flat price history, and a history in which demand drove the price. | Log-log regression |
| 6. Protection levels | For two fares, protects the high fare up to the point where the chance that high-fare demand exceeds that protection, times the high fare, equals the low fare. For many fares, nests the protections. Also returns the exact optimal levels and expected revenue against first-come-first-served. | Littlewood; EMSR-a and EMSR-b |
| 7. Show rate | Estimates the share who show, by segment. A thin segment is pulled toward the overall rate. | Beta-Binomial shrinkage |
| 8. Overbooking | Raises the booking limit while the expected fare from one more booking exceeds the expected cost of turning a customer away. Shows are a binomial count at that show rate. Reports the chance of turning anyone away. Does not do this for a critical type. | Binomial overbooking |
| 9. Decision cards | Turns the figures into plain-language actions. These are first in the result. | Fixed templates filled with the calculated numbers |

**Why the answer does not change.** Each step is a fixed formula. No language model sets the price or the booking limit. The same input and the same settings return the same numbers. The agent's wording around those numbers may vary.

**What happens when data is missing.** Bookings, price history, fare classes, and show history are optional. A result is produced only for the steps those rows support. A product row with capacity, sold, days out, and price is enough to ask for a price. Booking pace and the pickup forecast need booking history. Price elasticity needs enough distinct prices, default 3, and is refused when that history cannot support it. If you pass `default_elasticity`, it is your assumption and is flagged as assumed. It is not a silent default. Overbooking is withheld for a row marked `critical`, and it is not applied to critical clinic types.

**Where the working is shown.** The hosted result leads with decision cards. The local script prints those cards and one line per product, and writes the rest to `revenue_plan.json`.

<!-- TODO Saifs AI: name the response fields that carry the decision cards, the per-product line, the refusal reason, and the assumptions. Take the names from a live response, not from another product. -->

## Data handling and security

| Question | Local engine | Hosted tool |
| --- | --- | --- |
| Does booking data reach Saifs AI? | No. The tool states that no booking data reaches the server on this path. | Yes. The rows passed in the call are sent for the calculation. |
| Where is it processed? | Your agent host's code sandbox | `revenue-optimizer-mcp.saifs.ai`, over HTTPS |
| What happens to the data after the call? | It stayed in your sandbox. | Saifs AI states that rows are processed in the request and not stored. See the note below. |
| What can the script do? | It reads the local files and writes `revenue_plan.json`. | Not used on this path. |

<!-- TODO Saifs AI: confirm hosted-tool retention in writing, including whether any billing or audit log keeps a hash, the account, or the rows. The product page says data is processed in the request and never stored. This page repeats that statement and does not add an audit-log claim. Also confirm the hosting region, rate limits, and request timeout. -->

**What "sandbox" means.** The sandbox is the code-execution environment of your own agent host, such as Copilot Studio or Azure AI Foundry. It belongs to your tenant, not to Saifs AI. Your host's data policies still apply to it.

**Use the local engine only.** If booking data must not leave your environment, turn off the hosted tool. In Copilot Studio, open the agent's **Tools** tab, select the Dynamic Pricing & Overbooking server, turn off **Allow all**, and switch off `revenue_optimizer`. Leave `revenue_optimizer_get_engine` on. The agent can then use only the local engine. A tool added to the server later stays off until you enable it. This setting is per agent.

**Sign-in.** Users sign in with Microsoft single sign-on. Saifs AI holds only the account details needed for sign-in and billing. No API key is created, shared, or stored.

**No write access.** The connector cannot create, change, or delete a rate, a booking limit, or any record in your systems. Both tools are marked read-only.

**The script is returned in full.** Your security team can read it before allowing it to run. It is Python and needs `numpy`. Do not shorten it or rewrite it.

<!-- TODO Saifs AI: confirm whether the local script makes network calls. This page does not claim that it does not. -->

## Security and compliance

This section separates what an independent party has reviewed from what Saifs AI states about itself.

**Reviewed by Microsoft**

| Item | What it covers |
| --- | --- |
| Microsoft connector certification | Microsoft reviews and tests this connector before listing it as a certified connector. The review covers the connector's function and packaging, not Saifs AI's internal systems. |

**Self-assessed by Saifs AI**

Self-assessments are statements Saifs AI makes about its own practices. They are not independent audits.

<!-- PUBLISH EACH ROW ONLY AFTER IT IS COMPLETED. Until then, keep the row inside this comment. Add the public link when you have it.

| Self-assessment | What it is | Where to read it |
| --- | --- | --- |
| Microsoft Publisher Attestation | Saifs AI's answers to Microsoft's questionnaire on security, data handling, and compliance. Microsoft publishes the answers and does not verify them. Renewed every year. | [link to the Microsoft app compliance page for this offer] |
| CSA STAR Level 1 | Saifs AI's completed Cloud Security Alliance questionnaire (CAIQ), published on the public STAR Registry. Updated every year. | [link to the STAR Registry entry] |
| CSA STAR for AI Level 1 | Saifs AI's completed AI questionnaire (AI-CAIQ) on the same registry. | [link to the STAR Registry entry] |

-->

| Design fact | Detail |
| --- | --- |
| Customer booking rows stored | Saifs AI states that hosted rows are not stored after the request. The local engine keeps the file in your sandbox. Confirm the hosted claim with the contact below if your review requires a written statement. |
| Passwords held | None. Sign-in is Microsoft single sign-on. |
| Write access to your systems | None |
| Local engine | The script is returned in full for review before it runs. It needs `numpy`. |

**Independent audits.** Saifs AI does not yet hold an independently audited certificate such as SOC 2 or ISO 27001. If your review needs a completed security questionnaire, request it through the [contact page](https://saifs.ai/contact-us).

## Responsible use

This connector recommends a price and an overbooking limit for perishable capacity. A person decides what to publish. It does not give financial, legal, or clinical advice.

Booking exports can contain personal data. The calculation needs counts, dates, prices, classes, and segments. It does not need a guest name. Send a pseudonymous product or segment code where you can.

For clinics, price increases are off unless you set `allow_price_increase` to true. Overbooking is not applied to a critical appointment type. Do not use a result to decide clinical priority, treatment, or who may be refused care. The overbooking figure is an operational capacity limit only, and it always includes the chance of turning someone away.

## Token usage and cost

With the hosted tool, the rows are part of the tool call, so token use grows with the file. With the local engine, the agent receives a script and the rows stay in the sandbox, so token use does not grow with the file.

<!-- TODO Saifs AI: measure tokens before publishing figures. Use one named tokenizer on three sizes: a small file, the hosted limit (500 products or 20,000 bookings or 5,000 price-history rows), and a larger file. Count both directions. Include the script, the write into the sandbox, and the printed output. Record input and output tokens, a dated reference price per million, the first-run and later-run multipliers, and a monthly example. State where the hosted tool uses fewer tokens. Do not copy another product's table. -->

Until those figures are measured, this page does not state a token count or a dollar cost.

**Which route to choose, before the token table exists**

- **Within the hosted limits, and a single question:** the hosted tool. It is one call.
- **Above 500 products, 20,000 bookings, or 5,000 price-history rows:** the local engine. It is the only route.
- **Booking data must stay in your environment:** the local engine. Turn the hosted tool off.
- **The same large file, many times in one session:** the local engine, if the sandbox keeps the script.

**Hosts that bill per action.** Some hosts charge a fixed amount per tool call and do not bill by token. On those hosts, a smaller token count may not reduce the bill. The local engine may also use one extra action to run the script. Choose it there for file size and for where the data stays, not for a lower token bill.

## Prerequisites

- A Saifs AI account, created by signing in with Microsoft single sign-on
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Access to Copilot Studio, Microsoft 365 Copilot, Azure AI Foundry, Power Automate, Power Apps, or Azure Logic Apps
- Capacity data with a product, capacity, units sold, a price, and days to the date. Bookings, price history, fare classes, and show history give a fuller answer.
- For the local engine: code execution enabled on the agent host, and `numpy` available there

## How to get credentials

No keys or secrets are needed. When you create a connection, you are redirected to sign in with your Microsoft account and approve access. If the connection later expires, sign in again from the connection page.

## Get started with your connector

1. In Copilot Studio, open your agent and go to **Tools**, then **Add a tool**.
2. Search for **Dynamic Pricing & Overbooking** and select it.
3. Create a connection and sign in.
4. Add this to the agent's instructions: "For any question about price, booking pace, protection levels, booking limits, or overbooking, call the Dynamic Pricing & Overbooking tool. Do not set the price, the protection level, or the overbooking limit yourself."
5. Provide capacity by pasting it, uploading it, or pointing the agent at a file. Include bookings and price history if you have them.
6. Ask in plain language, for example "What price should we charge for this weekend?" or "How many rooms should we overbook?"

Treat the result as decision support. Check it against your rate policy before you publish a price or a booking limit.

## For administrators

Enabling this connector gives the organization one approved way to answer what price to charge and how much to overbook, inside the agents your makers already build.

**Why this is an administrator decision.** Without a calculation tool, makers ask an agent to pick a rate from a spreadsheet. A language model can give a different figure on each run, and the result cannot be compared week to week. A governed connector uses one method for every agent and team.

- **You choose who can connect it.** Tenant and environment permissions apply. Only users who can create a connection can call the tools.
- **You keep your system of record.** The connector recommends and explains. It does not publish a rate or write back to any system.
- **You can check the number.** The same file and settings can be run again. A figure the engine cannot support is withheld.
- **You control where data goes.** Turn off the hosted tool per agent to keep booking data in your environment.
- **You set data policy as usual.** Place the connector in the Business, Non-business, or Blocked group with Power Platform data loss prevention policies.
- **There are no secrets to rotate.** Expired connections are renewed by signing in again.

**Review checklist**

- Decide whether the hosted tool is allowed, or the local engine only
- Assign the connector to the correct data policy group
- Review the privacy policy and terms
- Read the self-assessments listed under Security and compliance, once they are published
- Read the local engine script if your policy requires it
- For clinic use, confirm that critical types are marked and that price increases stay off unless you intend them

## For developers

No field is required. Send the columns the business has. Each calculation runs only when its inputs are present.

### Product fields

One object per product and date. Up to 500 objects on the hosted tool.

| Field | Meaning | Needed for |
| --- | --- | --- |
| `product` | Product code, such as a room type, flight, or ticket tier on a date | Everything |
| `capacity` | Units available | Price, protection levels, overbooking |
| `sold` | Units already sold | Remaining capacity |
| `days_out` | Days until the stay or event | Booking pace and the price |
| `price` | Price now | The band around the recommendation |
| `price_floor`, `price_ceiling` | Lowest and highest price you will accept | Keeps dynamic pricing inside your policy |
| `unit_cost` | Cost of one unit | Contribution, not only revenue |
| `segment` | Group, such as leisure or corporate | Show rate and pace cuts |
| `show_rate` | Share who show, if you already know it | Overbooking when show history is thin |
| `denied_cost` | Cost of turning one customer away | The overbooking limit |
| `no_show_fee` | Amount kept when a customer does not show | The overbooking tradeoff |
| `critical` | Urgent or critical type. Overbooking is not applied. | Clinic and similar slots |
| `elasticity` | Your own price elasticity for this product | Used as given, not estimated |
| `expected_remaining_demand` | Demand still to come, if a forecast already exists | Price when you are not using the pickup forecast |

The stay or event date can be sent instead of `days_out`. The exact key is still to be confirmed. See the note in [Where the data comes from](#where-the-data-comes-from).

### Fare classes (optional)

Up to 2,000 objects. Fields: `product`, `class`, `fare`, `mean_demand`, `sd_demand`. `mean_demand` and `sd_demand` are demand still to come. Put `capacity` on the row, or pass the `capacity` option when there is no product row. This is the input for Littlewood and EMSR protection levels.

### Price history (optional)

Up to 5,000 objects. Fields named by the tool: `product`, `date`, `price`, and units sold. This is the input for price elasticity. You need at least `min_price_points` distinct prices, default 3.

### Bookings (optional)

Up to 20,000 objects. Confirmed names: `product`, `booking_date` or `lead_time`, `status`, `class`, `price`, `segment`, `units`. Also send the stay or event date and, when you have it, the cancel date. Those two keys are named in words in the tool description. See the TODO above. Set `count_cancellations` to true only when the limit is set far ahead and a cancellation should count as a no-show. The default is false.

### Show history (optional)

Up to 2,000 objects. Fields: `segment`, `booked`, `showed`. This is the input for show rates.

### Options and defaults

| Option | Default | Purpose |
| --- | --- | --- |
| `industry` | `hotel` | One of `hotel`, `airline`, `event`, `rental`, `restaurant`, `clinic`, `other`. Clinic turns price increases off. |
| `as_of` | Latest booking date | Plan date, `YYYY-MM-DD` |
| `currency` | `INR` | Label on money figures |
| `currency_symbol` | None | Symbol shown with those figures |
| `max_price_change_pct` | 15 | Largest move from the current price, from 0 to 50 |
| `min_price_points` | 3 | Minimum distinct prices before elasticity is fit. From 2 to 50. |
| `default_elasticity` | None | Used only when you set it. Flagged as assumed. From -10 to 0. |
| `allow_price_increase` | true, and false for `clinic` | Set false to block a higher price |
| `pace_alert_pct` | 15 | Pace gap, in percent, that is flagged in the result. From 1 to 100. |
| `count_cancellations` | false | When true, cancellations count as no-shows |
| `denied_cost` | None | Cost of one turn-away, if not set on the product |
| `max_denied_prob` | None | Cap on the chance of turning anyone away, from 0 to 0.5, if you set it |
| `max_overbook_pct` | None | Cap on overbooking as a share of capacity, from 0 to 1, if you set it |
| `capacity` | None | Capacity for fare classes that have no product row |
| `max_products_listed` | 200 | How many products are listed in the result. From 1 to 5,000. |

<!-- TODO Saifs AI: confirm that currency is a label only and that no conversion runs. The schema does not say. -->

### Sample request

```json
{
  "industry": "hotel",
  "currency": "INR",
  "as_of": "2026-11-16",
  "max_price_change_pct": 15,
  "products": [
    {
      "product": "DELUXE-2026-11-21",
      "capacity": 40,
      "sold": 28,
      "days_out": 5,
      "price": 8000,
      "price_floor": 6000,
      "price_ceiling": 12000
    }
  ]
}
```

This request uses only field names given as keys in the tool schema. It is enough to ask for a price. It will not produce booking pace, price elasticity, protection levels, or an overbooking limit, because those inputs are not in the call.

### Sample response (shortened)

A signed-in call was not available while this page was written. A call to `https://revenue-optimizer-mcp.saifs.ai/mcp` without a token returned HTTP 401, `unauthorized`, "Missing or invalid authorization header." No response body is shown here, because it was not returned.

<!-- TODO Saifs AI: replace this note with a shortened real response from revenue_optimizer. Include one case where price elasticity or overbooking is withheld, and quote the field that says why. -->

The tool states that decision cards come first. The other results are listed in the next table. Each one is returned only when its inputs were sent.

### Response sections

| What comes back | Use it for |
| --- | --- |
| Decision cards | The action to consider, in plain language. Read these first. |
| Booking pace and the pickup forecast | Whether the date is ahead or behind, and demand still to come, with a range |
| Recommended price | The price with the highest expected contribution, inside the floor, ceiling, and maximum change |
| Price elasticity | How units have moved with price, or the reason it was refused |
| Protection levels and booking limits | How much to hold for higher fares, and how much each class may sell |
| Overbooking limit and turn-away chance | How far to overbook, read next to the chance of turning someone away |
| Show rates | The rate used for each segment, after thin segments are pulled toward the overall rate |
| `revenue_plan.json` | The full local-engine result, written in the sandbox |

<!-- TODO Saifs AI: replace the first column with the JSON keys from a live response. -->

## Known issues and limitations

- **Decision support only.** It does not publish a price, a protection level, or an overbooking limit, and it does not update any system.
- **Not advice.** It is not financial, legal, or clinical advice. Clinic results are operational limits on slots, not a care decision.
- **Hosted tool size.** 500 products, 20,000 bookings, 5,000 price-history rows, 2,000 fare classes, and 2,000 show-history rows. Above 500 products, 20,000 bookings, or 5,000 price-history rows, use the local engine.
- **The tools run on request.** They do not watch the book overnight and they do not send a message. `pace_alert_pct` only flags a gap inside the result of a call. Use a scheduled flow for a daily check.
- **Price elasticity.** Refused when price history is flat, or when demand drove the price. The minimum is 3 distinct prices unless you change `min_price_points`.
- **Price band.** The default maximum change is 15% of the current price, and it cannot be set above 50%. Floor and ceiling apply when you send them.
- **Clinics.** `allow_price_increase` is false for industry `clinic` unless you set it true. Rows marked `critical` are not overbooked.
- **Cancellations.** They count as no-shows only when `count_cancellations` is true. The default is false.
- **Caps you did not set.** `max_denied_prob` and `max_overbook_pct` apply only when you send them. There is no default cap in the schema.
- **Listed products.** The result lists up to `max_products_listed` products, default 200, even if more were sent.
- **Currency.** The default is INR. It labels the figures. This page does not claim a conversion between currencies.
- **Personal data.** Booking exports can identify guests. Send the fields the calculation needs, not a guest name.
- **Regions.** Not available in US Government (GCC, GCC High, DoD) or China (21Vianet) clouds.

<!-- TODO Saifs AI: rate limits, timeouts, and the service region are not in the tool schema. Add them here when confirmed. -->

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| The tool cannot run, or the call returns unauthorized | The connection is missing or expired. A call with no token returns HTTP 401. | Open the connection and sign in again |
| No price elasticity, with a statement that the history cannot support it | The price did not vary, demand drove the price, or there are fewer prices than `min_price_points` | Send history with real price changes, or pass `default_elasticity` and treat it as your assumption |
| No overbooking limit on a clinic row | The row is marked `critical`, or show information was not sent | Leave critical types without an overbooking limit. For other rows, send `show_rate` or show history. |
| The price barely moves | `max_price_change_pct` is 15 unless you raise it, and floor and ceiling still apply | Raise the cap only inside your rate policy. The maximum the schema allows is 50. |
| The agent states a price without calling the tool | The instructions do not require the tool | Add the instruction in Get started |
| The local engine does not run | Code execution is off, or the script was edited | Enable code execution, save the script exactly as returned, and install `numpy` in that sandbox |
| The hosted call rejects a large file | The file is above a `maxItems` limit | Use `revenue_optimizer_get_engine` |

## FAQ

**Do I need a revenue management specialist?**
No. A user asks in plain language. The formulas run inside the tool. Someone who knows your rate policy should still approve the price before it is published.

**What is the least data I need?**
One product row with a product, capacity, units sold, days to the date, and a price. Bookings, price history, fare classes, and show history each unlock a further result.

**Does my data leave my environment?**
Not with the local engine. With the hosted tool, the rows in the call are sent to Saifs AI for the calculation. You can turn the hosted tool off.

**Will it change anything in my systems?**
No. It recommends a price, a protection level, and an overbooking limit. It does not write them back.

**We already have our property system connected to an agent. Why add this?**
That connection gives the agent the system's records. This connector adds the yield management calculation: the price, the protection levels, and the overbooking limit. The property system remains where the rate is published.

**Why do I get the same answer every time?**
The price and the booking limits come from fixed formulas. The same input and settings return the same numbers. The sentences around the numbers can vary.

**What happens if my data is incomplete?**
The tool still runs the steps it can support. Price elasticity is refused when the history cannot support it. Overbooking is withheld for a critical type. Nothing is filled in with a silent guess. A stand-in elasticity is used only when you pass `default_elasticity`, and it is flagged as assumed.

**Can I check how a result was produced?**
Yes. Read the decision cards, then the protection levels, the price band, and the turn-away chance. On the local engine, the full result is in `revenue_plan.json`.

**How large a file can it handle?**
The hosted tool accepts up to 500 products, 20,000 bookings, and 5,000 price-history rows. Larger files use the local engine.

**Can it run on a schedule?**
Yes, by calling it from a scheduled Power Automate flow. The connector itself does not run on a timer and does not send messages.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment). Microsoft Premium connector licensing applies separately.

## Support

**Dynamic Pricing & Overbooking - Saifs AI** is published by Saifs AI.

- Publisher: Saifs AI
- Website: [https://saifs.ai](https://saifs.ai)
- Learn more: [https://saifs.ai/dynamic-pricing-overbooking](https://saifs.ai/dynamic-pricing-overbooking)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Privacy: [https://saifs.ai/privacy_policy/dynamic-pricing-overbooking](https://saifs.ai/privacy_policy/dynamic-pricing-overbooking)
- Terms: [https://saifs.ai/eula](https://saifs.ai/eula)
- Sign-in: Microsoft single sign-on (OAuth 2.0). No API keys.
- MCP endpoint: `https://revenue-optimizer-mcp.saifs.ai/mcp`
