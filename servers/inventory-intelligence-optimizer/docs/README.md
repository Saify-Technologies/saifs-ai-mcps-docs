# Inventory Intelligence & Optimization - Saifs AI

**Inventory Intelligence & Optimization - Saifs AI** helps purchasing and operations teams decide **what to buy, how much, and when**. You send a stock list, and sales history when you have it. It returns order quantities, reorder points, safety stock, overstock flags, cash tied up in inventory, and a buying list ranked by money impact.

It is built for teams that run inventory from spreadsheets, accounting software, or an ERP whose planning module is not set up, and who need clear, repeatable buying decisions they can act on today.

**Publisher:** Saifs AI
**Website:** [https://saifs.ai](https://saifs.ai)
**Learn more:** [https://saifs.ai/inventory-intelligence](https://saifs.ai/inventory-intelligence)
**Pricing:** [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
**Contact and help:** [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
**Privacy:** [https://saifs.ai/privacy_policy/inventory-intelligence](https://saifs.ai/privacy_policy/inventory-intelligence)
**Terms:** [https://saifs.ai/eula](https://saifs.ai/eula)
**Sign-in:** Microsoft single sign-on (OAuth 2.0). No API keys.

| If you are | Start with |
| --- | --- |
| A business owner, purchasing or operations lead | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [Where the data comes from](#where-the-data-comes-from) |
| An administrator or security reviewer | [Data handling and security](#data-handling-and-security), [Security and compliance](#security-and-compliance), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [Where the data comes from](#where-the-data-comes-from), [For developers](#for-developers) |
| A finance or audit reviewer checking the method | [Inside the calculation](#inside-the-calculation) |

## The problem it solves

Most teams still reorder from habit, or from a fixed reorder level that is wrong for half the year. Overstock locks up cash and can spoil. Understock loses sales. The inventory formulas that solve this have existed for decades, but they usually sit inside planning modules that many businesses never set up.

Inventory Intelligence makes that calculation available to any AI agent:

- **What to order and how much**: recommended order quantities using economic order quantity (EOQ) when cost inputs are available.
- **When to reorder**: reorder points with safety stock for a chosen service level. The default is 95%.
- **What is wrong in the warehouse**: overstock and understock, ranked by cash impact.
- **What to buy first under a budget**: a purchase list sorted by money, grouped by supplier if you want.
- **Perishables and short selling windows**: quantities that balance the cost of running out against the cost of leftovers.
- **Real purchasing rules**: open purchase orders, minimum order quantity, case packs, maximum order quantity and price breaks, when you supply them.

## Why choose it: the same answer every time

Inventory Intelligence is deterministic. The same stock list and sales history, with the same settings, produce the same order quantities, reorder points, overstock flags and buy list. Results are calculated from established inventory models. They are not written afresh by a language model each time you ask.

That matters because purchasing and finance act on cash and units. They need a recommendation they can rerun, audit and compare week to week. The agent's wording around the numbers may vary. The numbers do not.

| What it does | Established method |
| --- | --- |
| Recommends how much to order when order cost and holding cost matter | Economic order quantity (EOQ) |
| Sizes one-off, seasonal or perishable buys so you cover demand without excess | Newsvendor (critical fractile) model |
| Sets when to reorder and how much buffer to hold for a service level | Safety stock and reorder point models |
| Flags overstock and understock, ranked by cash tied up or at risk | Exception reporting against those optimal levels |
| Respects purchasing constraints: open orders, minimum quantities, case packs, budget | The same formulas, applied within your constraints |

**It does not guess.** When sales history is too thin to support a safety stock, the tool does not calculate one and says why. Every item also carries its working, step by step, so a number can be checked.

**It recommends and does not act.** It never places a purchase order and never writes to an ERP, accounting system or Dataverse.

## Who should use this

| Role | How it helps |
| --- | --- |
| **Purchasing and inventory managers** at distributors and wholesalers | Which products to reorder now, which slow movers are tying up cash, and a buy list grouped by supplier |
| **Retail and store owners** | Replaces gut-feel reorder levels with quantities and reorder points based on demand and lead time |
| **E-commerce and D2C operations** | Sizes seasonal and campaign buys, such as a festival window, from demand and margin |
| **Manufacturers and spare-parts planners** | Balances order cost against holding cost, and sets safety stock for critical parts |
| **Pharmacy and perishable goods buyers** | Order quantities that account for shelf life, so you cover demand without excess expiry |
| **Business owners and finance leads** | Shows cash stuck in overstock, and ranks purchases when budget is limited |
| **Agent makers and developers** | Adds a tested inventory calculation to an agent, so nobody has to build or maintain the formulas |

**Best fit:** distributors, wholesalers, retailers, e-commerce sellers, manufacturers and pharmacies that hold stock and decide order quantity and timing regularly.

**Not required:** a planning module, a data science team, or an ERP. A stock file is enough to start, and sales history makes the answer sharper.

## Use cases

Each use case below can be built as an agent. The agent types are listed in [Agents you can build](#agents-you-can-build).

**1. Daily or weekly buying list**
Scenario: a purchasing manager uploads on-hand stock and, if available, sales history.
Outcome: a ranked buy list showing what to order today and how much, with stock already on order taken into account.
Ask: "What should I order this week?"

**2. Reorder points and stockout prevention**
Scenario: lead times and service levels differ by product.
Outcome: reorder points and safety stock per item, and a clear list of items now below their reorder point.
Ask: "Reorder points for these items at 99% service level."

**3. Free cash from overstock**
Scenario: slow movers sit for months while fast movers run out.
Outcome: overstock flags with the cash tied up in each, ranked so finance and purchasing can pause the worst first.
Ask: "Which products are overstocked, and how much cash is stuck?"

**4. Budget-constrained purchasing**
Scenario: there is a fixed purchase budget for the week or month.
Outcome: orders prioritised within `purchase_budget`, with the risk left uncovered stated.
Ask: "Build a buy list under a budget of 500,000, grouped by supplier."

**5. Seasonal and campaign buys**
Scenario: a festival or promotion with a limited selling period.
Outcome: newsvendor order quantities from sale price, cost, salvage value and demand uncertainty.
Ask: "How many units should I buy for the festival season?"

**6. Pharmacy and perishable ordering**
Scenario: items expire or have a short shelf life.
Outcome: quantities that account for the selling window and leftover value.
Ask: "What should I order for items expiring within 30 days?"

**7. One purchase order per supplier**
Scenario: many products come from the same vendor.
Outcome: a buying list grouped by `supplier`, with minimum order quantity and case-pack rounding applied.
Ask: "Prepare this week's order for each supplier."

**8. Compare habit with recommendation**
Scenario: the team orders a fixed quantity every cycle.
Outcome: pass `current_order_qty` to see today's habit against the recommended quantity and the yearly cost difference.
Ask: "Are we ordering the right amounts?"

**9. Large catalogues**
Scenario: more than about 200 products or 2,500 sales rows.
Outcome: the same calculation runs inside the agent's own sandbox through the local engine, and the data stays there.
Ask: "Analyse this 10,000-row stock export."

## Agents you can build

Each agent is one agent in your agent host with this connector added as a tool. The last column shows what else to add.

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Reorder agent | Answers what to order today and how much, with reorder points per item | A stock file upload, or a data source from [Where the data comes from](#where-the-data-comes-from) | 1, 2 |
| Overstock and cash agent | Ranks slow stock by the cash it holds, for finance and purchasing | The same data source | 3 |
| Purchase budget agent | Fits the buy list to a fixed budget and states the risk left uncovered | The same data source | 4 |
| Seasonal and perishable buying agent | Sizes festival, campaign and short shelf-life buys | Sale price, shelf life and salvage value in the stock file | 5, 6 |
| Supplier order agent | Produces one draft order per supplier, respecting minimum order quantity and case pack | An email, Teams or ERP tool, if the draft should be sent for approval | 7 |
| Purchase review agent | Compares current ordering habits with the recommended quantities | Current order quantities in the stock file | 8 |
| Large-catalogue agent | Runs the local engine on a full export of thousands of products | Code execution on the agent host | 9 |
| Forecast-to-order agent | Turns forecasts into reorder points and order quantities | A demand forecasting tool, passed in through the `forecasts` option | |
| ERP buying agent | Reads stock and sales from the ERP, calculates the buy list, and hands it back for a person to approve | The ERP's own MCP server | 1, 7 |
| Scheduled buy-list agent | Sends a daily or weekly buy list without anyone asking | A Power Automate flow with a recurrence trigger, and email or Teams | 1 |

The tools run on request. Scheduling comes from the flow, not from the connector.

## At a glance

| Item | Detail |
| --- | --- |
| Tools | `inventory_optimizer` (hosted calculation) and `inventory_optimizer_get_engine` (local engine) |
| Minimum input | One field per item: `sku` |
| Hosted tool size | Up to about 200 products and 2,500 sales rows per call |
| Larger files | Local engine, runs in your agent's code sandbox, needs only `numpy` |
| Writes to your systems | None |
| Engine version | Returned in every response as `metadata.model_version` |

## Where the data comes from

Inventory Intelligence does not connect to your systems. The agent brings the data to it, from any source that can supply stock and sales rows.

| Data source | How the agent gets the data | Best route |
| --- | --- | --- |
| Spreadsheet or CSV export from accounting, inventory, e-commerce or point-of-sale software (for example Tally, QuickBooks, Zoho Inventory, Shopify) | The user uploads the file to the agent | Local engine for large files, hosted tool for small ones |
| An ERP that has its own MCP server (for example Dynamics 365 Finance and Supply Chain Management, Business Central, SAP, NetSuite) | The agent calls the ERP's MCP tools to read items, stock levels, open purchase orders and sales lines, then passes the rows on | Hosted tool up to about 200 products. For more, export to a file and use the local engine. |
| Dataverse tables | The Dataverse MCP server or the Dataverse connector | Same as above |
| SQL database, Excel workbook online, or SharePoint list | A Power Platform connector added to the same agent or flow | Same as above |
| A scheduled flow | A Power Automate flow reads the source on a schedule and calls this connector | Hosted tool |
| A demand forecasting tool | Pass its forecast records in the `forecasts` option | Either |

The connector needs only rows of data, so any other tool that returns stock and sales records can feed it in the same way.

**Works alongside your ERP's MCP server.** An ERP's MCP server gives the agent the ERP's records and functions. This connector adds the ordering calculation, with no planning module to configure. The ERP supplies the data, Inventory Intelligence calculates what to buy, and the ERP or a person creates the purchase order.

**What to pull from the source**

| Data in your system | Field it fills | Why it matters |
| --- | --- | --- |
| Item or product master | `sku`, `name`, `category`, `unit_cost`, `sale_price` | Identifies the item and values the stock |
| Stock levels | `on_hand` | Shows what is on the shelf today |
| Open purchase orders | `on_order` | Stops the buy list repeating stock already on its way |
| Sales orders not yet shipped | `backorders` | Counts stock already promised |
| Sales or issue lines | Sales history: `sku`, `date`, `quantity` | Gives demand and its variation, which unlocks reorder points and safety stock |
| Supplier terms | `supplier`, `lead_time_days`, `moq`, `case_pack`, `price_breaks` | Produces quantities a supplier will accept |

**Column names are recognised as they are.** You do not need to rename columns. Common headers are matched to the right field, and the result lists every match under `data_quality_report.column_mapping`.

| Field | Headers recognised (examples) |
| --- | --- |
| `sku` | Item Code, Product Code, Stock Code, Material, Part No, Barcode |
| `unit_cost` | Cost Price, Purchase Price, Landed Cost, Valuation Rate, Standard Cost |
| `on_hand` | Closing Stock, Closing Balance, Stock on Hand, SOH, Available Qty |
| `on_order` | Open PO, PO Qty, Inbound, In Transit |
| `lead_time_days` | Lead Time, Supplier Lead Time, Delivery Days |
| `supplier` | Vendor, Vendor Name, Party |
| `moq`, `case_pack` | Min Order Qty; Pack Size, Units Per Case, Carton Qty |
| `sale_price` | Selling Price, Retail Price, List Price, MRP |
| Sales `date` | Invoice Date, Bill Date, Voucher Date, Posting Date, Order Date |
| Sales `quantity` | Sold Qty, Units Sold, Sales Qty, Issued Qty |

**File formats for the local engine.** CSV separated by comma, semicolon, tab or pipe, plus JSON and JSON Lines. Stock and sales can be two files or one combined file. Save an Excel sheet as CSV first, or ask the agent to convert it in the sandbox.

## How it works

The connector offers two routes to the same calculation. The agent picks the route by the size of the data.

| | Hosted tool | Local engine |
| --- | --- | --- |
| Tool name | `inventory_optimizer` | `inventory_optimizer_get_engine` |
| Use when | Up to about 200 products and 2,500 sales rows | Anything larger, or when data must stay in your environment |
| What the agent sends | The stock rows and sales rows | File paths and settings only |
| What comes back | The full result | A ready-to-run Python script |
| Where the calculation runs | Saifs AI endpoint | Your agent's code-execution sandbox |
| Where your inventory data goes | To the Saifs AI endpoint for calculation | Nowhere. It stays in the sandbox. |
| Requires | Nothing extra | An agent host with code execution, and `numpy` |

**Local engine, step by step**

1. The user uploads or points the agent to a stock file.
2. The agent calls `inventory_optimizer_get_engine` with the file path and any settings.
3. The connector returns the calculation script.
4. The agent runs the script in its sandbox against the file.
5. The script prints the buying decisions and writes full detail to `inventory_optimizer_result.json` in the sandbox.
6. The agent reports the decisions to the user.

### Inside the connector

1. The user asks the agent a stock or buying question.
2. The agent chooses a tool and calls the connector's single operation, which passes the call to the Saifs AI MCP server over HTTPS at `https://inventory-optimizer-mcp.saifs.ai/mcp`.
3. The server checks the signed-in account.
4. The hosted tool runs the engine on the rows and returns the result. The engine tool returns the same engine as a script, for the agent to run in its sandbox.
5. The agent reads the result and explains it in plain language. The numbers come from the engine. The wording comes from the agent.

### Inside the calculation

Both routes run the same engine. It works through the stock list in fixed steps, and each response lists which steps ran under `metadata.layers_run`.

| Step | What the engine does | Method |
| --- | --- | --- |
| 1. Clean the data | Matches column headers to fields, removes rows with no product code and duplicate codes, and reads numbers written with currency symbols, commas or percent signs. Everything it changed is listed in `data_quality_report`. | Rule-based matching |
| 2. Estimate demand | Uses the first source available for each item: the annual demand you supply, then a forecast you pass in, then your sales history. From history it takes average daily demand and its variation, counting days with no sale as zero. | Mean and standard deviation of daily demand |
| 3. Order quantity | Balances the cost of placing orders against the cost of holding stock. | Economic order quantity: the square root of (2 × annual demand × order cost ÷ holding cost per unit per year) |
| 4. Supplier rules | Rounds up to whole case packs, raises to the minimum order quantity, caps at the maximum, and compares total yearly cost across price breaks. | Constraint rules and total-cost comparison |
| 5. Safety stock | Sizes the buffer for the service level you chose, from variation in demand and in lead time. | z × the square root of (lead time × demand variation² + daily demand² × lead-time variation²), where z comes from the service level |
| 6. Reorder point | The stock level at which to order. | Daily demand × lead time + safety stock |
| 7. Perishable and seasonal items | Sizes a one-period buy from the cost of running short against the cost of leftovers. | Newsvendor: order up to the demand level at the critical fractile, shortage cost ÷ (shortage cost + leftover cost) |
| 8. Stock position | Compares on hand + on order − backorders with the reorder point and with a ceiling of reorder point + order quantity. Marks each item reorder now, healthy or overstock. | Comparison against the two levels |
| 9. Cash | Values the units above the ceiling as cash tied up, and the expected shortfall as yearly lost sales, at margin when sale price is known and at cost otherwise. | Excess units × unit cost; expected units short × orders per year × margin |
| 10. Buy list | Ranks orders by the sales at risk, fits them to a budget if you set one, and groups them by supplier. | Ranking by avoided stockout cost per unit of spend |
| 11. Decisions | Turns the results into plain-language actions, each with its reason, its worth and a confidence level. | Fixed templates filled with the calculated numbers |

**Why the answer does not change.** Every step is a fixed formula with no language model involved. The one step that samples data, the range around the portfolio cash figures, uses a fixed starting seed, so it also repeats exactly.

**What happens when data is missing.** The engine uses a stated default and records it under `assumptions`: order cost 250, lead time 7 days, service level 95%, holding cost 25% of unit cost per year. When an item has no unit cost or no demand figure, it is marked `insufficient_data` and not scored. When sales history is shorter than 8 records across 60 days, safety stock is not calculated for that item.

**Each item carries its working.** The `explanation_trace` for an item states the demand used, the order quantity formula with its numbers, the supplier rules applied, and every default that was assumed.

## Data handling and security

| Question | Local engine | Hosted tool |
| --- | --- | --- |
| Does inventory data reach Saifs AI? | No | Yes, the rows passed in the call |
| Where is it processed? | Your agent host's code sandbox | `inventory-optimizer-mcp.saifs.ai`, over HTTPS |
| What happens to the data after the call? | It never left your sandbox | It is processed in the request and discarded. The service is stateless and does not store your stock or sales rows. |
| What does Saifs AI record? | Only that the engine script was requested | For audit and billing: the time, the signed-in account, the tool called, and a hash of the request and result. Not the rows themselves. |

**What "sandbox" means.** The sandbox is the code-execution environment of your own agent host, such as Copilot Studio or Azure AI Foundry. It belongs to your tenant, not to Saifs AI, and your host's data policies continue to apply to it.

**Use the local engine only.** If your policy requires that inventory data never leaves your environment, turn off the hosted tool. In Copilot Studio, open the agent's **Tools** tab, select the Inventory Intelligence server, turn off **Allow all**, and switch off `inventory_optimizer`. Leave `inventory_optimizer_get_engine` on. The agent can then use only the local engine, and any tool added to the server later stays off by default. This setting is per agent.

**Sign-in.** Users sign in with Microsoft single sign-on. Saifs AI holds only the account details needed for sign-in and billing. No API key is created, shared or stored.

**No write access.** The connector has no action that creates, changes or deletes records in any ERP, accounting system or Dataverse.

**The script is open to review.** The local engine script is plain Python with a single dependency, `numpy`, and makes no network calls. Your security team can read it before allowing it to run.

## Security and compliance

This section separates what an independent party has reviewed from what Saifs AI states about itself.

**Reviewed by Microsoft**

| Item | What it covers |
| --- | --- |
| Microsoft connector certification | Microsoft reviewed and tested this connector before listing it as a certified connector. The review covers the connector's function and packaging, not Saifs AI's internal systems. |

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
| Customer data stored | None. The hosted tool is stateless, and the local engine keeps data in your sandbox. |
| Passwords held | None. Sign-in is Microsoft single sign-on. |
| Write access to your systems | None |
| Local engine | Plain Python, one dependency, no network calls, open to review |

**Independent audits.** Saifs AI does not yet hold an independently audited certificate such as SOC 2 or ISO 27001. If your review needs a completed security questionnaire, request it through the [contact page](https://saifs.ai/contact-us).

## Token usage and cost

With the hosted tool, token use grows with every row. With the local engine, it is a fixed amount that does not grow with the file, because the rows never pass through the model.

| Stock file | Hosted tool: tokens and cost per run | Local engine: first run | Local engine: each later run | Comparison |
| --- | --- | --- | --- | --- |
| 25 products, 300 sales rows | about 39,000 tokens, about $0.14 | about 90,000 tokens, about $0.35 | about 1,900 tokens, under $0.01 | Hosted tool costs about 2.6 times less |
| 200 products, 2,500 sales rows | about 285,000 tokens, about $1.06 | about 90,000 tokens, about $0.35 | about 1,900 tokens, under $0.01 | Local engine costs about 3 times less |
| 2,000 products, 60,000 sales rows | Not possible. Sending the rows alone is about 1.7 million tokens. | about 90,000 tokens, about $0.35 | about 1,900 tokens, under $0.01 | Local engine only |

Costs use a reference price of $1.25 per million input tokens and $10 per million output tokens, a common list price for a general-purpose model in November 2026. Rows the model sends to the hosted tool are output tokens, the more expensive kind.

**Estimate your own file**

- **Hosted tool:** about 79 tokens per product and 26 per sales row to send, plus about 1,000 tokens per product to read the result.
- **Local engine, first run:** about 90,000 tokens. The agent reads the script, writes it into the sandbox, and reads the printed decisions.
- **Local engine, later runs:** about 1,900 tokens each, for as long as the sandbox keeps the script. At 200 products that is about 380 times less than the hosted tool.

**Which route to choose**

- **A few dozen products:** the hosted tool. It uses fewer tokens and needs one call.
- **Around 60 products or more:** the local engine uses fewer tokens.
- **More than about 200 products or 2,500 sales rows:** the local engine. It is the only route.
- **Repeated runs in one session:** the local engine.

**Hosts that bill per action.** Some agent hosts charge a fixed amount for each tool call or response and do not bill by token. On those hosts the two routes cost about the same, and the local engine may use one extra action. There, choose the local engine for file size and data privacy, not for cost.

How these figures were measured: token counts use the `o200k_base` tokenizer on generated sample data, with rows written as JSON, ten fields per product and about twelve sales rows per product. Counts vary by model and by how many fields you send. The figures reflect the engine as delivered on 16 November 2026, and hosted results are counted at about 1,000 tokens per product, as measured on the engine's result file.

## Prerequisites

- A Saifs AI account, created by signing in with Microsoft single sign-on
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Access to Copilot Studio, Microsoft 365 Copilot, Azure AI Foundry, Power Automate, Power Apps or Azure Logic Apps
- Stock data with at least a product code per item. On-hand quantity, unit cost and sales history give a fuller answer.
- For the local engine: code execution enabled on the agent host

## How to get credentials

No keys or secrets are needed. When you create a connection, you are redirected to sign in with your Microsoft account and approve access. If the connection later expires, sign in again from the connection page.

## Get started with your connector

1. In Copilot Studio, open your agent and go to **Tools**, then **Add a tool**.
2. Search for **Inventory Intelligence** and select it.
3. Create a connection and sign in.
4. Add this to the agent's instructions: "For any question about stock, ordering, reordering or overstock, call the Inventory Intelligence tool. Do not calculate quantities yourself."
5. Provide a stock list by pasting it, uploading it, or pointing the agent to a file. Include open purchase orders if you have them.
6. Ask in plain language, for example "What should I order this week?" or "Which products are overstocked, and how much cash is stuck?"

Treat results as decision support. Check critical items against supplier terms and business policy before placing orders.

## For administrators

Enabling this connector gives the organisation one approved way to answer what to buy, how much, and when, inside the agents your makers already build.

**Why this is an administrator decision.** Without a calculation tool, makers ask an agent to work out order quantities from a spreadsheet. A language model can give a different figure on each run, and the result cannot be audited or compared week to week. A governed connector replaces that with one method for every agent and team.

- **You choose who can connect it.** Tenant and environment permissions apply. Only users who can create a connection can call the tools.
- **You keep your system of record.** The connector recommends and explains. It does not place purchase orders or write back to any system.
- **You can check the number.** Results can be rerun on the same file, and missing inputs are reported, not assumed.
- **You control where data goes.** Turn off the hosted tool per agent to keep inventory data in your environment.
- **You set data policy as usual.** Place the connector in the Business, Non-business or Blocked group with Power Platform data loss prevention policies.
- **There are no secrets to rotate.** Expired connections are renewed by signing in again.

**Review checklist**

- Decide whether the hosted tool is allowed, or local engine only
- Assign the connector to the correct data policy group
- Review the privacy policy and terms
- Read the self-assessments listed under Security and compliance
- Review the local engine script if your policy requires it

## For developers

Only `sku` is required. Send whatever columns the business has. The response reports what was missing and what that cost in accuracy.

### Item fields

| Field | Meaning | Needed for |
| --- | --- | --- |
| `sku` | Product code. Required. | Everything |
| `name`, `category` | Labels | Readable output |
| `unit_cost`, `on_hand` | Cost per unit and units in stock | Cash and overstock figures |
| `on_order` | Units ordered but not yet delivered | Avoiding double-buying |
| `backorders` | Units already promised to customers | Correct stock position |
| `annual_demand` | Units sold per year. Skip if sending sales history. | Order quantity |
| `lead_time_days`, `lead_time_std` | Supplier delivery time and its variation | Reorder point, safety stock |
| `service_level` | Target chance of not running out, 0.5 to 0.9999. Default 0.95. | Safety stock |
| `order_cost`, `holding_cost` | Cost per order; cost to hold one unit for a year | Order quantity |
| `supplier`, `moq`, `case_pack`, `max_order_qty`, `price_breaks` | Purchasing rules | Placeable order quantities |
| `sale_price`, `perishable`, `shelf_life_days`, `salvage_value` | Selling price and expiry details | Perishable and seasonal buys |
| `current_order_qty` | What the team orders today | Savings comparison |

### Sales history (optional)

Rows of `sku`, `date`, `quantity`. The aliases `sale_date`, `sold_qty`, `qty` and `units_sold` are accepted.

### Options and defaults

| Option | Default | Purpose |
| --- | --- | --- |
| `currency`, `currency_symbol` | USD | Display label only. No currency conversion. |
| `as_of` | Latest sales date, or today | Analysis date |
| `purchase_budget` | None | Caps the buy list at a spend limit |
| `group_by_supplier` | true | One draft order per supplier |
| `default_service_level` | 0.95 | Used when an item has none |
| `default_lead_time_days` | 7 | Used when an item has none |
| `default_order_cost` | 250 | Used when an item has none |
| `holding_cost_rate` | 0.25 | Yearly holding cost as a share of unit cost. Results also show the 0.20 to 0.30 band. |
| `min_demand_observations`, `min_history_days` | 8, 60 | Minimum sales history before a reorder point is given |
| `forecasts` | None | Forecast records from a demand forecasting tool |

### Sample request

```json
{
  "items": [
    {
      "sku": "SKU-1001",
      "name": "Widget A",
      "unit_cost": 4.5,
      "on_hand": 120,
      "on_order": 50,
      "annual_demand": 3600,
      "lead_time_days": 14,
      "supplier": "Acme Supply",
      "moq": 100,
      "case_pack": 25
    }
  ],
  "currency": "USD",
  "purchase_budget": 5000,
  "as_of": "2026-11-16"
}
```

### Sample response (shortened)

```json
{
  "status": 1,
  "message": "Inventory optimised successfully",
  "decisions": {
    "summary": "Biggest thing to act on: ...",
    "decisions": [
      { "action": "ADD DATA", "headline": "...", "because": "...", "confidence": "high", "priority": 1 }
    ]
  },
  "metadata": { "model_version": "inventory-optimizer/1.0.0", "as_of_date": "2026-11-16", "refusals": [], "warnings": [] },
  "data_quality_report": { "items_received": 1, "items_used": 1, "items_dropped": 0, "issues": [] },
  "portfolio_summary": { "n_skus": 1, "n_reorder_now": 0, "n_overstock": 0, "total_inventory_value": 540.0 },
  "cash_exposure": { "capital_tied_in_overstock": 0.0, "annual_stockout_cost": 0.0, "headline": "..." },
  "assumptions": { "holding_cost_rate": 0.25, "default_order_cost": 250.0, "default_service_level": 0.95 },
  "skus": [
    {
      "sku": "SKU-1001",
      "status": "partial",
      "confidence": "low",
      "policy": {
        "eoq": 1264.91,
        "order_quantity": 1275.0,
        "order_quantity_rules": ["rounded up to 51 x case pack of 25"],
        "safety_stock": null,
        "reorder_point": null
      },
      "explanation_trace": ["Demand: 3600 units/yr as supplied.", "..."]
    }
  ],
  "purchase_queue": { "queue": [], "draft_purchase_orders": [], "budget": null, "delivery_text": "..." }
}
```

In this sample, `safety_stock` and `reorder_point` are `null` because no sales history was sent. The reason appears in `metadata.refusals`.

### Response sections

| Section | Use it for |
| --- | --- |
| `decisions` | Plain-language actions (order, reduce, add data) with the reason and confidence |
| `cash_exposure` | Cash tied up in overstock and yearly cost of stockouts, with ranges |
| `purchase_queue` | Ranked buy list, draft orders per supplier, and `delivery_text` ready to send |
| `skus[]` | Per-item numbers and `explanation_trace`, the step-by-step working |
| `data_quality_report` | Rows received, used and dropped, and any issues found |
| `metadata` | Engine version, which calculation layers ran, and `refusals` |
| `assumptions` | Every default that was applied |

## Known issues and limitations

- **Decision support only.** It does not place purchase orders or update any system.
- **Hosted tool size.** About 200 products and 2,500 sales rows per call. Use the local engine for more.
- **Thin sales history.** With fewer than 8 sales records across 60 days, safety stock is not calculated for that item, and no buffered reorder point is given.
- **Open orders.** If `on_order` is left out, the buy list may include stock that is already on its way.
- **Currency.** The currency is a display label. There is no conversion between currencies.
- **Purchasing rules.** Minimum order quantity, case pack and maximum quantity apply only when supplied.
- **Holding cost.** When not supplied, it is set at 25% of unit cost per year, and results show the range for 20% to 30%.
- **Multiple warehouses.** Transfer planning between locations is out of scope. Run one call per location.
- **Pharmacy and health use.** Stock decisions only. It gives no clinical, prescribing or substitution advice.
- **Regions.** Not available in US Government (GCC, GCC High, DoD) or China (21Vianet) clouds.

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| The tool cannot run, or asks you to sign in again | The connection has expired | Open the connection and sign in again |
| Items marked REVIEW, with no reorder point | Not enough sales history for those items | Add sales rows, or send `annual_demand` and accept an order quantity without a reorder point |
| The buy list includes stock already ordered | `on_order` was not sent | Include open purchase order quantities |
| The agent gives a number without calling the tool | Agent instructions do not require the tool | Add the instruction shown in Get started |
| The local engine does not run | Code execution is not enabled on the agent host | Enable code execution, or use the hosted tool for small files |
| Items appear under `items_dropped` | Rows had no usable product code | Check `data_quality_report.issues` |

## FAQ

**Do I need a data scientist or an operations research team?**
No. A user asks in plain language and the agent returns a buy list. The formulas run inside the tool.

**What is the least data I need?**
A list of product codes. Stock on hand, unit cost, lead time and sales history make the answer more complete.

**Does my data leave my environment?**
Not with the local engine. With the hosted tool, the rows in the call are sent to Saifs AI for calculation. You can turn the hosted tool off.

**My ERP already has an MCP server. Why add this?**
That server gives the agent your ERP's records and functions. This connector gives the agent the ordering calculation, with no planning module to configure, and it works on data from any source.

**What happens if my data is incomplete?**
The tool still runs. It applies the documented defaults, lists them under `assumptions`, and withholds any figure it cannot support.

**Can it handle thousands of products?**
Yes, through the local engine.

**Can it run every morning automatically?**
Yes, by calling it from a scheduled Power Automate flow.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment). Microsoft Premium connector licensing applies separately.

## Support

**Inventory Intelligence & Optimization - Saifs AI** is published by Saifs AI.

- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Learn more: [https://saifs.ai/inventory-intelligence](https://saifs.ai/inventory-intelligence)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Privacy: [https://saifs.ai/privacy_policy/inventory-intelligence](https://saifs.ai/privacy_policy/inventory-intelligence)
- Terms: [https://saifs.ai/eula](https://saifs.ai/eula)

