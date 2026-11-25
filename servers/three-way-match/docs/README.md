# 3 Way Match:PO-Receipt-Invoice - Saifs AI

**3 Way Match:PO-Receipt-Invoice - Saifs AI** helps accounts payable teams pay only for what was ordered and received, at the agreed price. You send purchase orders, goods receipts, and supplier invoices. It returns line matches, a verdict per invoice, the amount that can be paid, and the amount to hold, with reason codes.

It is built for teams that still match those three documents by hand, or whose accounts system fails the match when a delivery is partial, split, or billed in a different unit.

**In plain language:** you give it what you ordered, what arrived, and what the supplier billed. It tells you how much of each invoice a person can pay now, and why the rest stays on hold.

**For technical readers:** quantities are allocated with min-cost flow. Descriptions are compared with Jaro-Winkler and token similarity. Split deliveries use subset-sum. Differences inside your tolerance bands can pass. Differences outside them are held.

It recommends. It does not release, block, or post a payment. It is not bank reconciliation, and it is not a duplicate-invoice audit.

**Publisher:** Saifs AI
**Website:** [https://saifs.ai](https://saifs.ai)
**Pricing:** [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
**Contact and help:** [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
**Privacy:** [https://saifs.ai/privacy_policy/three-way-match](https://saifs.ai/privacy_policy/three-way-match)
**Terms:** [https://saifs.ai/eula](https://saifs.ai/eula)
**Sign-in:** Microsoft single sign-on (OAuth 2.0). No API keys.
**MCP endpoint:** [https://three-way-match-mcp-microsoft.saifs.ai/mcp](https://three-way-match-mcp-microsoft.saifs.ai/mcp)

<!-- TODO Saifs AI: publish the product page, then add a Learn more line here and in Support. https://saifs.ai/three-way-match returned HTTP 404 on 24 November 2026. Do not link it until it opens. -->

| If you are | Start with |
| --- | --- |
| A business owner, controller, or accounts payable lead | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [Where the data comes from](#where-the-data-comes-from) |
| An administrator or security reviewer | [Data handling and security](#data-handling-and-security), [Security and compliance](#security-and-compliance), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [Where the data comes from](#where-the-data-comes-from), [For developers](#for-developers) |
| A finance or audit reviewer checking the method | [Inside the calculation](#inside-the-calculation), [Responsible use](#responsible-use) |

## The problem it solves

Before a supplier is paid, three documents should agree. The purchase order says what was ordered. The goods receipt says what arrived. The invoice says what is billed.

Real documents rarely line up. A delivery arrives in parts. One invoice covers several receipts. Several invoices sit against one purchase order. The supplier bills by the box and stores count pieces. The price is a little above the order. A freight line was not on the order. A strict match fails, so a clerk overrides it, or the check is skipped. The company can then pay for goods that never arrived, or at a price above the order.

3 Way Match:PO-Receipt-Invoice makes that check available to any AI agent:

- **What matches:** each invoice line against the receipt lines and the purchase-order lines.
- **What the invoice verdict is:** match, within tolerance, or hold.
- **What can be paid now:** the payable amount, and the hold, with reason codes.
- **What was received and not yet billed:** an uninvoiced-receipt list, for month-end accruals.
- **Where the differences repeat:** a variance report by supplier and by item.
- **Why one invoice matched the way it did:** the lines it used, through `explain_match`.

## Why choose it: the same match every time

The same documents and the same settings are the same request. The hosted tool is marked idempotent. The match is a fixed method. It is not written afresh by a language model each time you ask.

That matters because a controller holds part of an invoice on a result. The result has to be the same when the file has not changed. The agent's wording around the numbers may vary. The numbers do not.

| What it does | Established method |
| --- | --- |
| Allocates received quantity across invoice lines, without using a receipt twice | Min-cost flow assignment |
| Finds a split, where several lines add up to what was received | Subset-sum |
| Matches an item by description when the code does not match | Jaro-Winkler and token similarity |
| Decides whether a price or quantity difference can be paid | Tolerance bands. `price_pct` defaults to 2% |
| Keeps unit conversions a person has confirmed | A caller-held rules snapshot |
| Returns the verdict, the payable amount, the hold, uninvoiced receipts, and variances | The same match, reported for the job you ask for |

**It does not guess a unit.** The tool returns unit-conversion suggestions. A conversion is used after a person confirms it, through `confirmed_conversions` or `rules_snapshot`. This page does not claim what the engine does with two units when no factor was sent.

**It recommends and does not act.** It never releases a payment, never blocks one, and never writes to an ERP or accounting system.

## Who should use this

| Role | How it helps |
| --- | --- |
| **Accounts payable clerks** | A verdict per invoice, with the payable amount and the hold separated |
| **Purchasing and stores teams** | The invoice checked against what was ordered and what arrived, plus receipts not yet billed |
| **Finance controllers** | Variances by supplier, and the accrual list of goods received but not invoiced |
| **Internal auditors** | Match evidence they can rerun on the same export |
| **Manufacturers, distributors, hospitals, and construction firms** | Partial deliveries and split documents, which are common in those books |
| **Business owners** | What can be paid now, without building the match themselves |
| **Agent makers and developers** | A tested three-way match inside an agent, so the agent does not invent the allocation |

**Best fit:** accounts payable, purchasing, stores, controllership, and internal audit at companies that buy against a purchase order and record a goods receipt.

**Not required:** a separate payables suite, a data science team, or a planning module. An export of orders, receipts, and invoices is enough to start. Tolerances and confirmed unit conversions make the answer follow your policy.

## Use cases

Each use case below can be built as an agent. The agent types are listed in [Agents you can build](#agents-you-can-build).

**1. A whole payment run**
Scenario: several invoices must be checked together, so one receipt is not used twice.
Outcome: call `three_way_match` with `job` set to `match_batch`.
Ask: "Match these supplier invoices to our purchase orders and receipts."

**2. One invoice**
Scenario: one bill is waiting, and the open orders and receipts are already exported.
Outcome: `job` set to `match_invoice`, or the single invoice passed in `invoice`.
Ask: "Does this invoice match the order and the receipt?"

**3. How much can be paid today**
Scenario: part of the quantity arrived, or the price is above the order, and only the difference should wait.
Outcome: `job` set to `payable_amount`. The payable amount and the hold come back with reason codes.
Ask: "How much of this invoice can we pay today?"

**4. Why this line matched**
Scenario: a clerk or an auditor wants the purchase-order and receipt lines behind one invoice.
Outcome: `job` set to `explain_match`, with `invoice_ref` set to that invoice.
Ask: "Which order and receipt lines did this invoice match, and why?"

**5. Goods received but not invoiced**
Scenario: month-end, and stores have receipts that no invoice has used yet.
Outcome: `job` set to `uninvoiced_receipts`. `as_of` sets the ageing date. If omitted, the tool uses the latest date it sees.
Ask: "List goods received but not yet invoiced."

**6. Where price and quantity keep drifting**
Scenario: the same supplier, or the same item, is outside tolerance more than once.
Outcome: `job` set to `variance_report`.
Ask: "Show price and quantity variances by supplier and by item."

**7. A service bill with no goods receipt**
Scenario: there is a purchase order and an invoice, and stores never raise a receipt.
Outcome: set `require_receipt` to false. The default is true, which is a three-way match. False is a two-way match against the order.
Ask: "Match this service invoice to the purchase order only."

**8. A large book**
Scenario: more than 5,001 invoice rows, or more than 20,001 purchase-order, receipt, or prior-invoice rows.
Outcome: `three_way_match_get_engine` returns a script. The agent runs it in its own sandbox. The rows stay there.
Ask: "Match this full export of purchase orders, receipts, and invoices."

**9. The same run again, with confirmed units**
Scenario: last time the tool suggested a conversion, and a person confirmed the factor.
Outcome: send that `rules_snapshot`, or the confirmed rows in `confirmed_conversions`.
Ask: "Run this week's match with the conversions we confirmed last time."

## Agents you can build

Each agent is one agent in your agent host with this connector added as a tool. The last column shows what else to add.

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Payment-run agent | Matches a batch of invoices to open orders and receipts before the run | A file upload, or a data source from [Where the data comes from](#where-the-data-comes-from) | 1 |
| Single-invoice agent | Returns the verdict for one bill | The same data source | 2 |
| Payable-amount agent | Separates what can be paid now from what stays on hold | The same data source | 3 |
| Match-evidence agent | Shows which order and receipt lines an invoice used | `invoice_ref` for the bill to explain | 4 |
| Accrual agent | Lists goods received and not yet invoiced | Receipts and the invoices already booked | 5 |
| Variance agent | Reports price and quantity differences by supplier and item | The same data source | 6 |
| Service-invoice agent | Matches an invoice to the order when there is no goods receipt | `require_receipt` set to false | 7 |
| Large-book agent | Runs the local engine on a full export | Code execution on the agent host | 8 |
| Accounts-system agent | Reads orders, receipts, and invoices, calculates the match, and hands holds to a person | That system's own MCP server | 1, 3 |
| Scheduled match agent | Runs the check on a recurrence, without anyone asking | A Power Automate flow with a recurrence trigger, and email or Teams | 1, 5 |

The tools run on request. Scheduling comes from the flow, not from the connector. The connector does not release the payment.

## At a glance

| Item | Detail |
| --- | --- |
| Tools | `three_way_match` (hosted match) and `three_way_match_get_engine` (local engine) |
| Jobs on the hosted tool | `explain_match`, `match_batch`, `match_invoice`, `payable_amount`, `uninvoiced_receipts`, `variance_report`. Omit `job` and the tool runs what the inputs allow. |
| Minimum input | Nothing is marked required. A match needs invoices, open purchase orders, and receipts. |
| Hosted tool size | Up to 5,001 invoice rows. Up to 20,001 rows each for open purchase orders, receipts, and prior invoices. Up to 5,000 confirmed conversions. |
| Listed rows | `max_listed` defaults to 200. The range is 1 to 100,000. |
| Larger files | Local engine. It runs in your agent's code sandbox and needs `numpy`. |
| Writes to your systems | None. The local engine writes `three_way_match_result.json` and `rules_snapshot.json` in the sandbox. |
| Price tolerance | `price_pct` defaults to 2% (send `2` or `0.02`). |
| Receipt required | `require_receipt` defaults to true. |
| Out of scope | Bank reconciliation. Duplicate-invoice audits. Releasing or blocking a payment. |

<!-- TODO Saifs AI: confirm the engine version field name from a signed-in response. Do not copy metadata.model_version from Inventory Intelligence. -->

## Where the data comes from

3 Way Match:PO-Receipt-Invoice does not connect to your systems. The agent brings the rows to it, from any source that can supply invoices, purchase orders, and goods receipts.

| Data source | How the agent gets the data | Best route |
| --- | --- | --- |
| A spreadsheet or CSV export from accounting or inventory software (for example Tally) | The user uploads the file to the agent | Local engine for large files, hosted tool inside the caps |
| An ERP that has its own MCP server | The agent reads orders, receipts, and invoices, then passes the rows on | Hosted tool inside the caps. Above them, export a file and use the local engine. |
| Dataverse tables | The Dataverse MCP server or the Dataverse connector | Same as above |
| A SQL database, an Excel workbook online, or a SharePoint list | A connector added to the same agent or flow | Same as above |
| A scheduled flow | A flow reads the source on a schedule and calls this connector | Hosted tool |

The connector needs only rows, so any other tool that returns those documents can feed it the same way.

**Works alongside your ERP's MCP server.** That server gives the agent the ERP's records and functions. This connector adds the three-way match. The ERP supplies the documents. This connector calculates the match. A person releases or holds the payment.

**What to pull from the source**

| Data in your system | Field it fills | Why it matters |
| --- | --- | --- |
| Supplier invoices, one row per line, or a header plus `lines` | `invoices` or `invoice` | The bill being checked. Include invoice number, supplier, date, purchase-order number, item or description, quantity, unit, rate, amount, and tax. Freight and other charges go in as lines. |
| Open purchase-order lines | `open_pos` | What was ordered, at what rate. Charge lines on the order are allowed. Quantity already invoiced, if you know it. |
| Goods receipts | `receipts` | What arrived. Include the receipt number, supplier, purchase-order number, date, item, quantity received, rejected quantity, and unit. |
| Invoices already booked on these orders | `prior_invoices` | They use receipt quantity first, so a new bill is not matched to goods already billed. |
| Your price and quantity policy | `tolerances` | What may pass, and what must be held. |
| Conversions a person has accepted | `confirmed_conversions` or `rules_snapshot` | So "box" and "pieces" are converted with a factor you confirmed, not a guess. |

**Headers can stay as exported.** The tool accepts ERP and Tally headers as they are. `mapping` overrides a header when it was read as the wrong field. This page does not list the header aliases, because the schema does not list them.

**File formats for the local engine.** CSV, TSV, or JSON. The engine reads the files named in the call. Save an Excel sheet as CSV first, or ask the agent to convert it in the sandbox. The script prints the decision cards and writes `three_way_match_result.json` and `rules_snapshot.json`.

## How it works

The connector offers two routes to the same match. The agent picks the route by the size of the data.

| | Hosted tool | Local engine |
| --- | --- | --- |
| Tool name | `three_way_match` | `three_way_match_get_engine` |
| Use when | Up to 5,001 invoice rows, and up to 20,001 rows each for orders, receipts, and prior invoices | Anything larger, or when the rows must stay in your environment |
| What the agent sends | The rows and the settings | File paths, and other inputs in `params` |
| What comes back | The full result | A ready-to-run Python script |
| Where the match runs | Saifs AI endpoint | Your agent's code-execution sandbox |
| Where your documents go | To the Saifs AI endpoint for the match | Nowhere. They stay in the sandbox. The tool states that no data reaches the server on this path. |
| Requires | Nothing extra | An agent host with code execution, and `numpy` |

**Local engine, step by step**

1. The user uploads or points the agent to the invoice, purchase-order, and receipt files.
2. The agent calls `three_way_match_get_engine` with those paths and any settings in `params`.
3. The connector returns the match script. Save it exactly as given. Do not shorten it or rewrite it.
4. The agent runs the script in its sandbox against the files.
5. The script prints the decision cards and writes `three_way_match_result.json` and `rules_snapshot.json` in the sandbox.
6. The agent reports the cards to the user. A person decides what to pay and what to hold.

### Inside the connector

1. The user asks whether an invoice can be paid, or asks for a match, a hold, an accrual list, or a variance report.
2. The agent chooses a tool and calls the connector, which passes the call to the Saifs AI MCP server over HTTPS at `https://three-way-match-mcp-microsoft.saifs.ai/mcp`.
3. The server checks the signed-in account.
4. The hosted tool runs the match on the rows and returns the result. The engine tool returns the same match as a script, for the agent to run in its sandbox.
5. The agent reads the result and explains it in plain language. The numbers come from the match. The wording comes from the agent.

### Inside the calculation

Both routes are the same engine. The tool states what that engine does. A signed-in response was not available, so this page does not name the JSON keys inside the result.

| Step | What the engine does | Method |
| --- | --- | --- |
| 1. Read the documents | Takes invoices, open purchase orders, and receipts as exported. `mapping` can correct a header. | The call |
| 2. Find candidate lines | Compares invoice lines with receipt and order lines. An item code matches exactly. Otherwise the description is scored. | Jaro-Winkler and token similarity |
| 3. Allocate quantity | Assigns received quantity to invoice lines. A receipt's remaining quantity is not exceeded, including across a batch. | Min-cost flow assignment |
| 4. Find a split | Looks for quantities that add up across lines or deliveries. | Subset-sum |
| 5. Apply your policy | Compares price and quantity with the bands you send. `price_pct` defaults to 2%. | Tolerance bands |
| 6. Verdict | Marks each invoice match, within tolerance, or hold. | The three verdicts named on the tool |
| 7. Payable and hold | Returns the amount that can be paid and the amount on hold, with reason codes. | Reported by `payable_amount`, and by a batch or single-invoice match |
| 8. Units | Returns conversion suggestions. Confirmed factors are kept in `rules_snapshot`. | Caller-supplied factors |
| 9. The other jobs | Lists receipts not yet billed, and price and quantity variances, when that is the job. | `uninvoiced_receipts`, `variance_report` |
| 10. Decision cards | Puts the plain-language actions first. | Fixed output of every call |

**Why the answer does not change.** The hosted tool is marked idempotent. The steps above are a fixed method. No language model writes the match.

**What happens when an input is missing.** Nothing on the schema is required. If `job` is omitted, the tool runs what the inputs allow. A job that needs a document you did not send has nothing to match. `prior_invoices` matter when goods were already billed. Omit them and this page does not claim the engine knows that history.

**Where the working is shown.** Decision cards come first. Then line matches, the verdict, the payable amount, the hold with reason codes, conversion suggestions, uninvoiced receipts, and the variance report. `explain_match` is the job that explains one invoice. This page does not name the JSON keys for those fields. See the sample response note in [For developers](#for-developers).

<!-- TODO Saifs AI: confirm from a signed-in call or the engine script the payable formula, the reason-code strings, and what happens when two units have no conversion factor. The tool returns suggestions and a hold with reason codes. It does not list the codes. Do not claim a line is always held, or that payable equals the minimum of invoiced, received, and ordered times the order price, until the script shows it. -->

## Data handling and security

| Question | Local engine | Hosted tool |
| --- | --- | --- |
| Do purchase orders, receipts, or invoices reach Saifs AI? | No. The tool states that no data reaches the server on this path. | Yes. The rows passed in the call are sent for the match. |
| Where are they processed? | Your agent host's code sandbox | `three-way-match-mcp-microsoft.saifs.ai`, over HTTPS |
| What happens to the rows after the call? | They stayed in your sandbox. The script writes `three_way_match_result.json` and `rules_snapshot.json` there. | Not stated on the tool. See the note below. |
| What about the rules snapshot? | The caller keeps `rules_snapshot.json`. | The tool describes it as confirmed conversions from the last run. This page does not say the server stores it. |
| What can the script do? | It reads the local files named in the call and writes the two JSON files above. It needs `numpy`. | Not used on this path. |

<!-- TODO Saifs AI: confirm hosted-tool retention in writing before this sentence is made stronger. Say whether invoice, order, and receipt rows are discarded after the request, and whether any billing or audit log keeps a hash, the account, or the rows. Also confirm the hosting region, rate limits, and request timeout. Do not copy the audit-hash sentence from the Inventory Intelligence listing unless this server does the same. The public privacy page does not state row retention. -->

**What "sandbox" means.** The sandbox is the code-execution environment of your own agent host, such as Copilot Studio or Azure AI Foundry. It belongs to your tenant, not to Saifs AI. Your host's data policies still apply to it.

**Use the local engine only.** If these documents must not leave your environment, turn off the hosted tool. In Copilot Studio, open the agent's **Tools** tab, select the 3 Way Match:PO-Receipt-Invoice server, turn off **Allow all**, and switch off `three_way_match`. Leave `three_way_match_get_engine` on. The agent can then use only the local engine. A tool added to the server later stays off until you enable it. This setting is per agent.

**Sign-in.** Users sign in with Microsoft single sign-on. Saifs AI holds only the account details needed for sign-in and billing. No API key is created, shared, or stored.

**No write access.** The connector has no action that creates, changes, or deletes an invoice, a receipt, a purchase order, or a payment. Both tools are marked read-only and not destructive. The local engine is marked open-world because it reads files in the caller's sandbox and writes two JSON files there. It still does not write to an ERP.

**The script is returned in full.** Your security team can read it before allowing it to run. It is Python and needs `numpy`. Do not shorten it or rewrite it.

<!-- TODO Saifs AI: confirm whether the local script makes network calls. The tool says no data reaches the server on that path. This page does not claim the script itself makes no network calls. -->

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
| Customer document rows stored by the hosted tool | Not confirmed here. The local engine keeps rows in your sandbox. |
| Passwords held | None. Sign-in is Microsoft single sign-on. |
| Write access to your systems | None |
| Local engine | The script is returned in full for review before it runs. It needs `numpy`. |

**Independent audits.** Saifs AI does not yet hold an independently audited certificate such as SOC 2 or ISO 27001. If your review needs a completed security questionnaire, request it through the [contact page](https://saifs.ai/contact-us).

## Responsible use

This connector is a three-way match of purchase order, goods receipt, and supplier invoice. It recommends an amount to pay and an amount to hold. A person releases or holds. The connector does not.

Say "recommended" and "on hold." Do not say the payment was released, approved, or blocked. A verdict is a match result, not an instruction to the bank.

It is not an audit opinion, a regulatory determination, or accounting advice. Tolerances are your company's policy. The default price band is 2% only when you do not send `tolerances`.

Do not use it to reconcile a bank statement. Do not use it to find duplicate invoices. The tool states that both are out of scope.

There is no promise that every overbilling will be caught. A clear verdict means the documents matched at these settings. It does not mean the commercial deal was right.

## Token usage and cost

With the hosted tool, the rows are part of the tool call, so token use grows with the file. With the local engine, the agent receives a script and the rows stay in the sandbox, so token use does not grow with the file.

<!-- TODO Saifs AI: measure tokens before publishing figures. Use one named tokenizer on three sizes: a small file, the hosted limit (5,001 invoice rows, or 20,001 purchase-order, receipt, or prior-invoice rows), and a larger file. Count both directions. Include the script, the write into the sandbox, and the printed decision cards. Record input and output tokens, a dated reference price per million, the first-run and later-run multipliers, and a monthly example. State where the hosted tool uses fewer tokens. Do not copy the Inventory Intelligence token table. -->

Until those figures are measured, this page does not state a token count or a dollar cost.

**Which route to choose, before the token table exists**

- **Inside the hosted limits, and a single question:** the hosted tool. It is one call.
- **Over 5,001 invoice rows, or over 20,001 purchase-order, receipt, or prior-invoice rows:** the local engine. It is the only route.
- **The rows must stay in your environment:** the local engine. Turn the hosted tool off.
- **The same large file, many times in one session:** the local engine, if the sandbox keeps the script.

**Hosts that bill per action.** Some hosts charge a fixed amount per tool call and do not bill by token. On those hosts, a smaller token count may not reduce the bill. The local engine may also use one extra action to run the script. Choose it there for file size and for where the data stays, not for a lower token bill.

## Prerequisites

- A Saifs AI account, created by signing in with Microsoft single sign-on
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Access to Copilot Studio, Microsoft 365 Copilot, Azure AI Foundry, Power Automate, Power Apps, or Azure Logic Apps
- Purchase orders, goods receipts, and supplier invoices. Tolerances and confirmed unit conversions make the match follow your policy.
- For the local engine: code execution enabled on the agent host, and `numpy` available there

## How to get credentials

No keys or secrets are needed. When you create a connection, you are redirected to sign in with your Microsoft account and approve access. If the connection later expires, sign in again from the connection page.

## Get started with your connector

1. In Copilot Studio, open your agent and go to **Tools**, then **Add a tool**.
2. Search for **3 Way Match:PO-Receipt-Invoice** and select it.
3. Create a connection and sign in.
4. Add this to the agent's instructions: "For any question about matching a supplier invoice to a purchase order and a goods receipt, how much can be paid, what to hold, goods received but not invoiced, or price and quantity variances, call 3 Way Match:PO-Receipt-Invoice. Do not match the lines yourself. Do not release, block, or post a payment."
5. Provide the rows by pasting them, uploading them, or pointing the agent at a file. Include receipts already billed as `prior_invoices` when you have them. Send confirmed unit conversions when units differ.
6. Ask in plain language. Two examples are "How much of this invoice can we pay today?" and "List goods received but not yet invoiced."

Treat the result as a recommendation. A person checks the hold before a payment is released.

## For administrators

Enabling this connector gives the organisation one approved way to decide how much of a supplier invoice matches the order and the receipt. It does that inside the agents your makers already build.

**Why this is an administrator decision.** Without a match tool, makers ask an agent to compare three spreadsheets. A language model can give a different payable amount on the next run, and the result cannot be compared week to week. A governed connector uses one method for every agent and team.

- **You choose who can connect it.** Tenant and environment permissions apply. Only users who can create a connection can call the tools.
- **You keep your system of record.** The connector recommends and explains. It does not release a payment or write back to any system.
- **You can rerun the match.** The hosted tool is marked idempotent. Missing documents are not filled in from a previous call unless you send `rules_snapshot` or `prior_invoices`.
- **You control where data goes.** Turn off the hosted tool per agent to keep the documents in your environment.
- **You set data policy as usual.** Place the connector in the Business, Non-business, or Blocked group with Power Platform data loss prevention policies.
- **There are no secrets to rotate.** Expired connections are renewed by signing in again.

**Review checklist**

- Decide whether the hosted tool is allowed, or local engine only
- Assign the connector to the correct data policy group
- Review the privacy policy and terms
- Read the self-assessments listed under Security and compliance, once they are published
- Review the local engine script if your policy requires it
- Confirm that makers are told not to release a payment from the result alone

## For developers

Nothing is required on the schema. Send the documents the business has. The response is still a recommendation.

### Document fields

| Field | Meaning | Needed for |
| --- | --- | --- |
| `invoices` | Invoice rows, or headers with `lines`. Up to 5,001. | A batch match |
| `invoice` | One invoice, header plus lines, or its rows | A single-invoice match |
| `open_pos` | Purchase-order lines. Up to 20,001. | The ordered quantity, unit, and rate |
| `receipts` | Goods receipts. Up to 20,001. | The received quantity. Include rejected quantity when you have it. |
| `prior_invoices` | Invoices already booked on these orders. Up to 20,001. | Stopping a new bill from using goods already billed |
| `tolerances` | `price_pct`, `price_abs`, `qty_units`, `qty_pct`, `amount_abs`, `charge_abs`. Each is a number, minimum 0. | Your pass-or-hold policy |
| `unit_conversions` | Known factors, as an array or an object | A conversion you already trust |
| `confirmed_conversions` | Rows a person confirmed: supplier, item, from, to, factor. Up to 5,000. | The same conversion on the next run |
| `rules_snapshot` | The snapshot from the last run | Carrying confirmed conversions forward |
| `invoice_ref` | The invoice to explain | `explain_match` |
| `job` | One of the six jobs below | Choosing the report. Omit it to run what the inputs allow. |

### Jobs

| Job | What you are asking |
| --- | --- |
| `match_batch` | Match a set of invoices together, so no receipt is used twice |
| `match_invoice` | Match one invoice |
| `payable_amount` | The amount that can be paid now, and the hold |
| `explain_match` | Which lines one invoice used. Send `invoice_ref`. |
| `uninvoiced_receipts` | Goods received and not yet billed |
| `variance_report` | Price and quantity variances |

### Options and defaults

| Option | Default | Purpose |
| --- | --- | --- |
| `require_receipt` | true | False runs a two-way match against the purchase order, for a service with no goods receipt. |
| `allow_cross_po` | false | True allows a receipt booked on another purchase order of the same supplier. |
| `price_pct` | 2% (`2` or `0.02`) | Price band. The other tolerance fields have no default on the schema. |
| `currency_symbol` | Empty. The symbol on the cards follows the currency the data is written in. | A rupee symbol uses lakh and crore grouping. A dollar sign is also accepted. Display only. |
| `as_of` | Latest date seen | Ageing date for uninvoiced receipts |
| `date_order` | Not set on the schema | `DMY` or `MDY` for an ambiguous date |
| `mapping` | None | Header to standard field, when a column was read as the wrong field |
| `max_listed` | 200 | How many rows to list. From 1 to 100,000. |

<!-- TODO Saifs AI: confirm from a live call whether mixed currencies in one file are kept separate, converted, or refused. The schema describes a display symbol only. It does not describe a conversion. Also confirm defaults, if any, for price_abs, qty_units, qty_pct, amount_abs, and charge_abs. -->

### Sample request

```json
{
  "job": "payable_amount",
  "invoice": {
    "Invoice No": "INV-1044",
    "Supplier": "Northwind Supplies",
    "PO Number": "PO-220",
    "lines": [
      {
        "Item": "BOLT-10",
        "Description": "Bolt 10mm",
        "Qty": 100,
        "Unit": "pcs",
        "Rate": 51,
        "Amount": 5100
      }
    ]
  },
  "open_pos": [
    {
      "PO Number": "PO-220",
      "Supplier": "Northwind Supplies",
      "Item": "BOLT-10",
      "Qty": 100,
      "Unit": "pcs",
      "Rate": 50
    }
  ],
  "receipts": [
    {
      "GRN Number": "GRN-88",
      "PO Number": "PO-220",
      "Supplier": "Northwind Supplies",
      "Date": "2026-11-18",
      "Item": "BOLT-10",
      "Qty Received": 98,
      "Unit": "pcs"
    }
  ],
  "tolerances": {
    "price_pct": 2
  },
  "currency_symbol": "$",
  "as_of": "2026-11-24"
}
```

`job`, `invoice`, `open_pos`, `receipts`, `tolerances`, `currency_symbol`, and `as_of` are keys on `three_way_match`. The names inside each document are export headers. The schema does not fix those inner names. This sample shows the shape of a payable-amount check. It is not a result.

### Sample response (shortened)

A signed-in call was not available while this page was written. A call to `https://three-way-match-mcp-microsoft.saifs.ai/mcp` without a token returned HTTP 401 on 24 November 2026. No response body is shown here, because a real result was not returned.

<!-- TODO Saifs AI: replace this note with a shortened real response from three_way_match. Include one invoice verdict, the payable amount, the hold, and one reason code. Do not invent field names. Do not paste a response from another product. -->

The tool states what a result contains. Decision cards come first. Then line matches, a verdict per invoice (match, within tolerance, or hold), the payable amount, the hold with reason codes, unit-conversion suggestions, uninvoiced receipts, and a variance report. The local engine prints the decision cards. It writes the full set to `three_way_match_result.json` and `rules_snapshot.json`.

### Response sections

| What comes back | Use it for |
| --- | --- |
| Decision cards | The plain-language actions to read first. They are recommendations, not a payment. |
| Line matches | Which receipt and purchase-order lines an invoice line used |
| Verdict | Match, within tolerance, or hold, per invoice |
| Payable amount and hold | What a person may pay now, and what stays on hold, with reason codes |
| Unit-conversion suggestions | Factors to confirm before the next run |
| Uninvoiced receipts | Goods received and not yet billed |
| Variance report | Price and quantity differences by supplier and by item |
| `three_way_match_result.json` | The full local-engine result, written in the sandbox |
| `rules_snapshot.json` | Confirmed conversions, written in the sandbox, to send next time |

<!-- TODO Saifs AI: replace the first column with the JSON keys from a live response. -->

## Known issues and limitations

- **Recommendation only.** It does not release a payment, block a payment, or update any system.
- **Not an audit opinion.** A verdict is a match result for a person to review. It is not accounting advice.
- **Out of scope.** It does not reconcile a bank. It does not audit duplicate invoices.
- **The tools run on request.** They do not watch the ledger overnight, and they do not send an alert. Use a scheduled flow, and pass the rules snapshot back, for a repeated match.
- **Hosted tool size.** 5,001 invoice rows. 20,001 purchase-order rows, 20,001 receipt rows, and 20,001 prior-invoice rows. 5,000 confirmed conversions. Above that, use the local engine.
- **Price band.** `price_pct` defaults to 2%. Other tolerance fields apply only when you send them.
- **Units.** Suggestions come back for a person to confirm. This page does not state the result when no factor exists.
- **Receipts.** `require_receipt` defaults to true. A service bill needs it set to false.
- **Already billed goods.** If `prior_invoices` is left out, the tool is not given bills already posted against these orders.
- **Cross-order receipts.** `allow_cross_po` defaults to false.
- **Listed rows.** `max_listed` defaults to 200. Raise it, up to 100,000, when the hosted result must list more rows. The local engine writes the full result to `three_way_match_result.json`.
- **Currency.** The symbol labels the cards. A rupee symbol uses lakh and crore grouping. This page does not claim a conversion between currencies.
- **No catch-all promise.** A match verdict means the documents agreed at these settings.
- **Regions.** Not available in US Government (GCC, GCC High, DoD) or China (21Vianet) clouds.

<!-- TODO Saifs AI: rate limits, timeouts, and the service region are not in the tool schema. Add them here when confirmed. Also add the payable formula, the reason-code list, and the no-conversion behaviour once the engine script confirms them. -->

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| The tool cannot run, or the call returns unauthorized | The connection is missing or expired. A call with no token returned HTTP 401 on 24 November 2026. | Open the connection and sign in again |
| The match has nothing to compare | Invoices, open purchase orders, or receipts were not sent | Send all three. For one bill, `invoice` can replace `invoices`. |
| Goods already billed are matched again | `prior_invoices` was not sent | Send the invoices already booked on these orders |
| A service invoice is held for no receipt | `require_receipt` is still true, which is the default | Set `require_receipt` to false for a two-way match |
| A confirmed unit conversion did not apply | `rules_snapshot` or `confirmed_conversions` was omitted | Store the snapshot and send it on the next call |
| The agent states a payable amount without calling the tool | The instructions do not require the tool | Add the instruction in Get started |
| The hosted call refuses a large file | The file is over 5,001 invoice rows or 20,001 order, receipt, or prior-invoice rows | Call `three_way_match_get_engine` and run the script unchanged in the sandbox |
| The local engine does not run | Code execution is off, or the script was edited | Enable code execution, save the script exactly as returned, and install `numpy` in that sandbox |
| Dates land on the wrong day | An ambiguous date was read in the other order | Set `date_order` to `DMY` or `MDY` |

## FAQ

**Do I need an accountant to use this?**
No. A user asks in plain language, and the agent returns a match. Min-cost flow, subset-sum, and description similarity run inside the tool. Releasing the payment is still a person's job.

**What is the least data I need?**
Nothing is required to call the tool. A match needs the invoice, the purchase order, and the goods receipt. A service bill can omit the receipt when `require_receipt` is false.

**Does my data leave my environment?**
Not with the local engine. The tool states that no data reaches the server on that path. With the hosted tool, the rows in the call are sent to Saifs AI for the match. You can turn the hosted tool off.

**Will it change anything in my systems?**
No. It does not release a payment, block a payment, or write to an ERP. A person acts on the recommendation.

**I already have an ERP MCP server. Why add this?**
That server gives the agent your ERP's records and functions. This connector gives the agent the three-way match, and it works on rows from any export.

**Why do I get the same match every time?**
The hosted tool is marked idempotent, and the match is a fixed method. The same inputs return the same verdicts. The agent's sentences around the numbers may change.

**What happens if my data is incomplete?**
The tool still runs on what you sent. A job that needs a missing document has nothing to match. Confirmed conversions apply only when you send `rules_snapshot` or `confirmed_conversions`. The page does not add a default the schema does not state.

**Can I check how a result was produced?**
Yes. Ask for `explain_match` with `invoice_ref`. The result is the line match and the reason for a hold. It is not a payment.

**How large a file can it handle?**
The hosted tool stops at the limits in At a glance. Larger files use the local engine. `max_listed` (default 200) caps how many rows the hosted result lists.

**Can it run on a schedule?**
Yes. The tools run on request. Call the connector from a Power Automate flow with a recurrence trigger, and store the rules snapshot between runs. The connector does not send the alert itself.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment). Microsoft Premium connector licensing applies separately.

**Does a match verdict mean we should pay the invoice?**
No. It means the documents matched, sat inside tolerance, or should be held, at these settings. A person releases or holds.

## Support

**3 Way Match:PO-Receipt-Invoice - Saifs AI** is published by Saifs AI.

- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Privacy: [https://saifs.ai/privacy_policy/three-way-match](https://saifs.ai/privacy_policy/three-way-match)
- Terms: [https://saifs.ai/eula](https://saifs.ai/eula)
- MCP endpoint: `https://three-way-match-mcp-microsoft.saifs.ai/mcp`

<!-- OPEN QUESTIONS
1. Hosted-tool retention, any audit or billing hash, hosting region, rate limits, and timeout. Not stated on the live tool schema.
2. Whether the local engine script makes network calls. The tool states that no data reaches the server on that path.
3. A shortened real response, including JSON field names, from a signed-in call. The endpoint returned HTTP 401 without a token on 24 November 2026.
4. Engine version field name, if one exists. Do not copy metadata.model_version from Inventory Intelligence.
5. The payable formula and the reason-code strings. The tool says a payable amount and a hold with reason codes. It does not list the codes.
6. What the engine does when two units have no conversion factor. It returns suggestions. It does not state that the line is held.
7. Defaults, if any, for price_abs, qty_units, qty_pct, amount_abs, and charge_abs. Only price_pct is described as defaulting to 2%.
8. Named header aliases, and the response field that reports a mapping. Not listed in the schema.
9. Whether mixed currencies in one file are kept separate, converted, or refused. No conversion is claimed.
10. Token counts. Not measured. Do not copy another product's table.
11. Product page URL. https://saifs.ai/three-way-match returned HTTP 404 on 24 November 2026.
-->
