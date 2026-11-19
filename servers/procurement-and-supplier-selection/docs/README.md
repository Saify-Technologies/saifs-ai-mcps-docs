<!-- DRAFT for review. Do not replace intro.md until this file is accepted.
FIELD BLOCK (Microsoft validated fields). Confirm before submission.

Title (30 characters): Procurement & Vendor Selection
Description (372 characters):
Ranks which supplier to choose, and why, from quotations, delivery history, and spend. Returns a ranked supplier, a consistency check on criteria weights, a bulk-discount total-cost test, late-delivery and defect flags, and a spend class. The same inputs and settings return the same numbers. It recommends a supplier. It does not place a purchase order or award a tender.

Operation summary (32 characters): Procurement and Vendor Selection
Operation description:
Ranks suppliers from quotations, optional delivery history, and spend, and returns a consistency check, a bulk-discount test, and a spend class.
-->

# Procurement & Vendor Selection - Saifs AI

**Procurement & Vendor Selection - Saifs AI** helps purchasing teams decide **which supplier to choose, and why**. You send quotations, criteria, and, when you have them, delivery history and spend. It returns a ranked supplier, a consistency check on the weights, a bulk-discount total-cost test, late-delivery and defect flags, a spend class, and a decision record a committee can rerun.

It is built for teams that choose suppliers from spreadsheets or a purchasing export, and who need a choice they can rerun and compare from one committee to the next.

| If you are | Start with |
| --- | --- |
| A business owner, purchasing or finance lead | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [Where the data comes from](#where-the-data-comes-from) |
| An administrator or security reviewer | [Data handling and security](#data-handling-and-security), [Security and compliance](#security-and-compliance), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [Where the data comes from](#where-the-data-comes-from), [For developers](#for-developers) |
| A purchase committee or audit reviewer checking the method | [Inside the calculation](#inside-the-calculation) |

## The problem it solves

Most teams still choose a supplier on the lowest unit price, or on a relationship, then cannot show why when quality or delivery fails. Arbitrary spreadsheet weights rarely survive a second look. A bulk discount can look cheap and still raise the cost of holding stock.

Procurement & Vendor Selection makes that comparison available to any AI agent:

- **Which supplier, and why**: a rank from the scores and weights you supply, with cost criteria and benefit criteria handled separately.
- **Weights a committee can inspect**: pairwise comparisons, and a consistency check that names the pair to revisit.
- **Whether a bulk discount is worth the extra stock**: total cost at each price break, not the unit price alone.
- **Who is delivering badly**: on-time rate and defect rate from the delivery history you supply.
- **How to treat the category**: a spend class of strategic, leverage, bottleneck, or routine.
- **A record for the committee**: criteria, weights, scores, consistency, and your file reference when you supply one.

## Why choose it: the same choice every time

Procurement & Vendor Selection is deterministic. The same quotations, pairwise comparisons, delivery history, and settings produce the same rank, the same consistency check, the same bulk-discount answer, and the same spend class. Results are calculated from established decision models. They are not written afresh by a language model each time you ask.

That matters because a purchase committee, finance, and audit act on a choice they may have to show again. They need a recommendation they can rerun and compare. The agent's wording around the numbers may vary. The numbers do not.

| What it does | Established method |
| --- | --- |
| Turns "price matters more than delivery" into weights, and flags judgments that contradict each other | Analytic Hierarchy Process (AHP) |
| Ranks suppliers against an ideal option and a worst-case option, with cost and benefit criteria | TOPSIS |
| Tests whether a bulk price break lowers total cost | Quantity-discount economic order quantity (EOQ) |
| Scores on-time delivery and defects, and withholds a reliability read when history is thin | On-time rate and defect rate |
| Sorts spend into strategic, leverage, bottleneck, and routine | Kraljic portfolio matrix |

**It does not guess.** Scores you did not supply are not invented. Pairwise judgments that contradict each other return **FIX FIRST** and name the pair to revisit. A rank is not forced on top of a failed consistency check.

**It recommends and does not act.** It never places a purchase order, never awards a tender, and never writes to an ERP, a public procurement portal, or Dataverse. On a government, public-sector, or GeM tender, set `procurement_context` to `public_tender`. The rank is decision support. It does not replace L1, QCBS, GeM, or the tender's own award rule.

## Who should use this

| Role | How it helps |
| --- | --- |
| **Purchase managers** at manufacturers and distributors | Rank component suppliers on price, quality, delivery, and service, and leave a record the committee can rerun |
| **Hospital and pharmacy purchase officers** | Compare surgical consumables or other supplies on more than unit price, with weights a review can inspect |
| **School and college administrators** | Choose a vendor for uniforms, transport, or services in a way a management board can follow |
| **Construction and project buyers** | Score quotes on price, delivery commitment, quality, and reliability when those scores exist |
| **Factory buyers** | Test whether a bulk price break is worth the holding cost, with a total-cost comparison |
| **Government and public-procurement cells** | Document a structured comparison as decision support. The tender's own award rule still governs the award |
| **Finance and operations leads** | See the trade-off between price, reliability, bulk stock, and supply risk before cash is committed |
| **Agent makers and developers** | Add a tested supplier comparison to an agent, so nobody has to build or maintain AHP or TOPSIS |

**Best fit:** manufacturers, hospitals, pharmacy buyers, schools and colleges, construction firms, and public-procurement teams that compare suppliers and must show why.

**Not required:** an enterprise procurement suite, an operations-research team, or an ERP. A quote sheet is enough to start. A delivery log or a spend file makes the answer sharper.

## Use cases

Each use case below can be built as an agent. The agent types are listed in [Agents you can build](#agents-you-can-build).

**1. Rank suppliers for a purchase committee**
Scenario: a hospital purchase officer has four suppliers of surgical consumables, scored on price, quality, delivery, and service.
Outcome: criteria weights from pairwise comparisons, a consistency check, and a ranked choice the committee can rerun.
Ask: "Which supplier should we choose, and why?"

**2. Is the bulk discount worth it?**
Scenario: a factory buyer is offered a lower unit price at a higher order quantity.
Outcome: a yes-or-no on total cost (purchase plus ordering plus holding), not a guess from the unit price.
Ask: "Is this bulk discount worth the extra stock?"

**3. A vendor choice a board can follow**
Scenario: a school administrator must pick a vendor for uniforms or transport.
Outcome: a transparent rank and the weights behind it, so the board can see what was prioritised.
Ask: "Rank these vendors and show the weights."

**4. Who is delivering badly?**
Scenario: goods-receipt history has promised dates, actual dates, and sometimes rejected quantity.
Outcome: on-time rate and defect rate by supplier. Fewer than 5 deliveries is reported as too thin to assess.
Ask: "Which suppliers are delivering late or with defects?"

**5. Strategic spend versus routine buys**
Scenario: a category list has annual spend and a supply-risk rating.
Outcome: each item classed as strategic, leverage, bottleneck, or routine.
Ask: "Classify this spend as strategic, leverage, bottleneck, or routine."

**6. An audit-ready decision record**
Scenario: finance or an auditor asks why a supplier was chosen.
Outcome: a record with criteria, weights, scores, consistency, your `decision_id`, and `approved_by` when someone has approved it. The connector returns the record. It does not file it.
Ask: "Give me the decision record for this supplier choice."

**7. Public tender or PSU buying**
Scenario: the purchase is a government, public-sector, or GeM tender with its own award rule.
Outcome: set `procurement_context` to `public_tender`. Use the rank as decision support only.
Ask: "Compare these bids as decision support for a public tender."

**8. Large delivery logs or spend files**
Scenario: more than about 1,000 delivery rows, more than about 500 spend items, or more than about 60 suppliers.
Outcome: the same calculation runs in the agent's sandbox through the local engine, and the file stays there.
Ask: "Score this large delivery log without sending the rows."

## Agents you can build

Each agent is one agent in your agent host with this connector added as a tool. The last column shows what else to add.

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Supplier rank agent | Answers which supplier to choose, and why, with weights and a consistency check | A quote sheet, or a data source from [Where the data comes from](#where-the-data-comes-from) | 1, 3 |
| Bulk-discount agent | Tests whether a price break lowers total cost | Annual demand, order cost, and price breaks in the request | 2 |
| Delivery review agent | Flags late delivery and defects, and withholds a read when history is thin | A goods-receipt or delivery export | 4 |
| Spend-class agent | Classes categories as strategic, leverage, bottleneck, or routine | Annual spend and a supply-risk rating | 5 |
| Committee record agent | Returns the decision record for a file number and, when supplied, an approver | Your `decision_id`. Leave `approved_by` empty until someone approves | 6 |
| Public-tender support agent | Produces a comparison that does not replace the tender award rule | `procurement_context` set to `public_tender` | 7 |
| Large-file agent | Runs the local engine on a full delivery or spend export | Code execution on the agent host | 8 |
| ERP quote agent | Reads quotations and receipts from the ERP, calculates the rank, and hands it back for a person to award | The ERP's own MCP server | 1, 4 |
| Scheduled review agent | Sends a weekly delivery and spend-class note without anyone asking | A flow with a recurrence trigger, and email or Teams | 4, 5 |

The tools run on request. Scheduling comes from the flow, not from the connector.

## At a glance

| Item | Detail |
| --- | --- |
| Tools | `procurement_supplier_selection` (hosted calculation) and `procurement_get_engine` (local engine) |
| Minimum input | No single field is required. A useful call sends criteria and supplier scores, or a delivery log, or a spend list |
| Hosted tool size | About 1,000 delivery rows, about 500 spend items, and about 60 suppliers |
| Larger files | Local engine, runs in your agent's code sandbox, needs only `numpy` |
| Writes to your systems | None |
| Pricing | [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment) |

<!-- TODO Saifs AI: confirm this marketplace URL is the pricing page for this offer, and confirm the response field that carries the engine version. -->

## Where the data comes from

Procurement & Vendor Selection does not connect to your systems. The agent brings the data to it, from any source that can supply quotations, deliveries, or spend rows. The connector needs only rows. It has not been certified against each system named below.

| Data source | How the agent gets the data | Best route |
| --- | --- | --- |
| Spreadsheet or CSV export from purchasing, ERP, or goods receipt (for example Tally, QuickBooks, Zoho, a hospital purchase sheet) | The user uploads the file to the agent | Local engine for large files, hosted tool for small ones |
| An ERP that has its own MCP server (for example Dynamics 365, Business Central, SAP, NetSuite) | The agent reads quotations, receipts, and spend, then passes the rows on | Hosted tool up to the size limits. For more, export a file and use the local engine |
| A public tender workbook or GeM export | The user uploads the comparison sheet and sets `procurement_context` to `public_tender` | Hosted tool |
| Dataverse tables | The Dataverse MCP server or the Dataverse connector | Same size rule as above |
| SQL database, Excel workbook online, or SharePoint list | A connector added to the same agent or flow | Same size rule as above |
| A scheduled flow | A flow reads the source on a schedule and calls this connector | Hosted tool |

**Works alongside your ERP's MCP server.** An ERP's MCP server gives the agent the ERP's records and functions. This connector adds the supplier comparison, with no procurement suite to configure. The ERP supplies the rows. Procurement & Vendor Selection calculates the rank. A person places the order or applies the tender rule.

**What to pull from the source**

| Data in your system | Field it fills | Why it matters |
| --- | --- | --- |
| What is being bought | `item_name` | Names the decision |
| Criteria and pairwise judgments | `criteria`, `pairwise_comparisons` | Produces weights a committee can inspect |
| Supplier scores | `suppliers` | The numbers TOPSIS ranks. Do not invent a score |
| Goods receipts | `delivery_history` | On-time rate and defect rate |
| Price breaks | `price_breaks`, `annual_demand`, `order_cost` | Whether a bulk discount lowers total cost |
| Category spend and supply risk | `spend_items` | Strategic, leverage, bottleneck, or routine |
| Your file reference | `decision_id`, `approved_by` | A record the committee can cite |

**Column names are recognised as they are.** You do not need to rename ERP headers on a delivery file. Send them as exported.

**File formats for the local engine.** CSV, TSV, and JSON. Pass small inputs such as criteria, pairwise comparisons, and supplier scores in `params`. Point `delivery_history_path` and `spend_items_path` at the files. Save an Excel sheet as CSV first, or ask the agent to convert it in the sandbox.

## How it works

The connector offers two routes to the same calculation. The agent picks the route by the size of the data.

| | Hosted tool | Local engine |
| --- | --- | --- |
| Tool name | `procurement_supplier_selection` | `procurement_get_engine` |
| Use when | About 1,000 delivery rows, 500 spend items, and 60 suppliers, or fewer | Anything larger, or when the rows are already a file and there are more than about 200 of them |
| What the agent sends | The rows and settings | File paths and settings only |
| What comes back | The full result | A ready-to-run Python script |
| Where the calculation runs | Saifs AI endpoint | Your agent's code-execution sandbox |
| Where your commercial data goes | To the Saifs AI endpoint for calculation | Nowhere. It stays in the sandbox |
| Requires | Nothing extra | An agent host with code execution, and `numpy` |

**Local engine, step by step**

1. The user uploads or points the agent to a delivery file or a spend file.
2. The agent calls `procurement_get_engine` with the file path and any settings in `params`.
3. The connector returns the calculation script.
4. The agent runs the script in its sandbox against the file. It does not shorten or rewrite the script.
5. The script prints the decision cards and writes full detail to `procurement_result.json` in the sandbox.
6. The agent reports the decisions to the user.

### Inside the connector

1. The user asks which supplier to choose, and why.
2. The agent chooses a tool and calls the connector, which passes the call to the Saifs AI MCP server over HTTPS at `https://procurement-supplier-selection-mcp-microsoft.saifs.ai/mcp`.
3. The server checks the signed-in account.
4. The hosted tool runs the engine on the rows and returns the result. The engine tool returns the same engine as a script, for the agent to run in its sandbox.
5. The agent reads the result and explains it in plain language. The numbers come from the engine. The wording comes from the agent.

### Inside the calculation

Both routes run the same engine. It works through the decision in fixed steps.

| Step | What the engine does | Method |
| --- | --- | --- |
| 1. Take the inputs you sent | Uses any subset: criteria, scores, deliveries, price breaks, or spend. It does not invent a missing score | Supplied values only |
| 2. Weights | Turns pairwise comparisons on a 1 to 9 scale into criteria weights. A consistency ratio above `cr_threshold` (default 0.10) returns FIX FIRST and names the pair to revisit. Typed-in `criteria_weights` are a fallback and are not consistency-checked | Analytic Hierarchy Process (AHP) |
| 3. Rank | Ranks suppliers against an ideal option and a worst-case option. Cost criteria are lower-is-better. Benefit criteria are higher-is-better. Scores closer than `tie_threshold` (default 0.02) are treated as tied | TOPSIS |
| 4. Bulk discount | Compares total yearly cost at each all-units price break: purchase plus ordering plus holding. Holding cost defaults to 20% of unit value per year (`holding_rate` 0.20) | Quantity-discount economic order quantity |
| 5. Delivery | Computes on-time rate and defect rate. History shorter than `min_deliveries_for_assessment` (default 5) is too thin to assess. `on_time_floor` defaults to 0.80. `late_grace_days` defaults to 0 | On-time rate and defect rate |
| 6. Spend class | Splits items by annual spend and supply risk into strategic, leverage, bottleneck, or routine. `spend_threshold` defaults to the median | Kraljic portfolio matrix |
| 7. Decision record | Records criteria, weights, scores, consistency, `decision_id` (generated if absent), and `approved_by` only when you supply it | Fixed record from the steps above |

**Why the answer does not change.** Every step is a fixed procedure with no language model involved. The same inputs and settings return the same numbers.

**What happens when data is missing.** The engine runs the layers you can support and reports the rest. It does not block the call for a missing layer, and it does not fill in a supplier score. Currency is a display label. There is no conversion between currencies. Default currency is USD.

**Where the working is shown.** FIX FIRST names the contradictory pair. Thin delivery history is reported rather than scored. The decision record lists the criteria, weights, and scores that produced the rank.

## Data handling and security

| Question | Local engine | Hosted tool |
| --- | --- | --- |
| Does commercial data reach Saifs AI? | No | Yes, the rows passed in the call |
| Where is it processed? | Your agent host's code sandbox | `procurement-supplier-selection-mcp-microsoft.saifs.ai`, over HTTPS |
| What happens to the data after the call? | It never left your sandbox | <!-- TODO Saifs AI: confirm retention. Do not state "discarded" or an audit-hash policy until the facts sheet confirms it for this server. --> |
| What does Saifs AI record? | Only that the engine script was requested, if your facts sheet confirms that | <!-- TODO Saifs AI: confirm the audit and billing log for this server. --> |

**What "sandbox" means.** The sandbox is the code-execution environment of your own agent host, such as Copilot Studio or Azure AI Foundry. It belongs to your tenant, not to Saifs AI, and your host's data policies continue to apply to it.

**Use the local engine only.** If your policy requires that quotations and delivery files never leave your environment, turn off the hosted tool. In Copilot Studio, open the agent's **Tools** tab, select the Procurement & Vendor Selection server, turn off **Allow all**, and switch off `procurement_supplier_selection`. Leave `procurement_get_engine` on. The agent can then use only the local engine, and any tool added to the server later stays off by default. This setting is per agent.

**Sign-in.** Users sign in with Microsoft single sign-on (OAuth 2.0). Saifs AI holds only the account details needed for sign-in and billing. No API key is created, shared, or stored.

**No write access.** The connector has no action that creates, changes, or deletes records in any ERP, tender portal, or Dataverse.

**The script is open to review.** The local engine script is plain Python with a single dependency, `numpy`, and makes no network calls. Your security team can read it before allowing it to run.

Vendor files can be commercial data. A proprietorship vendor can also be personal data. Send only data you are authorised to process.

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
| Microsoft Publisher Attestation | Saifs AI's answers to Microsoft's questionnaire on security, data handling and compliance. Microsoft publishes the answers and does not verify them. Renewed every year. | [link] |
| CSA STAR Level 1 | Saifs AI's completed Cloud Security Alliance questionnaire (CAIQ), published on the public STAR Registry. Updated every year. | [link] |
| CSA STAR for AI Level 1 | Saifs AI's completed AI questionnaire (AI-CAIQ) on the same registry. | [link] |

-->

| Design fact | Detail |
| --- | --- |
| Customer data stored | <!-- TODO Saifs AI: confirm. Local engine keeps rows in your sandbox. Hosted-tool retention is not confirmed in this draft. --> |
| Passwords held | None. Sign-in is Microsoft single sign-on. |
| Write access to your systems | None |
| Local engine | Plain Python, one dependency, no network calls, open to review |

**Independent audits.** Saifs AI does not yet hold an independently audited certificate such as SOC 2 or ISO 27001. If your review needs a completed security questionnaire, request it through the [contact page](https://saifs.ai/contact-us).

## Token usage and cost

With the hosted tool, token use grows with every delivery and spend row, because the model sends those rows out and reads the rank back. With the local engine, the rows stay in the sandbox, so that row cost does not grow with the file.

| File | Hosted tool: tokens and cost per run | Local engine: first run | Local engine: each later run | Comparison |
| --- | --- | --- | --- | --- |
| 4 suppliers, 20 deliveries, 10 spend items | about 1,700 tokens, about $0.02 | Script not measured | about 80 tokens, under $0.01 | Hosted tool. One short comparison |
| 60 suppliers, 1,000 deliveries, 500 spend items | about 65,000 tokens, about $0.63 | Script not measured | about 80 tokens, under $0.01 | Hosted tool is still within the size cap |
| 200 suppliers, 10,000 deliveries, 2,000 spend items | Not the hosted route. Sending the rows alone is about 540,000 tokens | Script not measured | about 80 tokens, under $0.01 | Local engine only |

**Estimate your own file**

- **Hosted tool, to send:** about 48 tokens per delivery row, 26 per spend item, and 33 per supplier on this sample. Pairwise comparisons are extra.
- **Hosted tool, to read:** about 32 tokens per ranked supplier in this sample.
- **Local engine, later run:** about 80 tokens for a short decision print.

**Which route to choose**

- **A short quote comparison:** the hosted tool. Four suppliers on this sample are about $0.02.
- **Up to about 1,000 deliveries, 500 spend items, and 60 suppliers:** the hosted tool is still the allowed route. That sample is about $0.63.
- **Above those caps, or more than about 200 rows already in a sandbox:** the local engine.
- **Quotation files must stay in your environment:** the local engine, with the hosted tool switched off.

**Hosts that bill per action.** Some agent hosts charge a fixed amount for each tool call and do not bill by token. On those hosts the measured token saving does not change the bill. Choose the local engine for file size and for keeping the file in your environment.

How these figures were measured: token counts use the `o200k_base` tokenizer on generated sample data, with rows written as JSON, on 16 November 2026. Costs use $1.25 per million input tokens and $10 per million output tokens. Rows the model sends are output tokens. The rank it reads is input tokens. Rank records are representative objects from the documented fields, not a captured engine file. The local-engine script is not in this folder, so the first local run has no dollar figure. Do not copy the Inventory script cost onto this connector.

## Prerequisites

- A Saifs AI account, created by signing in with Microsoft single sign-on
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Access to Copilot Studio, Microsoft 365 Copilot, Azure AI Foundry, Power Automate, Power Apps, or Azure Logic Apps
- Quotations, a delivery log, or a spend list. Any subset can start a call
- For the local engine: code execution enabled on the agent host

## How to get credentials

No keys or secrets are needed. When you create a connection, you are redirected to sign in with your Microsoft account and approve access. If the connection later expires, sign in again from the connection page.

## Get started with your connector

1. In Copilot Studio, open your agent and go to **Tools**, then **Add a tool**.
2. Search for **Procurement & Vendor Selection** and select it.
3. Create a connection and sign in.
4. Add this to the agent's instructions: "For any question about choosing, comparing, or reviewing suppliers, call the Procurement & Vendor Selection tool. Do not calculate weights, ranks, or on-time rates yourself."
5. Provide criteria and supplier scores, or a delivery file, or a spend list. Do not invent a score the user did not give.
6. Ask in plain language, for example "Which supplier should we choose, and why?"

If the result says FIX FIRST, show that contradiction and ask for the named pair again. Treat results as decision support. A person still awards the business and places the order.

## For administrators

Enabling this connector gives the organisation one approved way to answer which supplier to choose, and why, inside the agents your makers already build.

**Why this is an administrator decision.** Without a calculation tool, makers ask an agent to pick a vendor from a spreadsheet. A language model can give a different rank on each run, and the result cannot be rerun or compared from one committee to the next. A governed connector replaces that with one method for every agent and team.

- **You choose who can connect it.** Tenant and environment permissions apply. Only users who can create a connection can call the tools.
- **You keep your system of record.** The connector recommends and explains. It does not place purchase orders, award tenders, or write back to any system.
- **You can check the number.** The same quotations and pairwise comparisons can be rerun. Contradictory weights return FIX FIRST. Missing scores are reported, not filled in.
- **You control where data goes.** Turn off the hosted tool per agent to keep quotation and delivery files in your environment.
- **You set data policy as usual.** Place the connector in the Business, Non-business, or Blocked group with data loss prevention policies.
- **There are no secrets to rotate.** Expired connections are renewed by signing in again.

**Review checklist**

- Decide whether the hosted tool is allowed, or local engine only
- Assign the connector to the correct data policy group
- Confirm that public tenders stay on `public_tender` and do not override the award rule
- Review the privacy policy and terms
- Read the self-assessments listed under Security and compliance, once they are published
- Review the local engine script if your policy requires it

## For developers

No field is required. Send whatever the business has. The response reports what was missing. Do not invent a score.

### Choice fields

| Field | Meaning | Needed for |
| --- | --- | --- |
| `item_name` | What is being bought | A readable decision |
| `criteria` | Criterion names, in order | Weights and rank |
| `pairwise_comparisons` | Preferred weights. Each pair once, on a 1 to 9 scale: `a` is `value` times as important as `b` | A consistency-checked weight set |
| `criteria_weights` | Fallback fixed weights. Not consistency-checked | A rank when pairwise judgments are refused |
| `criteria_directions` | `cost` means lower is better. `benefit` means higher is better | A correct TOPSIS rank |
| `suppliers` | One object per supplier: name plus a number for each criterion you have | The rank |
| `procurement_context` | `private` (default) or `public_tender` | Keeping a tender inside its own award rule |
| `decision_id` | Your file or PO number. Generated if absent | The decision record |
| `approved_by` | Approver name or role. Leave empty until someone has approved | Recording an approval you already have |
| `currency`, `currency_symbol` | Display label only. Default currency USD. No conversion | Labels on money figures |
| `as_of` | Decision date (YYYY-MM-DD). Later deliveries can be filtered | A dated record |

### Delivery history (optional)

One row per delivery: supplier, and either on-time (yes or no) or promised date plus delivered date. Optional quantity, rejected quantity, and date. Up to 1,000 rows on the hosted tool.

### Bulk discount (optional)

`annual_demand`, `order_cost`, `holding_rate` (default 0.20), `max_order_qty`, and `price_breaks` as all-units slabs, or one slab list per supplier.

### Spend class (optional)

`spend_items` with item, annual spend, and supply risk (`low`, `medium`, `high`, or 1 to 5). Up to 500 items on the hosted tool. `spend_threshold` splits high and low spend. Default is the median.

### Options and defaults

| Option | Default | Purpose |
| --- | --- | --- |
| `cr_threshold` | 0.10 | Consistency ratio above this returns FIX FIRST |
| `tie_threshold` | 0.02 | Scores this close are tied |
| `on_time_floor` | 0.80 | On-time target used in the delivery read |
| `min_deliveries_for_assessment` | 5 | Minimum deliveries before a supplier is assessed |
| `late_grace_days` | 0 | Days late that still count as on time |
| `holding_rate` | 0.20 | Yearly holding cost as a share of unit value |
| `procurement_context` | `private` | Set `public_tender` for government, PSU, or GeM |
| `include_diagnostics` | false | Extra diagnostic detail when you ask for it |

### Sample request

```json
{
  "item_name": "surgical gloves",
  "criteria": ["price", "quality", "delivery_days", "service"],
  "criteria_directions": {
    "price": "cost",
    "quality": "benefit",
    "delivery_days": "cost",
    "service": "benefit"
  },
  "pairwise_comparisons": [
    { "a": "price", "b": "quality", "value": 3 },
    { "a": "price", "b": "delivery_days", "value": 2 },
    { "a": "price", "b": "service", "value": 4 },
    { "a": "quality", "b": "delivery_days", "value": 2 },
    { "a": "quality", "b": "service", "value": 3 },
    { "a": "delivery_days", "b": "service", "value": 2 }
  ],
  "suppliers": [
    { "name": "North Supply", "price": 42, "quality": 8, "delivery_days": 5, "service": 7 },
    { "name": "Harbour Medical", "price": 39, "quality": 6, "delivery_days": 9, "service": 8 }
  ],
  "currency": "USD",
  "procurement_context": "private"
}
```

### Sample response (shortened)

<!-- TODO Saifs AI: replace this note with a shortened response from a real call. Do not publish invented ranks or scores. -->

A live shortened response is not included in this draft. The sections below are the ones the tool is documented to return. Capture one call before you publish the page.

### Response sections

| Section | Use it for |
| --- | --- |
| Ranked supplier | Which supplier to choose, and why, including closeness to the ideal |
| Consistency result | Weights, and FIX FIRST when judgments contradict, naming the pair to revisit |
| Bulk-discount test | Whether a price break lowers total cost |
| Delivery flags | On-time rate, defect rate, and suppliers too thin to assess |
| Spend class | Strategic, leverage, bottleneck, or routine |
| Decision record | Criteria, weights, scores, consistency, `decision_id`, and `approved_by` when supplied |

## Known issues and limitations

- **Decision support only.** It does not place a purchase order or award a tender. GeM, L1, QCBS, and other statutory award rules are not overridden.
- **FIX FIRST stops the rank.** Do not use a rank until the named pairwise comparison is answered again.
- **Thin delivery history.** Fewer than 5 deliveries is too thin for a reliability assessment.
- **Hosted tool size.** About 1,000 delivery rows, 500 spend items, and 60 suppliers. Above about 200 rows already in a sandbox, use the local engine.
- **Holding cost.** When not supplied, it is set at 20% of unit value per year.
- **Currency.** The currency is a display label. There is no conversion between currencies.
- **Scores.** Only scores you supply are used. The tool does not estimate a missing quality or service score.
- **Public tenders.** Set `procurement_context` to `public_tender`. Present the output as decision support.
- **Regions.** Not available in US Government (GCC, GCC High, DoD) or China (21Vianet) clouds.

<!-- TODO Saifs AI: confirm the excluded-cloud sentence is still how Microsoft lists this connector. -->

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| The tool cannot run, or asks you to sign in again | The connection has expired | Open the connection and sign in again |
| FIX FIRST, with a named pair | Pairwise judgments contradict each other | Re-answer that pair. Do not treat the rank as ready |
| A supplier has no delivery assessment | Fewer than 5 deliveries | Add receipts, or accept that the supplier is not assessed yet |
| The agent gives a rank without calling the tool | Agent instructions do not require the tool | Add the instruction shown in Get started |
| The local engine does not run | Code execution is not enabled on the agent host | Enable code execution, or use the hosted tool for small files |
| Not permitted, or a subscribe link | The account cannot call the tool | Paste the full Marketplace link exactly as returned. Do not shorten it |

## FAQ

**Do I need a specialist to use this?**
No. A user asks in plain language and the agent returns a rank, a consistency check, and the layers the file can support.

**What is the least data I need?**
Any subset. A criteria list and supplier scores produce a rank. Delivery history and spend add the other layers. The tool reports what was missing.

**Does my data leave my environment?**
Not with the local engine. With the hosted tool, the rows in the call are sent to Saifs AI for calculation. You can turn the hosted tool off.

**Will it change anything in my systems?**
No. It does not place a purchase order, award a tender, or write to an ERP or Dataverse.

**My ERP already has an MCP server. Why add this?**
That server gives the agent your ERP's records and functions. This connector gives the agent the supplier comparison, and it works on rows from any source.

**Why do I get the same answer every time?**
The same quotations, pairwise comparisons, and settings run the same formulas. The agent's wording around the numbers may vary. The numbers do not.

**What happens if my data is incomplete?**
The tool still runs the layers it can support. It does not invent a score. Contradictory weights return FIX FIRST.

**Can I check how a result was produced?**
Yes. The decision record lists criteria, weights, scores, and the consistency result. FIX FIRST names the pair that failed.

**How large a file can it handle?**
The hosted tool is for about 1,000 delivery rows, 500 spend items, and 60 suppliers. Larger files use the local engine.

**Can it run on a schedule?**
Yes, by calling it from a scheduled flow. The connector itself runs on request.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment). Microsoft Premium connector licensing applies separately.

## Support

**Procurement & Vendor Selection - Saifs AI** is published by Saifs AI.

- Website: [https://saifs.ai](https://saifs.ai)
- Learn more: [https://saifs.ai/procurement-supplier-selection](https://saifs.ai/procurement-supplier-selection)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Privacy: [https://saifs.ai/privacy_policy/procurement-supplier-selection](https://saifs.ai/privacy_policy/procurement-supplier-selection)
- Terms: [https://saifs.ai/eula](https://saifs.ai/eula)
- Sign-in: Microsoft single sign-on (OAuth 2.0). No API keys.
- MCP endpoint: `https://procurement-supplier-selection-mcp-microsoft.saifs.ai/mcp`

<!-- OPEN QUESTIONS
1. Confirm the marketplace offer URL for this connector.
2. Confirm hosted-tool retention and what is written to the audit or billing log.
3. Capture a shortened live response before publish.
4. Hosted send and read tokens are measured on generated JSON. The local-engine script is still not tokenized. A live result file would replace the representative rank objects.
5. Confirm the engine version field name.
6. Confirm excluded government and China clouds for this offer.
7. Publisher Attestation, STAR Level 1, and STAR for AI are not listed until completed, dated, and linked.
-->
