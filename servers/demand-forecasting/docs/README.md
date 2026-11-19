<!-- DRAFT for review. Do not replace intro.md until this file is accepted.
This folder has no tool description file. The zip package also has no mcptools JSON. Tool names, defaults, and the 2,500-row / 200-product split below are taken from intro.md. Confirm them against the live server before you publish.

FIELD BLOCK
Title (18 characters): Demand Forecasting
Description (222 characters):
Forecasts what each product will sell next, as a range, from a sales history. The same file and settings return the same outlook. Short history is called out instead of a precise number. It does not place a purchase order.

Operation summary (31 characters): Next period demand with a range
Operation description:
Forecasts what each product will sell next, as a range, from a sales history. The same file and settings return the same outlook.
-->

# Demand Forecasting - Saifs AI

**Demand Forecasting - Saifs AI** helps buyers and planners **see what each product will sell next, as a range**. You send a sales or volume history. It returns a per-product outlook, a range, and a short plan, watch, or hold-back list. The same file and the same settings return the same outlook.

It is built for teams that still plan from last year plus a round number, and that have a date and a quantity to start from.

The limits and defaults on this page come from the current intro. This folder does not include a tool description file. Confirm them on the live server before you publish.

| If you are | Start with |
| --- | --- |
| A buyer, planner, or operations lead | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [Where the data comes from](#where-the-data-comes-from) |
| An administrator or security reviewer | [Data handling and security](#data-handling-and-security), [Security and compliance](#security-and-compliance), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [Where the data comes from](#where-the-data-comes-from), [For developers](#for-developers) |
| A planning reviewer checking the method | [Inside the calculation](#inside-the-calculation) |

## The problem it solves

Most teams answer "what will we sell next?" with last year plus 10 percent, or with a guess. Buying, staffing, and cash then rest on a single number that nobody can rerun.

Demand Forecasting makes that work available to any AI agent:

- **What each product will sell next**: a point and a range. The default range is 95 percent coverage. Do not plan from the midpoint alone.
- **Which pattern was used**: a simple average, a trend, a seasonal pattern, or a lumpy spare-parts rate, chosen by a time-ordered check on your history.
- **What to watch**: a note when recent demand no longer matches the fitted history.
- **What purchasing can reuse**: expected demand over the supplier wait, plus a measure of forecast error. The default wait is 14 days when you do not pass a lead time.

It does not place a purchase order.

## Why choose it: the same history, the same range

The reason to use this connector is that the outlook is fitted to your history. Ask again with the same file and the same settings and you get the same range. The agent does not invent next month's units.

**Short history does not get a precise forecast.** The current intro says a product needs 12 observed periods before a full model. Below that, the result is a wide average and a stated reason, or the product is held back.

**A range is the result.** A single number hides the uncertainty. Use the interval.

**It does not buy anything.** People still place orders in their own systems.

## Who should use this

| Role | How it helps |
| --- | --- |
| **Retail buyers and merchandisers** | A per-product outlook for the next quarter instead of last year plus 10 percent |
| **Distributors** | A split by warehouse or region when that column is in the file |
| **Spare-parts planners** | Lumpy demand scored as gaps and sizes, not as a smooth weekly drip |
| **Production planners** | A unit outlook by product for the next planning window |
| **Store and e-commerce operations** | A weekly outlook, plus a warning when the pattern has shifted |
| **Finance** | Expected units as a range for a cash conversation |
| **Agent makers** | Call a tool instead of averaging the column in the agent |

**Best fit:** teams with a sales or volume history and a need for a next-period range per product. A date and a quantity are enough to start. A product id, a location, a channel, and a lead time make the outlook more useful.

**Not required:** a planning suite or an API key. A Saifs AI account and OAuth 2.0 sign-in are required.

This connector forecasts. It is not an inventory buyer. Pair the handoff with a separate inventory connector when you also need an order quantity.

## Use cases

**1. Next-quarter buy by product**
Scenario: weekly sales by product are in a sheet.
Outcome: a forecast and a range per product. The default horizon is 13 periods. On a weekly grain, that is about one quarter.
Ask: "What will each product sell over the next 13 weeks?"

**2. Split by warehouse**
Scenario: the export has a godown, warehouse, or store column.
Outcome: each product at each location is its own series.
Ask: "Forecast this export by warehouse."

**3. Spare parts**
Scenario: the log only has a row when something sold, with long gaps.
Outcome: the current intro says those series are routed to a lumpy-demand method, not a smooth weekly curve.
Ask: "Which parts are lumpy, and what demand should we plan?"

**4. Demand over a supplier wait**
Scenario: each product has its own lead time.
Outcome: expected demand over that wait, plus forecast-error size. Without a lead time, every product uses 14 days.
Ask: "Give me demand over each product's lead time."

**5. The pattern has changed**
Scenario: a launch, a price change, or a stock-out may have broken the old curve.
Outcome: a regime alert when the recent history does not match the fit.
Ask: "Has demand for this product shifted?"

**6. Ticket or contact volume**
Scenario: the history is a count of tickets by day or week, not units sold.
Outcome: the same tools, with quantity meaning volume. This connector does not compute headcount.
Ask: "Forecast the next 13 weeks of contact volume."

## Agents you can build

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Buy-plan agent | Returns a range per product for the next quarter | A sales history. Do not rename columns before you send them | 1 |
| Location agent | Splits the outlook by warehouse or region | A location column left in the file | 2 |
| Spare-parts agent | Keeps lumpy series on a lumpy method | The sparse log, as it is | 3 |
| Lead-time agent | Hands purchasing a demand-over-wait figure | `series_meta` with a product id and `lead_time_days` | 4 |
| Shift-watch agent | Surfaces products whose pattern broke | The same history. Read the alerts | 5 |
| Large-catalog agent | Forecasts a file past about 2,500 rows or 200 products | A sandbox with numpy. The current intro says the rows stay in the sandbox | Any |
| Scheduled plan agent | Refreshes the outlook on a cadence | A flow with a recurrence trigger. A person still places the order | 1 |

The tools run on request. This connector does not write a purchase order.

## At a glance

| Item | Detail |
| --- | --- |
| Tools | `demand_forecaster` (about 2,500 rows or 200 products or fewer). `demand_forecaster_get_engine` (larger) |
| Minimum input | A date and a quantity |
| Defaults | Weekly grain. Horizon 13. Lead time 14 days. Minimum 12 observed periods. Interval 0.95. Three backtest folds |
| Writes to your systems | None |
| Same file, next run | Same outlook, if the settings match |
| Pricing | [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment) |

## Where the data comes from

You export the rows and send them as they are. The current intro says common header names are already mapped, including voucher date, billed quantity, stock item, and godown. This connector has not been certified against each source system.

| You have | Send | Why it matters |
| --- | --- | --- |
| A date and a quantity | One row per sale or per period | Required |
| More than one product | A product column | Splits the series |
| More than one site | A region, godown, warehouse, store, or branch column | Splits again |
| More than one route to market | A channel column | Splits again |
| Supplier waits | `series_meta` with product id and `lead_time_days` | The handoff uses each product's own wait |

Do not pre-clean the file in the agent. Unreadable quantities and holes in an otherwise complete file are missing, not zero sales. Empty weeks in a log that only records a sale when one happened are genuine zeros. Sending the wrong shape can swap those two readings. Read the data-quality notes.

## How it works

Two routes. The hosted tool takes the rows in the call when the book is within the size in the current intro. The engine tool returns a script. The current intro says no sales rows reach this server on that second path, and that the script needs numpy.

### Inside the connector

1. The user asks what they will sell next and provides a history.
2. At about 2,500 rows or 200 products or fewer, the agent calls `demand_forecaster` at `https://demand-forecaster-mcp-microsoft.saifs.ai/mcp`.
3. Above that, the agent calls `demand_forecaster_get_engine`, saves the script, and runs it. Only the path and option lines at the bottom may change.
4. The agent leads with the decisions and the range. A person still buys.

To keep a large file out of the chat, turn the hosted tool off for that agent and leave `demand_forecaster_get_engine` on: open the agent's **Tools** tab, select this server, turn off **Allow all**, turn off `demand_forecaster`, and leave `demand_forecaster_get_engine` on. Do this on each agent. This matches the current intro's claim that the engine path does not upload the rows. Confirm that claim on the live tool before you rely on it.

### Inside the calculation

The current intro names the candidate methods as a naive carry-forward, simple exponential smoothing, Holt, Holt-Winters, and a lumpy-demand method (Croston / SBA). The choice is a rolling-origin check: time-ordered prefixes, not a random split. Intermittent series are routed to the lumpy method. Seasonal models are candidates only after enough full cycles. The default is two cycles.

| Step | What happens | What to know |
| --- | --- | --- |
| 1. Grid | Rows land on a regular calendar | Default grain is weekly. Daily is often noise. Monthly can hide a weekly reorder cycle |
| 2. Choose | The method with the better time-ordered error is kept | A requested method that does not fit the series is ignored, with a warning |
| 3. Range | A prediction interval around the outlook | Default coverage 0.95. A flat series gets a narrow range |
| 4. Short series | Below 12 observed periods | A wide average and a stated reason, or the product is held back |
| 5. Handoff | Demand over the lead time, plus error size | Default wait 14 days |
| 6. Alerts | Spikes and broken patterns | The current intro says a 100-times spike is flagged and capped |

**What happens when an input is missing.** One series can be forecast from date and quantity alone. Without a product column, the file is one series. Without lead times, every product uses 14 days. The tool does not invent a precise number for a short series.

## Data handling and security

| Question | Hosted `demand_forecaster` | Local `demand_forecaster_get_engine` |
| --- | --- | --- |
| Do sales rows reach Saifs AI? | Yes. They are in the tool call | The current intro says no. Confirm on the live tool. There is no tool file in this folder |
| Where is the work done? | `demand-forecaster-mcp-microsoft.saifs.ai`, over HTTPS | In the sandbox. Numpy, as the current intro states |
| Where is the result stored in your tenant? | In the agent thread, unless you save it | `demand_forecast_result.json` by default, as the current intro states |
| How long does Saifs AI keep a hosted upload? | <!-- TODO Saifs AI: confirm retention and the audit log. --> | Not uploaded, if the intro's claim holds |
| Write access to ERP or purchasing | None | None |

**Sign-in.** OAuth 2.0 through Saifs AI. No API key is created for the maker to paste into the agent.

## Token usage and cost

Prices below use the rates published on 16 November 2026: **$1.25 per million tokens the model reads**, and **$10 per million tokens the model sends**. Rows the model sends are the expensive direction. Counts use the `o200k_base` tokenizer on spaced JSON. Each sample row has a date, a product id, a quantity, and a location. The forecast objects are a representative shape, not a captured live result. A full per-product ledger raises the read cost. The script is not in this folder, so its first-run token cost is not stated. Do not copy another connector's script cost onto this one.

One history row of that shape is about 33 tokens to send.

| File | Hosted tokens and cost | Local first run | Local later run | Comparison |
| --- | --- | --- | --- | --- |
| 200 history rows | About 6,200 sent and 2,100 read. About $0.07 | Script not measured | A short printed outlook is about 20 tokens, under $0.01 | Fine on the hosted tool |
| 2,500 history rows | About 77,500 sent and 2,100 read. About $0.78 | Script not measured | Under $0.01 for the printed outlook | This is the hosted size named in the current intro. The read stays small only if the agent does not paste every series |
| 20,000 history rows | Not the hosted tool. Sending the rows alone is about 660,000 tokens | Script not measured | Under $0.01 for the printed outlook, if the rows stay in the sandbox | Use the local engine |

Hosts that charge a fixed amount per tool call are not changed by these token figures. Choose the local engine for file size and, if the live tool confirms it, for keeping the rows in the sandbox.

## Security and compliance

**Reviewed by Microsoft**

| Item | What it covers |
| --- | --- |
| Microsoft connector certification | Microsoft reviewed and tested this connector before listing it as a certified connector. The review covers the connector's function and packaging, not Saifs AI's internal systems. |

**Self-assessed by Saifs AI**

<!-- PUBLISH EACH ROW ONLY AFTER IT IS COMPLETED.

| Self-assessment | What it is | Where to read it |
| --- | --- | --- |
| Microsoft Publisher Attestation | Saifs AI's answers to Microsoft's questionnaire. Microsoft publishes the answers and does not verify them. | [link] |
| CSA STAR Level 1 | Saifs AI's completed CAIQ on the public STAR Registry. | [link] |
| CSA STAR for AI Level 1 | Saifs AI's completed AI-CAIQ on the same registry. | [link] |

-->

| Design fact | Detail |
| --- | --- |
| Purchase orders placed | None |
| Hosted retention | <!-- TODO Saifs AI: confirm. --> |
| Passwords held | None. Sign-in is OAuth 2.0 |
| Short history | Called out. Not a precise forecast |

**Independent audits.** Saifs AI does not yet hold SOC 2 or ISO 27001. For a security questionnaire, use the [contact page](https://saifs.ai/contact-us).

## Prerequisites

- A Saifs AI account with access to Demand Forecasting
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- OAuth 2.0 sign-in when you create the connection
- A history with a date and a quantity
- For a file past about 2,500 rows or 200 products: a sandbox with numpy
- Access to Copilot Studio, Microsoft 365 Copilot, Azure AI Foundry, Power Automate, Power Apps, or Azure Logic Apps

## How to get credentials

No API key is created for you to copy. Create a connection and sign in with OAuth 2.0. If the connection expires, sign in again from the connection page.

## Get started with your connector

1. In Copilot Studio, open your agent and go to **Tools**, then **Add a tool**.
2. Search for **Demand Forecasting** and select it.
3. Create a connection and complete OAuth 2.0 sign-in.
4. Add this to the agent's instructions: "When the user asks what they will sell or handle next, call the demand tool. Copy rows as they are. At about 2,500 rows or 200 products or fewer, call demand_forecaster. Above that, call demand_forecaster_get_engine and run the script as given. Change only the path and option lines. Lead with the range, not a single number. Do not use last year plus 10 percent. If history is short, say so."
5. Test with a small history of at least 12 periods.

## For administrators

Enabling this connector gives the organisation one approved way to turn a sales history into a rerunnable range.

- **You choose who can connect it.** Tenant and environment permissions apply.
- **You keep purchasing in your own systems.** This connector does not place orders.
- **You can rerun the file.** The same history and settings return the same outlook.
- **You do not get a single number.** The result is a range. Short series are called out.
- **You choose the route for large files.** The current intro says the local script keeps the rows in the sandbox. Confirm that on the live tool.
- **You set data policy as usual.** Place the connector in the Business, Non-business, or Blocked group.
- **There is no API key to rotate.** Expired connections are renewed by signing in again.

**Review checklist**

- Confirm the live tool names and the row cap. This folder has no tool file
- Decide who may upload sales history
- Assign the connector to the correct data policy group
- Review the privacy policy and terms
- Read the self-assessments once they are published

## For developers

Figures in this section are from the current intro, not from a tool schema in this folder.

| Tool | When | Input |
| --- | --- | --- |
| `demand_forecaster` | About 2,500 rows or 200 products or fewer | `history` |
| `demand_forecaster_get_engine` | Larger files | `history_path` when you run the script |

| Field | Meaning | Default in the current intro |
| --- | --- | --- |
| `history` | Date and quantity. Optional product, region, channel | Required for the hosted tool |
| `series_meta` | Product id and `lead_time_days` | Omitted. Then every product uses 14 days |
| `period` | `D`, `W`, or `M` | `W` |
| `horizon` | Periods ahead. Maximum stated as 260 | 13 |
| `min_periods` | Below this, no full model | 12 |
| `interval_confidence` | Range coverage | 0.95 |
| `cv_folds` | Time-ordered checks | 3 |
| `default_lead_time_days` | Wait when a product has none | 14 |
| `intermittency_threshold` | Average gap before a lumpy method | 1.32 |
| `requested_model` | A hint. Ignored when it does not fit | Omitted |

### Sample request

```json
{
  "history": [
    { "date": "2026-01-06", "sku": "SKU-1001", "quantity": 12 }
  ],
  "period": "W",
  "horizon": 13
}
```

One row shows the shape. A full forecast needs about 12 observed periods.

### Sample response (shortened)

<!-- TODO Saifs AI: replace this note with a shortened response from a real call. Do not publish invented units. -->

A live shortened response is not included in this draft. The current intro says the result includes decisions, per-series forecasts with intervals, the method chosen, accuracy, regime alerts, data-quality notes, and an inventory handoff.

## Known issues and limitations

- **No tool file in this folder.** Confirm every limit on the live server.
- **A range, not a promise.** The outlook is a reading of the history.
- **Short series.** Default gate is 12 observed periods.
- **Horizon longer than the history.** Far periods are widened and warned.
- **Seasonal models** need enough cycles. The default is two.
- **Lead time.** Missing waits all become 14 days.
- **Missing versus zero.** Depends on the file shape. Read the data-quality notes.
- **Spikes.** The current intro says a 100-times spike is capped.
- **Size.** About 2,500 rows or 200 products on the hosted tool.
- **No purchase order, no currency conversion, no promo-cause model.**
- **Script.** Run it as given. Numpy. Its token cost is unmeasured.

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| The tool asks you to sign in again | The connection expired | Sign in again with OAuth 2.0 |
| A product has a wide, weak outlook | Fewer than about 12 observed periods | Add history, or accept the wide range. Do not invent zeros |
| Every product has the same wait | No lead times were sent | Pass `series_meta` |
| A lumpy part looks smoothly weekly | The file was filled with zeros it did not contain, or the opposite | Send the export as it is and read the data-quality notes |
| A large file fails in chat | It was pasted into the hosted tool | Use `demand_forecaster_get_engine` |
| The script fails | It was edited above the path lines, or numpy is missing | Run it as given |

## FAQ

**Do I need a planner's model?**
No. Ask what you will sell next and attach the history. A person still places the order.

**What is the least I need to send?**
A date and a quantity.

**Does my data leave my environment?**
On the hosted tool, yes. On the local engine, the current intro says the rows stay in the sandbox. This folder has no tool file to confirm that sentence.

**Will it place a purchase order?**
No.

**We already add 10 percent to last year. Why add this?**
The same history returns the same range, with a stated method and a warning when the series is too short.

**Why might two runs differ?**
They should match when the file and the settings match. A different grain or horizon is a different run.

**What happens if lead times are missing?**
Every product uses 14 days.

**Can I check how a number was produced?**
The result names the method and the backtest. A live trace is not pasted into this draft.

**How large a file can I send?**
About 2,500 rows or 200 products on the hosted tool, as the current intro states. Larger files use the script.

**Can it run on a schedule?**
Yes, from a scheduled flow. A person still places the order.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment). A hosted file of 2,500 rows of the shape measured here is about $0.78 in model tokens at the 16 November 2026 rates. Microsoft Premium connector licensing applies separately.

## Support

**Demand Forecasting - Saifs AI** is published by Saifs AI.

- Website: [https://saifs.ai](https://saifs.ai)
- Learn more: [https://saifs.ai/demand-forecasting](https://saifs.ai/demand-forecasting)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Privacy: [https://saifs.ai/privacy_page](https://saifs.ai/privacy_page)
- Terms: [https://saifs.ai/terms](https://saifs.ai/terms)
- Sign-in: OAuth 2.0. No API key for the maker to paste.
- MCP endpoint: `https://demand-forecaster-mcp-microsoft.saifs.ai/mcp`

<!-- OPEN QUESTIONS
1. No mcptools JSON in this folder or in the updated zip. Confirm tool names, defaults, and the 2,500 / 200 cap on the live server.
2. Confirm the engine path really keeps sales rows off the server.
3. Confirm hosted retention and the audit log.
4. Capture a shortened live response.
5. The script is not in this folder, so its token cost is unmeasured.
6. Confirm excluded government and China clouds.
7. Publisher Attestation and STAR rows stay commented until completed and linked.
8. Privacy URL in this folder is https://saifs.ai/privacy_page and terms are https://saifs.ai/terms.
9. Pricing uses the same Marketplace URL as the other Saifs AI connector pages.
-->
