# 13-Week Cash Flow Forecast - Saifs AI

**13-Week Cash Flow Forecast - Saifs AI** helps finance teams answer **will we have enough cash** over the next 13 weeks. You send open receivables, how customers have paid, payables, payroll, and the cash you hold today. It returns cash in, cash out, and a closing balance for each week, as a range.

It is built for CFOs, controllers, founders, and treasury teams. They still build a cash flow forecast from due dates, and they need one they can rerun every week.

**In plain language:** you give it the ageing export you already have. It says whether cash covers the next 13 weeks, and which customers move that week.

**For technical readers:** the cash flow forecast uses receipts and disbursements. Days-to-pay use Kaplan-Meier where invoices are still open. The balance is a Monte Carlo simulation.

It recommends and does not act. It does not collect an invoice, pay a bill, or move cash. Every figure is an estimate, shown as a range. It is not financial advice. It does not compute interest, borrowing, or investments.

**Publisher:** Saifs AI
**Website:** [https://saifs.ai](https://saifs.ai)
**Pricing:** [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
**Contact and help:** [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
**Privacy:** [https://saifs.ai/privacy_policy/13-week-cash-flow-forecast](https://saifs.ai/privacy_policy/13-week-cash-flow-forecast)
**Terms:** [https://saifs.ai/eula](https://saifs.ai/eula)
**Sign-in:** Microsoft single sign-on (OAuth 2.0). No API keys.
**MCP endpoint:** [https://cash-forecast-mcp-microsoft.saifs.ai/mcp](https://cash-forecast-mcp-microsoft.saifs.ai/mcp)

<!-- TODO Saifs AI: publish the product page, then add a Learn more line here and in Support. https://saifs.ai/13-week-cash-flow-forecast, https://saifs.ai/cash-forecast, and https://saifs.ai/13-week-cash-forecast each returned HTTP 404 on 17 November 2026. Do not link one until it opens. The privacy URL above returned HTTP 200, and the same response size was returned for an unknown privacy slug on that date. Confirm it is this product's policy. -->

| If you are | Start with |
| --- | --- |
| A business owner, CFO, or finance lead | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [Where the data comes from](#where-the-data-comes-from) |
| An administrator or security reviewer | [Data handling and security](#data-handling-and-security), [Security and compliance](#security-and-compliance), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [Where the data comes from](#where-the-data-comes-from), [For developers](#for-developers) |
| A finance reviewer checking the method | [Inside the calculation](#inside-the-calculation) |

## The problem it solves

Most teams still build the 13-week cash forecast by putting each invoice on its due date. Customers do not pay that way. Some pay on time, some two weeks late, some much later. The cash flow forecast is already wrong when it is emailed.

An ageing report shows what is open. It does not show when cash will arrive, so working capital is misread. Treasury forecasting from that report has the same gap. A language model asked to sum invoices by week can return one number, with no range, and with no memory of how each customer pays.

13-Week Cash Flow Forecast makes that cash forecasting available to any AI agent:

- **Will we have enough cash.** A liquidity forecast of cash in, cash out, and closing balance for 13 weeks, as P10, P50, and P90.
- **Where the risk sits.** The chance of falling below a minimum cash level, and the earliest risky week.
- **What moves the week.** The customers and bills behind a week, including invoices a due-date view would count early.
- **What if a payer slips.** A late customer, a delayed supplier, or an extra inflow, without rebuilding the sheet.
- **Whether last week was right.** Accuracy against the previous cash forecast, when you send last week's result and this week's actuals.

## Why choose it: the same ranges every time

13-Week Cash Flow Forecast is deterministic. The same ledger and the same settings, including the same seed, produce the same ranges. The figures come from fixed cash forecasting methods. They are not written afresh by a language model each time you ask.

That matters for treasury forecasting. A board, a lender, or a founder acts on working capital. The range has to match when the file has not changed. The agent's wording around the numbers may vary. The numbers do not.

| What it does | Established method |
| --- | --- |
| Builds week-by-week cash in, cash out, and closing balance | Receipts and disbursements |
| Times the receivables forecast from how that customer has paid | Kaplan-Meier days-to-pay, with open invoices treated as censored |
| Turns many possible receipt timings into P10, P50, and P90 | Monte Carlo simulation of the cash balance |
| States the chance of falling below a cash cushion | Breach probability against the minimum cash you set |
| Checks this forecast against what cash actually did | Accuracy against the previous forecast, when you send actuals |

**It does not guess a single cash date.** Payment history can be too short to time receipts. Under 90 days, or under 20 paid invoices, with no snapshot, the forecast is labelled terms-based. That means due dates only. The label is stated. A bare total is not filled in.

**It recommends and does not act.** It never collects cash, pays a supplier, or writes to an ERP or accounting system. Loan repayments are only the fixed amounts you supply. No interest is computed.

## Who should use this

| Role | How it helps |
| --- | --- |
| **CFOs and finance controllers** | A 13-week cash forecast for the Monday pack, with a range on every week and last week's accuracy when you send it |
| **Founders of SMBs and startups** | A receivables forecast for week 9: whether payroll and tax dates clear, and which customers drive it |
| **Treasury analysts** | Treasury forecasting of liquidity from open receivables, payables, and the company's own payment pattern |
| **Fractional CFOs and CA firms** | The same cash flow forecast for each client company, with that client's snapshot kept beside the file |
| **FP&A teams** | A liquidity forecast they can rerun after a what-if, such as a large customer paying three weeks late |
| **Business owners** | Working capital shown as cash in, cash out, and closing balance, without a treasury system to configure |
| **Agent makers and developers** | A tested cash forecasting calculation inside an agent, so the agent does not sum invoices by due date |

**Best fit:** finance teams, SMB and startup founders, treasury desks, fractional CFOs, and CA firms that already export receivables and payables.

**Not required:** a treasury workstation, a data science team, or an ERP planning module. An ageing export is enough to start. Payment history makes the timing sharper.

## Use cases

Each use case below can be built as an agent. The agent types are listed in [Agents you can build](#agents-you-can-build).

**1. Will we have enough cash in a named week**
Scenario: payroll and a tax date land in the same week, and the due-date sheet looks fine.
Outcome: a liquidity forecast for that week, as P10, P50, and P90, plus the chance of falling below minimum cash.
Ask: "Will we have enough cash in week 9?"

**2. Cash forecast for the board**
Scenario: the controller does the treasury forecasting for Monday's board pack.
Outcome: a 13-week cash forecast of working capital, as cash in, cash out, and closing balance, with a range on every week.
Ask: "Build our 13-week cash forecast for the board."

**3. A customer who pays late**
Scenario: the largest customer often pays about three weeks after the due date.
Outcome: a what-if on that delay, and the weeks whose closing balance moves.
Ask: "What if our top customer pays 3 weeks late?"

**4. When will customers pay**
Scenario: someone asks "When will customers pay?" before they trust a full cash flow forecast.
Outcome: days-to-pay per customer. A customer with little history borrows the segment pattern. Open invoices with no paid date stay in the history as censored.
Ask: "When will customers pay, from how they have paid before?"

**5. Explain one week**
Scenario: week 6 looks tight, and the board will ask which invoices are really in that week.
Outcome: receipts by customer, the payments, and invoices a due-date view counts that payment behaviour says will slip.
Ask: "Explain week 6 of this cash forecast."

**6. Accuracy against last week**
Scenario: last week's forecast was saved, and this week's bank movement is known.
Outcome: accuracy of that cash forecast against actual cash in, cash out, and closing balance. Keep the new snapshot for next time.
Ask: "How did last week's cash forecast compare with actual cash?"

**7. Payroll and statutory dates**
Scenario: GST, TDS, PF, ESI, or payroll, not customer receipts, is what empties the account.
Outcome: those amounts on their dates, inside cash out. A loan repayment is included only as the fixed amount you send.
Ask: "Include GST, TDS, and payroll in the 13-week cash forecast."

**8. One forecast per client company**
Scenario: a fractional CFO or a CA firm repeats the same cash forecasting pack across clients.
Outcome: one 13-week cash forecast per company. Store each payment behaviour snapshot with that company.
Ask: "Build this client's 13-week cash forecast."

**9. A large receivables export**
Scenario: the file is over about 5,000 open invoices, 50,000 history rows, or 5,000 bills.
Outcome: the same cash flow forecast runs in the agent's sandbox. The rows stay there.
Ask: "Run the cash forecast on this full receivables export."

## Agents you can build

Each agent is one agent in your agent host with this connector added as a tool. The last column shows what else to add.

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Weekly cash agent | Runs treasury forecasting for the next 13 weeks, with cash in, cash out, and closing ranges | A receivables file, or a data source from [Where the data comes from](#where-the-data-comes-from) | 1, 2 |
| Minimum-cash agent | Turns the liquidity forecast into a breach chance, and names the earliest risky week | `minimum_cash` in the same call | 1 |
| Late-payer agent | Reruns the forecast with a customer delay, a supplier delay, or an extra inflow | A `scenarios` entry, such as `customer_delay` | 3 |
| Payment-behaviour agent | Returns days-to-pay per customer from history alone | Payment history, including invoices that are still open | 4 |
| Week-explanation agent | Lists the receipts and bills behind one week | `explain_week` set to that week number | 5 |
| Forecast-accuracy agent | Compares the previous cash forecast with actual cash | Last week's result, this week's actuals, and the stored snapshot | 6 |
| Payroll and dues agent | Places payroll and statutory amounts on their dates inside cash out | Payroll rows and statutory rows, such as GST or TDS | 7 |
| Large-ledger agent | Runs the local engine on a full export | Code execution on the agent host, and `numpy` | 9 |
| ERP cash agent | Reads open items from the ERP, then returns the cash flow forecast for a person to read | The ERP's own MCP server. This connector does not write the ERP. | 1, 2 |
| Monday forecast agent | Sends the 13-week cash forecast on a recurrence | A Power Automate flow with a recurrence trigger, plus email or Teams. Store the snapshot between runs. | 2, 6 |

The tools run on request. Scheduling comes from the flow, not from the connector. There is no alert inside the server.

## At a glance

| Item | Detail |
| --- | --- |
| Tools | `cash_forecast` (hosted calculation) and `cash_forecast_get_engine` (local engine) |
| Jobs inside `cash_forecast` | Chosen by what you send: the forecast, breach risk (`minimum_cash`), one week (`explain_week`), what-ifs (`scenarios`), days-to-pay (`ar_history` alone), and accuracy (`previous_forecast` plus `actuals`) |
| Minimum input | Nothing is required. Send whatever columns you have. |
| Hosted tool size | 5,000 open invoices, 50,000 history rows, 5,000 open bills, 20,000 paid bills, 5,000 actuals, 10 scenarios, and 1,000 rows each for payroll, recurring items, statutory dues, fixed payments, fixed receipts, and new sales |
| Larger files | Local engine. It runs in your agent's code sandbox and needs only `numpy`. Paths can go up to 50,000. |
| Writes to your systems | None. Both tools are marked read-only and not destructive. |
| Pricing | [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment) |
| Engine version | Not named in the tool schema. |

<!-- TODO Saifs AI: if a live response includes an engine version field, name it in At a glance. Do not copy metadata.model_version from another product. -->

## Where the data comes from

13-Week Cash Flow Forecast does not connect to your systems. The agent brings the rows to it. Any source that can supply receivables, payables, or payroll can feed it. The connector needs only rows. It has not been certified against each system named below. There is no bank connection in this connector.

| Data source | How the agent gets the data | Best route |
| --- | --- | --- |
| Spreadsheet or text export from accounting software (for example Tally, Zoho, or SAP ageing reports) | The user uploads the file to the agent | Local engine for large files, hosted tool for files inside the limits |
| An ERP that has its own MCP server (for example Dynamics 365 Finance, Business Central, SAP, or NetSuite) | The agent reads open items with the ERP's tools, then passes the rows on | Hosted tool up to the limits above. Above that, export a file and use the local engine. |
| Dataverse tables | The Dataverse MCP server or the Dataverse connector | Same as above |
| SQL database, Excel workbook online, or SharePoint list | A Power Platform connector on the same agent or flow | Same as above |
| A scheduled flow | A Power Automate flow reads the source on a schedule and calls this connector | Hosted tool, if the file is inside the limits |
| The previous run of this connector | Pass `payment_behaviour_snapshot` from the last result. The caller stores it. | Either route |

**Works alongside your ERP's MCP server.** That server gives the agent the ERP's records and functions. This connector adds the cash flow forecast. No treasury module has to be configured. The ERP supplies the rows. 13-Week Cash Flow Forecast returns the ranges. A person decides what to pay or collect. Neither tool writes back.

**What to pull from the source**

| Data in your system | Field it fills | Why it matters |
| --- | --- | --- |
| Open receivables / ageing | `ar_open` | The receivables forecast. Timing uses payment behaviour, not the due date alone. |
| How customers paid | `ar_history` | Days-to-pay. Include old invoices that are still open, with no paid date. They count as censored. |
| Open payables | `ap_open` | Bills still to pay |
| Bills already paid | `ap_history` | The company's own days-to-pay, once 10 bills have been paid |
| Payroll | `payroll` | A date and amount, or a weekly, fortnightly, or monthly schedule |
| Repeating items | `recurring` | Rent and similar items, in or out |
| Statutory dues | `statutory` | GST, TDS, PF, ESI, and the other types named on the tool, on their template dates |
| Fixed payments | `fixed_outflows` | Loan repayments and other fixed amounts you already know. No interest is computed. |
| Cash today | `opening_balance` | The starting point for the closing balance |
| Cash cushion | `minimum_cash` | Turns on the chance of a breach, and the earliest risky week |
| Last run | `payment_behaviour_snapshot`, `previous_forecast`, `actuals` | Continues the pattern, and scores last week's cash forecast |

**Column names can stay as they are.** The tool tells the agent to send Tally, Zoho, SAP, and ageing-report headers as they are. These aliases are named on the tool.

| Field | Headers recognised (from the tool schema) |
| --- | --- |
| `customer_ref` | customer, party, debtor, customer_name. A pseudonymous reference is accepted. |
| `invoice_ref` | invoice, invoice_no, bill_no, doc_no |
| `amount` on a receivable | balance, outstanding, amount_due, open_amount, invoice_amount |
| `due_date` | When missing, send `invoice_date` plus `terms`. Aliases for terms: terms_days, credit_days, payment_terms. |
| `paid_date` | payment_date, settled_date, receipt_date. Leave it empty when the invoice is still open. |
| `supplier_ref` | supplier, vendor, creditor, payee |
| `schedule` | frequency |
| `statutory.type` | gst, tds, pf, esi, pt, advance_tax, vat, payroll_tax, sales_tax |

**File formats for the local engine.** CSV, TSV, or JSON. Each list has its own path, such as `ar_open_path` and `ar_history_path`. Save an Excel sheet as CSV first, or ask the agent to convert it in the sandbox. The script writes `cash_forecast_result.json` and `payment_behaviour_snapshot.json`.

## How it works

The connector offers two routes to the same cash flow forecast. The agent picks the route by the size of the file.

| | Hosted tool | Local engine |
| --- | --- | --- |
| Tool name | `cash_forecast` | `cash_forecast_get_engine` |
| Use when | Inside the row caps in At a glance, and 20,000 simulation paths or fewer | A larger ledger, more than 20,000 paths, or when the rows must stay in your environment |
| What the agent sends | The rows and the settings | File paths and settings only |
| What comes back | The weekly ranges and the other results the inputs allow | A ready-to-run Python script |
| Where the calculation runs | Saifs AI endpoint | Your agent's code-execution sandbox |
| Where the ledger rows go | To the Saifs AI endpoint for the calculation | The tool states that the data never leaves the user's machine on this route |
| Requires | Nothing extra | An agent host with code execution, and `numpy` |

`cash_forecast` is one tool. The research plan described six separate tools. Those names are not on the server. The job is chosen by the fields you send.

**Local engine, step by step**

1. The user uploads the files, or points the agent at them.
2. The agent calls `cash_forecast_get_engine` with the paths and any settings.
3. The connector returns the calculation script. Save it exactly as given. Do not shorten it, retype it, or replace it with other code.
4. The agent runs the script in its sandbox against the files.
5. The script prints the decision cards and the weekly P10, P50, and P90 table. It writes `cash_forecast_result.json` and `payment_behaviour_snapshot.json` in the sandbox.
6. The agent reports the ranges to the user. Keep the snapshot file and pass it on the next run.

### Inside the connector

1. The user asks whether they will have enough cash. They may also ask for a 13-week cash forecast, a late-payer what-if, or last week's accuracy.
2. The agent chooses a tool and calls the connector. The call goes to the Saifs AI MCP server over HTTPS at `https://cash-forecast-mcp-microsoft.saifs.ai/mcp`.
3. The server checks the signed-in account.
4. The hosted tool runs the cash forecasting calculation and returns the result. The engine tool returns the same calculation as a script, for the agent to run in its sandbox.
5. The agent reads the result and explains it in plain language. The ranges come from the engine. The wording comes from the agent.

### Inside the calculation

Both routes run the same calculation. A part of it runs when its inputs are in the call.

| Step | What the engine does | Method |
| --- | --- | --- |
| 1. Read the export | Keeps the column names you sent. `date_order` of `DMY` or `MDY` sets how an ambiguous numeric date is read. Omit it and the tool detects the order. | Stated on the tool |
| 2. Days-to-pay | For each paid invoice, delay is paid date minus due date. An old invoice with no paid date is censored. A part payment can be sent as `paid_amount`. A customer with few paid invoices is blended toward the segment. `shrinkage_strength` defaults to 5. | Kaplan-Meier days-to-pay for censored invoices, plus the paid-invoice pattern |
| 3. Thin history | If history is shorter than `min_history_days` (default 90) or there are fewer than 20 paid invoices, and there is no snapshot, the forecast is labelled terms-based. That label means due dates only. | Stated on the tool |
| 4. Time the receipts | Open invoices, and `new_sales` when you send them, are the receivables forecast. They are placed in weeks from that customer's days-to-pay. A week explanation lists invoices a due-date view would count when behaviour says they will slip. | Receipts side of receipts and disbursements |
| 5. Time the payments | With `payables_policy` left at `history_or_due`, bills follow the company's own payment pattern once 10 bills are paid. `due_date` pays every bill on its due date. Payroll follows the date or the schedule. Statutory dues use template days unless `day` is set: GST on the 20th, TDS on the 7th, PF and ESI on the 15th, advance tax on 15 June, 15 September, 15 December, and 15 March. | Disbursements, plus the caller's fixed amounts |
| 6. Fixed amounts | `fixed_outflows` and `fixed_inflows` land on the date or week you send. A loan repayment is one of those amounts. Interest is not computed. | Caller-supplied amounts |
| 7. Simulate the balance | Draws `n_paths` receipt timings. The default is 5,000. The hosted tool allows 200 to 20,000. The local engine allows up to 50,000. Each week returns a low, middle, and high closing balance: P10, P50, and P90. | Monte Carlo |
| 8. Breach | When `minimum_cash` is set, each week includes the chance of falling below it, and the earliest risky week. A week is called a breach risk at `breach_threshold` (default 0.10). | Probability against the cushion |
| 9. Explain a week | `explain_week` returns receipts by customer, the payments, and the invoices that slip versus a due-date view. | The same timed receipts and disbursements |
| 10. What-if | Up to 10 `scenarios`. Types named on the tool: `customer_delay`, `supplier_delay`, `inflow`, `outflow`, `customer_no_pay`, `customer_on_time`, `collections_faster`, `all_customers_delay`. | The same balance, with one change applied |
| 11. Accuracy | When `previous_forecast` and `actuals` are both sent, the result includes accuracy against that forecast. | Comparison with the prior run |
| 12. Snapshot and cards | Returns `payment_behaviour_snapshot` with days-to-pay quantiles only. No invoices and no amounts are in that object. The caller stores it. Decision cards and `max_listed` (default 200) cap how much detail is listed. | Caller-held snapshot |

**Why the answer does not change.** The steps above are fixed. There is no language model in the calculation. The Monte Carlo step uses `seed` (default 7). The tool states that the same ledger and the same seed give the same ranges. A new snapshot or a new seed is a new input. The agent's sentences around the ranges may change.

**What happens when data is missing.** Nothing is required, so the call still runs. The tool reports what was missing. Defaults in the table above are the ones on the schema. They are listed again under [Options and defaults](#options-and-defaults). This page does not add a default the schema does not state.

**Where the working is shown.** Decision cards carry the plain-language action. `explain_week` is how one week is opened up. This page does not name the JSON keys inside a hosted result. The local engine writes the full detail to `cash_forecast_result.json`.

<!-- TODO Saifs AI: confirm from a signed-in call, or from the returned script, the exact week-spread formula, whether overdue invoices use a conditional days-to-pay, and which accuracy metrics are returned (the research names MAPE, bias, and a tracking signal; the tool schema says "accuracy" only). Do not publish those names until the script or a real response shows them. -->

## Data handling and security

| Question | Local engine | Hosted tool |
| --- | --- | --- |
| Do receivables, payables, or payroll rows reach Saifs AI? | No. The tool states that the data never leaves the user's machine on this route. | Yes. The rows passed in the call are sent for the calculation. |
| Where are they processed? | Your agent host's code sandbox | `cash-forecast-mcp-microsoft.saifs.ai`, over HTTPS |
| What happens to the rows after the call? | They stayed in your sandbox. The script writes `cash_forecast_result.json` and `payment_behaviour_snapshot.json` there. | Not stated on the tool, beyond the snapshot. See the note below. |
| What about the payment behaviour snapshot? | The caller keeps `payment_behaviour_snapshot.json`. It holds days-to-pay quantiles only. | The caller is told to store `payment_behaviour_snapshot` and send it next week. Quantiles only. No invoices and no amounts. |
| What can the script do? | It reads the local files and writes the two JSON files above. It needs `numpy`. The engine tool is marked open-world. | Not used on this path. |

<!-- TODO Saifs AI: confirm hosted-tool retention in writing before this sentence is made stronger. Say whether ledger rows are discarded after the request, and whether any billing or audit log keeps a hash, the account, or the rows. Also confirm the hosting region, rate limits, and request timeout. Do not copy the audit-hash sentence from the Inventory listing unless this server does the same. -->

**What "sandbox" means.** The sandbox is the code-execution environment of your own agent host, such as Copilot Studio or Azure AI Foundry. It belongs to your tenant, not to Saifs AI. Your host's data policies still apply to it.

**Use the local engine only.** If ledger rows must not leave your environment, turn off the hosted tool. In Copilot Studio, open the agent's **Tools** tab, select the 13-Week Cash Flow Forecast server, turn off **Allow all**, and switch off `cash_forecast`. Leave `cash_forecast_get_engine` on. The agent can then use only the local engine. A tool added to the server later stays off until you enable it. This setting is per agent.

**Sign-in.** Users sign in with Microsoft single sign-on. Saifs AI holds only the account details needed for sign-in and billing. No API key is created, shared, or stored.

**No write access.** The connector has no action that creates, changes, or deletes an invoice, a bill, a payment, or a journal. Both tools are marked read-only and not destructive.

**The script is returned in full.** Your security team can read it before allowing it to run. It is Python and needs `numpy`. Do not shorten it or rewrite it.

<!-- TODO Saifs AI: confirm whether the local script makes network calls. The tool says the ledger data never leaves the user's machine on that route. The engine tool is marked open-world, so this page does not claim the script itself makes no network calls. -->

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
| Customer ledger rows stored by the hosted tool | Not confirmed here. The snapshot the caller keeps has quantiles only. The local engine keeps rows in your sandbox. |
| Passwords held | None. Sign-in is Microsoft single sign-on. |
| Write access to your systems | None |
| Local engine | The script is returned in full for review before it runs. It needs `numpy`. |

**Independent audits.** Saifs AI does not yet hold an independently audited certificate such as SOC 2 or ISO 27001. If your review needs a completed security questionnaire, request it through the [contact page](https://saifs.ai/contact-us).

## Token usage and cost

With the hosted tool, the rows are part of the tool call, so token use grows with the file. With the local engine, the agent receives a script and the rows stay in the sandbox, so token use does not grow with the file.

<!-- TODO Saifs AI: measure tokens before publishing figures. Use one named tokenizer on three sizes: a small ledger, the hosted limit (5,000 open invoices, 50,000 history rows, or 5,000 bills), and a larger file. Count both directions. Include the script, the write into the sandbox, and the printed cards and weekly table. Record input and output tokens, a dated reference price per million, the first-run and later-run multipliers, and a monthly example. State where the hosted tool uses fewer tokens. Do not copy the Inventory token table. -->

Until those figures are measured, this page does not state a token count or a dollar cost.

**Which route to choose, before the token table exists**

- **Inside the hosted limits, and a single question:** the hosted tool. It is one call.
- **Above the row caps in At a glance, or above 20,000 paths:** the local engine. It is the only route.
- **The rows must stay in your environment:** the local engine. Turn the hosted tool off.
- **The same large file, many times in one session:** the local engine, if the sandbox keeps the script.

**Hosts that bill per action.** Some hosts charge a fixed amount per tool call and do not bill by token. On those hosts, a smaller token count may not reduce the bill. The local engine may also use one extra action to run the script. Choose it there for file size and for where the data stays, not for a lower token bill.

## Prerequisites

- A Saifs AI account, created by signing in with Microsoft single sign-on
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Access to Copilot Studio, Microsoft 365 Copilot, Azure AI Foundry, Power Automate, Power Apps, or Azure Logic Apps
- Open receivables, payables, payroll, or payment history. Cash on hand makes the closing balance usable.
- For the local engine: code execution enabled on the agent host, and `numpy` available there

## How to get credentials

No keys or secrets are needed. When you create a connection, you are redirected to sign in with your Microsoft account and approve access. If the connection later expires, sign in again from the connection page.

## Get started with your connector

1. In Copilot Studio, open your agent and go to **Tools**, then **Add a tool**.
2. Search for **13-Week Cash Flow Forecast** and select it.
3. Create a connection and sign in.
4. Add this to the agent's instructions: "For a cash question, a 13-week cash forecast, when customers will pay, or last week's accuracy, call 13-Week Cash Flow Forecast. Do not sum invoices by due date. Report every figure as a range."
5. Provide the rows by pasting them, uploading them, or pointing the agent at a file. Include `minimum_cash` if you want breach risk. Include `payment_behaviour_snapshot` if you are continuing last week's pattern.
6. Ask in plain language, for example "Will we have enough cash in week 9?" or "Build our 13-week cash forecast for the board."

Treat the result as decision support. A person checks the range before moving cash, delaying a supplier, or telling a board that a week is safe.

## For administrators

Enabling this connector gives the organisation one approved way to answer whether the business will have enough cash. It does that inside the agents your makers already build.

**Why this is an administrator decision.** Without a calculation tool, makers ask an agent to sum an ageing export by week. A language model can give a different figure on the next run, and the result cannot be compared week to week. A governed connector uses one method for every agent and team.

- **You choose who can connect it.** Tenant and environment permissions apply. Only users who can create a connection can call the tools.
- **You keep your system of record.** The connector forecasts and explains. It does not collect cash, pay bills, or write back to any system.
- **You can check the range.** The same file, settings, and seed can be run again. A thin history is labelled terms-based, not filled in with a silent date.
- **You control where data goes.** Turn off the hosted tool per agent to keep ledger rows in your environment.
- **You set data policy as usual.** Place the connector in the Business, Non-business, or Blocked group with Power Platform data loss prevention policies.
- **There are no secrets to rotate.** Expired connections are renewed by signing in again.

**Review checklist**

- Decide whether the hosted tool is allowed, or local engine only
- Assign the connector to the correct data policy group
- Confirm that customer references can stay pseudonymous where the review requires it
- Review the privacy policy and terms
- Read the self-assessments listed under Security and compliance, once they are published
- Review the local engine script if your policy requires it
- Confirm the hosted-tool retention note in Data handling and security before a wide rollout

## For developers

No field is required. Send the arrays for the jobs you want. Other jobs run only when their inputs are present. For a file over the hosted limits, call `cash_forecast_get_engine` instead.

Cash forecasting on this connector is the ranges from `cash_forecast`. It is not a posting to the ledger.

### Receivable fields

`ar_open` allows 5,000 objects on the hosted tool. `ar_history` allows 50,000.

| Field | Meaning | Needed for |
| --- | --- | --- |
| `customer_ref` | Who owes the invoice. A pseudonymous reference is accepted. | Days-to-pay and the week explanation |
| `invoice_ref` | Invoice number | Tying a receipt to an invoice |
| `amount` | Amount still open, or the invoice amount on a history row | The size of the receipt |
| `due_date` | When the invoice is due. YYYY-MM-DD is preferred. | The delay, and the terms-based fallback |
| `invoice_date`, `terms` | Used together when `due_date` is absent. `default_terms_days` is 30. | A due date the tool can time |
| `segment` | Customer group | The pattern borrowed when that customer has little history |
| `paid_date` | When a history invoice was paid. Empty if it is still open. | Days-to-pay. Empty dates are censored. |
| `paid_amount` | Amount received when it is a part payment | The cash that actually arrived |

### Payables, payroll, and other lists

| Input | Limit on the hosted tool | Needed for |
| --- | --- | --- |
| `ap_open` | 5,000 bills. `supplier_ref`, `bill_ref`, `amount`, `due_date`. | Cash out |
| `ap_history` | 20,000 paid bills. Needs `paid_date`. | The company's own days-to-pay, once 10 bills are paid |
| `payroll` | 1,000. A date and amount, or `schedule` plus `day`. | Pay runs. `schedule` may be weekly, fortnightly, or monthly. |
| `recurring` | 1,000. `name`, `amount`, `schedule`, `day` or `start`, `direction`. | Repeating inflows or outflows. Direction defaults to out. |
| `statutory` | 1,000. `type` plus `amount`, optional `day`. | Template dates for GST, TDS, PF, ESI, and the other types on the tool |
| `fixed_outflows`, `fixed_inflows` | 1,000 each. `date` or `week`, `amount`, `label`. | Loan repayments and other amounts you already know |
| `new_sales` | 1,000. `date` or `week`, `amount`, optional customer or segment, and terms. | Expected invoices, timed like open invoices |
| `actuals` | 5,000. `date` or `week`, and `cash_in`, `cash_out`, `closing`, or a signed `amount`. | Accuracy against `previous_forecast` |
| `scenarios` | 10 | What-ifs. See the type list under Options. |

### Options and defaults

| Option | Default | Purpose |
| --- | --- | --- |
| `opening_balance` | None | Cash on hand at `as_of`. Without it, a closing balance has no start. |
| `minimum_cash` | None | Turns on breach risk for each week |
| `as_of` | Today | Forecast start, YYYY-MM-DD. Weeks run 7 days from this date. |
| `weeks` | 13 | How many weeks. From 1 to 52. |
| `payment_behaviour_snapshot` | None | Quantiles from an earlier run. Store it and send it next week. |
| `previous_forecast` | None | An earlier result, or its weeks list. Send it with `actuals`. |
| `explain_week` | None | Week number to open, from 1 to 52 |
| `n_paths` | 5000 | Monte Carlo paths. Hosted tool: 200 to 20,000. Local engine: up to 50,000. |
| `seed` | 7 | Same ledger and same seed, same ranges. Minimum 0. |
| `breach_threshold` | 0.10 | Probability at which a week is called a breach risk. From 0.01 to 0.99. |
| `date_order` | Omit, and the tool detects it | `DMY` or `MDY` |
| `default_terms_days` | 30 | Credit days when an invoice has neither a due date nor terms. From 0 to 365. |
| `shrinkage_strength` | 5 | How strongly a thin customer history is blended toward the segment. From 0 to 100. |
| `min_history_days` | 90 | History shorter than this, with no snapshot, is labelled terms-based. From 0 to 3,650. |
| `payables_policy` | `history_or_due` | Or `due_date`, to pay every bill on its due date |
| `currency` | INR | Display label only. No conversion. |
| `currency_symbol` | None | Symbol on the cards, when you set one |
| `max_listed` | 200 | How many customers, bills, or drivers to list. From 1 to 100,000. |

<!-- TODO Saifs AI: confirm from a live call whether mixed currencies in one file are kept separate, converted, or refused. The schema describes a currency code and says there is no FX conversion. Also confirm whether a missing opening_balance returns a movement-only table or a refusal. -->

### Sample request

```json
{
  "ar_open": [
    {
      "customer_ref": "CUST-14",
      "invoice_ref": "INV-2201",
      "amount": 180000,
      "due_date": "2026-11-20",
      "segment": "Wholesale"
    }
  ],
  "ar_history": [
    {
      "customer_ref": "CUST-14",
      "invoice_ref": "INV-1904",
      "due_date": "2026-08-01",
      "paid_date": "2026-08-22",
      "amount": 150000
    }
  ],
  "ap_open": [
    {
      "supplier_ref": "SUP-3",
      "bill_ref": "BILL-88",
      "amount": 42000,
      "due_date": "2026-11-28"
    }
  ],
  "payroll": [
    { "schedule": "monthly", "day": 7, "amount": 90000 }
  ],
  "statutory": [
    { "type": "gst", "amount": 25000 }
  ],
  "opening_balance": 600000,
  "minimum_cash": 200000,
  "as_of": "2026-11-17",
  "weeks": 13,
  "currency": "INR",
  "explain_week": 9,
  "scenarios": [
    { "type": "customer_delay", "customer": "CUST-14", "weeks": 3 }
  ]
}
```

Every key in this sample is a key on `cash_forecast`. One paid invoice is below the tool's own line of 20. The history is also shorter than 90 days. With no snapshot, this forecast is labelled terms-based (due dates only). The sample shows the shape of the call. It is not a finished 13-week cash forecast.

### Sample response (shortened)

A signed-in call was not available while this page was written. A call to `https://cash-forecast-mcp-microsoft.saifs.ai/mcp` without a token returned HTTP 401 on 17 November 2026. The body was `{"error":"unauthorized","error_description":"Missing or invalid authorization header"}`. No forecast is shown here, because a real result was not returned.

<!-- TODO Saifs AI: replace this note with a shortened real response from cash_forecast. Include one week with P10, P50, and P90, the terms-based label or the refusal, and the field names exactly as returned. Do not invent field names. Do not paste a response from another product. -->

The tool states what a result contains. The local engine prints decision cards and the weekly table, and writes the full set to `cash_forecast_result.json`, plus `payment_behaviour_snapshot.json`.

### Response sections

| What comes back | Use it for |
| --- | --- |
| Weekly cash in, cash out, and closing balance | The 13-week cash forecast, each week as P10, P50, and P90 |
| Breach risk | The chance of falling below `minimum_cash`, and the earliest risky week. Present when `minimum_cash` was sent. |
| Week drivers | Customers and bills behind `explain_week`, including invoices that slip versus a due date |
| What-if results | The balance after each scenario, up to 10 |
| Days-to-pay | Per customer, and per segment when history is thin. Returned from `ar_history` alone as well. |
| Accuracy | Last week's cash forecast against `actuals`, when both were sent |
| `payment_behaviour_snapshot` | Quantiles to store and send on the next call. No invoices and no amounts. |
| Decision cards | The plain-language actions to read first. They are estimates, as a range. |
| `cash_forecast_result.json` | The full local-engine result, written in the sandbox |
| `payment_behaviour_snapshot.json` | The local-engine snapshot, written in the sandbox |

<!-- TODO Saifs AI: replace the first column with the JSON keys from a live response. -->

## Known issues and limitations

- **Decision support only.** It does not collect cash, pay a bill, post a journal, or update any system.
- **Not financial advice.** Figures are estimates, shown as a range. There is no interest, borrowing, overdraft, investment, or rate calculation. A loan repayment is only the fixed amount you send.
- **The tools run on request.** They do not watch cash overnight, and they do not send an alert. Use a scheduled flow, and pass the snapshot back, for a repeated forecast.
- **Hosted tool size.** The caps are in At a glance. Above them, or above 20,000 paths, use the local engine. The local engine allows up to 50,000 paths.
- **Thin payment history.** Under 90 days or under 20 paid invoices, with no snapshot, the forecast is labelled terms-based (due dates only).
- **Open invoices in history.** If still-open invoices are left out of `ar_history`, those censored cases are missing from days-to-pay.
- **Payables pattern.** The company's own days-to-pay is used only after 10 paid bills, and only while `payables_policy` is `history_or_due`.
- **The snapshot is yours.** If you omit `payment_behaviour_snapshot`, the next run does not continue last week's pattern. The server is not described as storing it.
- **Currency.** `currency` defaults to INR and labels the result. There is no conversion between currencies.
- **Listed rows.** `max_listed` defaults to 200. Raise it, up to 100,000, when the hosted result must list more drivers. The local engine writes the full result to `cash_forecast_result.json`.
- **No bank feed.** Bank balances and bank transactions are not pulled by this connector. Working capital here is the forecast you send, not a live bank balance. Send `opening_balance` and, for accuracy, `actuals`.
- **Personal data.** `customer_ref` can be pseudonymous. Send a code when a name is not required.
- **Regions.** Not available in US Government (GCC, GCC High, DoD) or China (21Vianet) clouds.

<!-- TODO Saifs AI: rate limits, timeouts, and the service region are not in the tool schema. Add them here when confirmed. -->

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| The tool cannot run, or the call returns unauthorized | The connection is missing or expired. A call with no token returned HTTP 401 on 17 November 2026. | Open the connection and sign in again |
| The forecast is labelled terms-based | History is under 90 days or under 20 paid invoices, and no snapshot was sent | Send a longer `ar_history`, including open invoices, or send `payment_behaviour_snapshot` |
| No breach risk on the weeks | `minimum_cash` was not sent | Send the cushion you want tested |
| No accuracy block | `previous_forecast` or `actuals` was missing | Send both. Actuals need a date or a week, and cash in, cash out, or closing. |
| Week 9 still looks like a due-date sum | The agent answered without calling the tool | Add the instruction in Get started |
| The hosted call refuses a large file | The file is over a cap in At a glance, or `n_paths` is over 20,000 | Call `cash_forecast_get_engine` and run the script unchanged in the sandbox |
| The local engine does not run | Code execution is off, or the script was edited | Enable code execution, save the script exactly as returned, and install `numpy` in that sandbox |
| Dates land in the wrong week | An ambiguous numeric date was read in the other order | Set `date_order` to `DMY` or `MDY` |
| Bills all sit on the due date | Fewer than 10 paid bills, or `payables_policy` is `due_date` | Send `ap_history`, or leave the policy at `history_or_due` once 10 bills are paid |

## FAQ

**Do I need a treasury analyst to use this?**
No. A user asks in plain language, and the agent returns the ranges. Treasury forecasting runs inside the tool, with receipts and disbursements, days-to-pay, and a Monte Carlo balance. A person still decides what to pay or collect.

**What is the least data I need?**
Nothing is required to call the tool. Open invoices plus an opening balance are what make a closing balance useful. Payment history is what moves the timing off the due date. Under 20 paid invoices, the forecast is labelled terms-based unless you send a snapshot.

**Does my data leave my environment?**
Not with the local engine. The tool states that the data never leaves the user's machine on that route. With the hosted tool, the rows in the call are sent to Saifs AI for the calculation. You can turn the hosted tool off.

**Will it change anything in my systems?**
No. It does not collect cash, pay a bill, or write to an ERP. A person acts on the range.

**I already have an ERP MCP server. Why add this?**
That server gives the agent your ERP's records and functions. This connector adds the cash flow forecast. The receivables forecast is timed by how customers pay. It works on rows from any export.

**Why do I get the same ranges every time?**
The calculation is fixed formulas, and the Monte Carlo step uses a seed. The default seed is 7. The same inputs and the same seed return the same ranges. The agent's sentences around them may change.

**What happens if my data is incomplete?**
The tool still runs. It reports what was missing. Thin history is labelled terms-based (due dates only). Breach risk is omitted when `minimum_cash` is omitted. Accuracy is omitted when last week's forecast or the actuals are omitted.

**Can I check how a week was produced?**
Yes. Set `explain_week` to that week. You get the receipts by customer, the payments, and the invoices that slip versus a due-date view. The basis is the days-to-pay pattern. It is not a promise that a customer will pay on that day.

**How large a file can it handle?**
The hosted tool stops at the caps in At a glance. Larger files use the local engine. `max_listed` (default 200) caps how many drivers the hosted result lists.

**Can it run on a schedule?**
Yes. The tools run on request. Call the connector from a Power Automate flow with a recurrence trigger, and store the payment behaviour snapshot between runs. The connector does not send the alert itself.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment). Microsoft Premium connector licensing applies separately.

**Is the closing balance a promise?**
No. It is a liquidity forecast, as P10, P50, and P90. It is not financial advice. It is not a borrowing or investment recommendation.

## Support

**13-Week Cash Flow Forecast - Saifs AI** is published by Saifs AI.

- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Privacy: [https://saifs.ai/privacy_policy/13-week-cash-flow-forecast](https://saifs.ai/privacy_policy/13-week-cash-flow-forecast)
- Terms: [https://saifs.ai/eula](https://saifs.ai/eula)
- MCP endpoint: `https://cash-forecast-mcp-microsoft.saifs.ai/mcp`

<!-- OPEN QUESTIONS
1. Hosted-tool retention, any audit or billing hash, hosting region, rate limits, and timeout. Not stated on the live tool schema.
2. Whether the local engine script makes network calls. The tool states that ledger data does not leave the user's machine on that route. The engine tool is marked open-world.
3. A shortened real response, including JSON field names, from a signed-in call. The endpoint returned HTTP 401 without a token on 17 November 2026.
4. Engine version field name, if one exists. Do not copy metadata.model_version from Inventory Intelligence.
5. The exact week-spread formula, the overdue conditional, and the accuracy metric names. The tool says "accuracy". It does not name MAPE, bias, or a tracking signal.
6. Whether mixed currencies in one file are kept separate, converted, or refused. No conversion is claimed. Default currency on the schema is INR.
7. What a missing opening_balance returns.
8. Token counts. Not measured. Do not copy another product's table.
9. Product page URL. The three candidate URLs returned HTTP 404 on 17 November 2026. Privacy https://saifs.ai/privacy_policy/13-week-cash-flow-forecast returned HTTP 200, as did an unknown privacy slug of the same size. Confirm the page is this product's policy.
10. Header aliases beyond the ones named in the tool schema. Only the named aliases are listed.
-->
