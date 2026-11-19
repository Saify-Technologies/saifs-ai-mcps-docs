# Business Formulas Calculator - Saifs AI

**Business Formulas Calculator - Saifs AI** helps finance, sales and operations teams get an exact figure for **percentages, profit, GST, invoices and everyday business formulas**. You send the values for one calculation, or a batch of them. It returns each result with the formula, the inputs used, the rounding rule, the rule version and the effective date.

It is built for quotes, invoices, tax workings and management reports, where the same inputs have to give the same number on every run.

**Publisher:** Saifs AI
**Website:** [https://saifs.ai](https://saifs.ai)
**Pricing:** [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
**Contact and help:** [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
**Privacy:** [https://saifs.ai/privacy_policy/business-formulas-calculator](https://saifs.ai/privacy_policy/business-formulas-calculator)
**Terms:** [https://saifs.ai/eula](https://saifs.ai/eula)
**Sign-in:** Microsoft single sign-on (OAuth 2.0). No API keys.
<!-- TODO Saifs AI: a product page at https://saifs.ai/business-formulas-calculator returned 404 on 17 November 2026. Add a Learn more line here, and in Support, when that page is published. -->

| If you are | Start with |
| --- | --- |
| A business owner, finance, sales or operations lead | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [Where the data comes from](#where-the-data-comes-from) |
| An administrator or security reviewer | [Data handling and security](#data-handling-and-security), [Security and compliance](#security-and-compliance), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [Where the data comes from](#where-the-data-comes-from), [For developers](#for-developers) |
| A finance, tax or payroll reviewer checking the method | [Inside the calculation](#inside-the-calculation), [Responsible use](#responsible-use) |

## The problem it solves

Staff and agents work these figures every day. A GST split, a margin, a discount chain, a break-even point, an invoice total, days sales outstanding. The working often sits in a spreadsheet, or in a message where someone typed the sum. A small slip is enough to put the wrong number on a quote, an invoice or a board pack.

The slips that show up most often are these. GST added on top of a price that already includes GST. Margin used where the working needed markup. A percent change mixed up with percentage points. A rounding rule that nobody wrote down. Lakh and crore read as million and billion. The next run can also come out differently, and there is no formula, rounding rule or rule date to point to.

Business Formulas Calculator gives the agent a fixed working for those questions:

- **What is 18% of a price:** percent of a number, percent change, percentage points, and the original number after a percent was applied.
- **How to calculate profit:** margin and markup, a price from a target margin, discounts, and profit or loss on cost and on sale.
- **How to split GST:** add GST, split a GST-inclusive amount, CGST and SGST or IGST, TDS and TCS, and the invoice total with amount in words.
- **What the operation needs:** break-even units and revenue, economic order quantity (EOQ), reorder point, and overall equipment effectiveness (OEE).
- **What the report should say:** growth, ratios, budget variance, days sales outstanding, and the other management figures in the tool list.

## Why choose it: the same answer every time

Business Formulas Calculator is deterministic. The same inputs and the same settings produce the same numbers. The arithmetic is done by a fixed formula in a Python script, not written afresh by a language model each time you ask.

That matters because finance, sales and operations act on the figure. They need a result they can rerun, compare and show to someone else. The agent's wording around the numbers may vary. The numbers do not.

**In plain language:** you give it the values for a business sum. It gives you the figure and the formula that produced it.

**For technical readers:** the script uses decimal arithmetic from the Python standard library, dated GST and TDS tables, cost-volume-profit (CVP) for break-even, economic order quantity, and overall equipment effectiveness.

| What it does | Established method |
| --- | --- |
| Percent of, percent change, percentage points, and reverse percent | Percentage arithmetic, using decimal arithmetic |
| Margin, markup, and a price from a target margin | Margin and markup identities |
| Single, chain and trade discounts, and MRP worked back from a selling price | Successive discount |
| Break-even units and revenue, contribution margin, target profit and margin of safety | Cost-volume-profit (CVP) |
| How much to order, when to reorder, and a simple safety stock | Economic order quantity (EOQ), reorder point, and safety stock |
| Availability, performance and quality as one figure | Overall equipment effectiveness (OEE) |
| GST split, place of supply, and TDS or TCS on a date | Dated GST and TDS tables, or a rate you supply |
| Invoice lines, discount, tax, round-off and amount in words | Commercial invoice arithmetic |
| Ratios, cash conversion cycle, growth and CAGR on values | Ratio analysis and growth formulas |
| A moving average, a straight-line trend and a correlation | Descriptive trend formulas |

<!-- TODO Saifs AI: confirm from the returned script that money uses decimal arithmetic, not binary floating point. The research design says decimal. The tool text says standard-library Python and does not name the decimal module. -->

**It does not guess.** Interest mathematics is refused. That covers EMI, simple or compound interest, NPV, IRR, present or future value, discounting and loan schedules. A currency rate is never fetched. Dated tax rules use the date you send, or the table, and the answer shows the rule version and the effective date.

**It calculates and does not act.** It does not post an invoice, file a GST or TDS return, pay a salary, place a purchase order, or write to an accounting system, an ERP or Dataverse.

## Who should use this

| Role | How it helps |
| --- | --- |
| **Finance and accounts teams** | Invoice totals, GST splits, TDS and TCS, depreciation, ratios, budget variance, and days sales outstanding, each with the formula |
| **Sales and pricing staff** | A price from a target margin, a discount check, commission slabs, and a quote total before the deal is approved |
| **Marketers and growth leads** | CTR, CPC, CPM, ROAS, CAC, LTV without discounting, and payback, from the counts they already have |
| **Buyers and procurement** | Landed cost, chain discounts, economic order quantity, and days payable outstanding |
| **Warehouse and inventory teams** | Reorder point, safety stock from a variation you supply, stock valuation, turnover and days of inventory |
| **Production and logistics leads** | OEE, takt and cycle time, yield, chargeable weight, and CBM |
| **HR and payroll teams** | Overtime, pro-rata salary, leave accrual and a gratuity estimate under the stated formula. Not a legal opinion |
| **Owners, CA firms and agent makers** | One set of formulas for board packs, bookkeeping workings and agents, instead of a fresh spreadsheet each time |

**Best fit:** distributors, wholesalers, retailers, e-commerce sellers, manufacturers, import and export firms, logistics teams, professional services, agencies, SaaS and subscription businesses, CA firms, bookkeeping practices, BPO and IT services, and real estate and construction firms that price work and apply GST.

**Not required:** an ERP, a tax engine, or a data science team. The values for one calculation are enough to start.

## Use cases

Each use case below can be built as an agent. The agent types are listed in [Agents you can build](#agents-you-can-build).

**1. Price a quote from a target margin**
Scenario: sales has a cost and a margin the business will accept.
Outcome: the selling price, the markup that margin implies, and the formula.
Ask: "What price gives a 25 percent margin on a cost of 750?"

**2. Split a GST-inclusive amount**
Scenario: a supplier total already includes GST, and the return needs base and tax.
Outcome: base, GST, and CGST plus SGST or IGST when place of supply is known, with the rule date.
Ask: "Split 1180 inclusive of GST at 18 percent into base and tax."

**3. Build an invoice total**
Scenario: an order has lines, a discount and GST, and the document needs a round-off and amount in words.
Outcome: line totals, tax, grand total and the amount in words.
Ask: "Invoice two lines at 500 and one at 1250, less 10 percent, plus 18 percent GST."

**4. Find the break-even point**
Scenario: a product has a fixed cost, a price and a variable cost.
Outcome: break-even units and break-even revenue, plus contribution margin.
Ask: "Break-even units for fixed cost 50000, price 250 and variable cost 150."

**5. Deduct TDS before paying a vendor**
Scenario: accounts is about to pay professional fees and needs the section rate on that date.
Outcome: TDS, the net payable, the section and the effective date. Not a filing.
Ask: "TDS on 50000 of fees at 10 percent, and the net payable."

**6. Size a purchase with EOQ**
Scenario: purchasing has annual demand, an order cost and a holding cost for one item.
Outcome: economic order quantity, and a reorder point when lead time and variation are supplied.
Ask: "EOQ for annual demand 10000, order cost 50 and holding cost 2 per unit."

**7. Explain a budget variance**
Scenario: the month came in above or below the budget, and the pack needs the amount and the percent.
Outcome: the variance, whether it is favourable or adverse, and the formula.
Ask: "Actual 11.5 lakh against a budget of 10 lakh. What is the variance?"

**8. Check a campaign and a payback**
Scenario: marketing has spend, impressions, clicks and, if they have it, customers and revenue.
Outcome: CTR, CPC, CPM, ROAS, and CAC payback when the inputs are there. LTV here is not discounted.
Ask: "Spend 10000, impressions 50000, clicks 1000. What are CTR, CPC and CPM?"

**9. Estimate gratuity or a joiner's pay**
Scenario: HR needs a pro-rata salary for a joiner, or a gratuity figure under the stated formula.
Outcome: the figure, the formula and the rule date. A person still checks it before anyone is paid.
Ask: "Pro-rata salary for a joiner, and the formula you used."

## Agents you can build

Each agent is one agent in your agent host with this connector added as a tool. The last column shows what else to add.

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Quote agent | Turns a cost and a target margin into a price, then checks the discount and the GST | The cost and the rate, typed or read from the quote | 1, 2 |
| Invoice agent | Builds the grand total, the round-off and the amount in words | Order lines from the user or from an order export | 3 |
| GST and TDS agent | Splits GST and works TDS or TCS for a date you name | The amount, the rate or section, and `as_of` | 2, 5 |
| Purchase and stock agent | Works landed cost, EOQ, reorder point and days of cover for the figures you supply | Demand, cost and lead time for that item | 6 |
| Board-pack agent | Works growth, margins, variance and ratios, and cites the formula | The month's figures, pasted or read from a sheet | 4, 7 |
| Campaign agent | Works ROAS, CAC and payback from a campaign export | Spend, impressions, clicks and customer counts | 8 |
| Payroll estimate agent | Works overtime, pro-rata pay, leave accrual and a gratuity estimate | Dates and salary figures. A person approves before payment | 9 |
| Accounts-system agent | Reads a total from the accounting system, then calculates | That system's own MCP server, for the read only | 2, 3, 5 |
| Batch agent | Runs up to 500 calculations from a JSON file in the sandbox | Code execution on the agent host, and `calls_path` | 3, 7 |
| Scheduled report agent | Runs the same workings each morning without anyone asking | A flow with a recurrence trigger, plus email or Teams | 7, 8 |

The tools run on request. Scheduling comes from the flow, not from the connector.

For a buy list across many products, use Inventory Intelligence. This connector calculates one economic order quantity, reorder point or safety stock from the figures you supply. It does not plan the warehouse.

## At a glance

| Item | Detail |
| --- | --- |
| Tools | Eight. Seven return a Python script. `business_calculator_list_formulas` returns the catalogue |
| Calculators inside the scripts | 32, grouped by area. The names are in [How it works](#how-it-works) |
| Minimum input | A tool name. The operation and its inputs come from the catalogue |
| Batch size | Up to 500 calls in one script (`calls` maxItems 500) |
| Where the arithmetic runs | Your agent's code sandbox. Not on the Saifs AI server |
| Script requirements | Python standard library. No extra package is named |
| Writes to your systems | None |
| Rule version | Returned on each answer as the rule version and the effective date |

## Where the data comes from

Business Formulas Calculator does not connect to your accounting, payroll or store systems. The agent brings the numbers to it.

| Data source | How the agent gets the numbers | Route that suits |
| --- | --- | --- |
| A spreadsheet, CSV or PDF the user already has | The user pastes the values, or the agent reads the file in its sandbox and writes a calls file | `calls_path` for a batch. `calls` for one or two sums |
| Accounting, billing, inventory or payroll software (for example Tally, QuickBooks, Zoho Books, Shopify) | Export the report, then pass the figures. Or use that product's own MCP server if it has one | Same as above |
| An ERP with its own MCP server (for example Dynamics 365, Business Central, SAP, NetSuite) | The agent reads the record through the ERP's tools, then passes the figures here | `calls` for a few records |
| Dataverse, an online Excel workbook, a SharePoint list or a SQL database | A connector on the same agent or flow reads the row | Same as above |
| A scheduled flow | A flow reads the source on a schedule and calls this connector | `calls`, or a file if the host has a sandbox |
| Another Saifs AI tool | Pass the figures that tool returned. This connector does not import its files | `calls` |

The connector needs the numbers only. It has not been certified against Tally, QuickBooks, Zoho, Shopify, or any ERP. Those names are examples of where the numbers live.

**Works alongside your system's MCP server.** That server gives the agent the system's records. This connector adds the formula. The system remains the place that posts the invoice, the return or the payment.

**What to pass for each job**

| What you have | Tool that owns it | Why it matters |
| --- | --- | --- |
| A price, a cost, a margin, a discount or a ratio | `pricing_profit_calculators` | Price, profit, break-even and commission |
| A GST-inclusive total, a TDS section, invoice lines, or a currency amount and a rate | `gst_tds_invoice_calculators` | Tax split, deduction, invoice total, conversion |
| Demand, stock lots, freight, a carton size, or OEE inputs | `inventory_logistics_calculators` | EOQ, valuation, landed cost, chargeable weight, OEE |
| A balance sheet, ageing, a budget, a depreciation base, or two periods of sales | `finance_mis_calculators` | Ratios, cash cycle, variance, growth |
| Ad counts, a funnel, or subscription totals | `marketing_sales_calculators` | ROAS, velocity, CAC, LTV, churn |
| Salary, dates, leavers, or headcount | `hr_payroll_calculators` | Pay estimates and workforce rates |
| A number to format, a unit, a date, a list of values, or a survey | `numbers_dates_stats_calculators` | Format, conversion, dates, averages, sample size |

There is no column mapper. The agent places each number in the input field the catalogue names. Call `business_calculator_list_formulas` to see those fields.

**File format for a batch.** `calls_path` is a JSON file of calls, already in the sandbox. Other formats are not described by the tool.

## How it works

Every calculation tool works the same way. The server returns a script. The sandbox runs it. `business_calculator_list_formulas` is the exception: it returns the catalogue and does not calculate.

| Tool | Calculators in the script |
| --- | --- |
| `pricing_profit_calculators` | `percentage_calculator`, `margin_markup_calculator`, `discount_calculator`, `profit_loss_calculator`, `tier_pricing_commission_calculator`, `break_even_calculator`, `ratio_proportion_calculator` |
| `gst_tds_invoice_calculators` | `gst_calculator`, `tds_tcs_calculator`, `invoice_calculator`, `currency_converter` |
| `inventory_logistics_calculators` | `eoq_reorder_calculator`, `inventory_metrics_calculator`, `landed_cost_calculator`, `shipping_weight_calculator`, `production_efficiency_calculator` |
| `finance_mis_calculators` | `ratio_analysis_calculator`, `working_capital_days_calculator`, `budget_variance_calculator`, `depreciation_calculator`, `growth_share_calculator` |
| `marketing_sales_calculators` | `ad_metrics_calculator`, `sales_funnel_calculator`, `unit_economics_calculator` |
| `hr_payroll_calculators` | `payroll_calculator`, `workforce_metrics_calculator` |
| `numbers_dates_stats_calculators` | `number_format_calculator`, `unit_converter`, `date_due_date_calculator`, `average_statistics_calculator`, `survey_sample_calculator`, `trend_basic_calculator` |
| `business_calculator_list_formulas` | No script. Catalogue, search and a worked explanation |

You can narrow a script with `tools`. If a `calls` entry names a calculator from another area, that calculator is added to the script.

**Run a batch in the sandbox**

1. The user points the agent at the values, or at a JSON file of calls.
2. The agent calls the area tool with `calls_path`, or with `calls` when the values are small enough to send.
3. The connector returns the script.
4. The agent runs the script in its sandbox.
5. The script prints each answer. The agent reports the answers to the user.

### Inside the connector

1. The user asks for a figure, in plain language.
2. The agent chooses a tool and calls the connector, which passes the call to the Saifs AI MCP server over HTTPS at `https://business-calculator-mcp-microsoft.saifs.ai/mcp`.
3. The server checks the signed-in account. A call without a valid connection is rejected.
4. An area tool returns a standard-library Python script for that area. The list tool returns the catalogue. The server does not run the arithmetic, and it does not call another API to do it.
5. The agent runs the script and explains the result. The numbers come from the script. The wording comes from the agent.

### Inside the calculation

The script applies the formula for the operation you named. Each answer includes the formula it used, so the working is on the result rather than only on this page.

| Step | What happens | Method |
| --- | --- | --- |
| 1. Choose the area | The agent calls one of the seven area tools, or the catalogue tool | Tool names above |
| 2. Build the script | The server includes the calculators for that area, plus any other calculator named in `calls` | Standard-library Python |
| 3. Attach the sums | Values in `calls` are written into the script. A `calls_path` file is read in the sandbox | Up to 500 calls |
| 4. Apply the formula | The named operation runs on the inputs. Percents are percents: 18 means 18 percent | The formula named on the answer |
| 5. Round | The default is half-up. You can set bankers, down, up, nearest rupee, nearest 50 paise, or a mode with places and step | The rounding named on the answer |
| 6. Date the rule | GST, TDS and payroll rules use `as_of` and, when you send it, `rate_table_version` | Dated table or the rate you supplied |
| 7. Return the working | Each answer has the result, the formula, the inputs used, the rounding, the rule version and the effective date | Fixed fields on the answer |

**Why the answer does not change.** The script is a fixed formula. The tools are marked idempotent. There is no language model inside the arithmetic. The same inputs and settings return the same numbers. The agent's wording around them may vary.

**What happens when something is missing or out of scope.** Interest mathematics is refused. Live foreign-exchange rates are not fetched. You supply the rate. The GST tool can use a dated slab, or a rate you send. The catalogue lists the inputs each operation expects.

<!-- TODO Saifs AI: document the exact error text when a required input is missing, and any default the script applies. Those texts were not visible without a signed-in call. Do not describe a default that the script does not use. -->

**Where the working is shown.** On each answer: formula, inputs used, rounding, rule version and effective date. `business_calculator_list_formulas` with a tool and an operation returns a plain-language explanation and a worked example for that operation.

## Data handling and security

| Question | `calls` sent in the request | `calls_path` in the sandbox | `business_calculator_list_formulas` |
| --- | --- | --- | --- |
| Do the numbers reach Saifs AI? | Yes. The inputs are sent and written into the script | No. Only the file path string is sent. The file stays in the sandbox | No calculation inputs. A tool name or a search word may be sent |
| Where does the arithmetic run? | In your sandbox, after the script comes back | In your sandbox | It does not run. The catalogue comes back from the server |
| Where is the script built? | `business-calculator-mcp-microsoft.saifs.ai`, over HTTPS | The same endpoint. It does not receive the file | The same endpoint |
| What happens to the numbers after the call? | <!-- TODO Saifs AI: confirm retention. The tool says the arithmetic never runs on the server. It does not say whether the request is stored. --> | They never left the sandbox | <!-- TODO Saifs AI: same retention confirmation. --> |

**What "sandbox" means.** The sandbox is the code-execution environment of your own agent host. It belongs to your tenant, not to Saifs AI. Your host's data policies still apply to it.

**Keep the numbers in your environment.** Do not put them in `calls`. Write a JSON file of calls in the sandbox and pass `calls_path`. The arithmetic still runs in the sandbox either way. The difference is whether the values travel to Saifs AI to be written into the script.

**Sign-in.** Users sign in with Microsoft single sign-on. Saifs AI holds only the account details needed for sign-in and billing. No API key is created, shared or stored.

**No write access.** The tools are read-only. None of them creates, changes or deletes a record in an ERP, an accounting system or Dataverse. They are marked as not calling the open web. Currency rates are not fetched.

<!-- TODO Saifs AI: review one returned script and confirm it makes no network calls. The tool text says the arithmetic never runs on this server or any API. The script itself was not reviewed for this page. -->

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
| Where the arithmetic runs | In the customer's sandbox. The server returns the script and does not run it |
| Customer numbers stored | <!-- TODO Saifs AI: state the retention fact once it is confirmed. Do not write "none" until the facts sheet says so. --> |
| Passwords held | None. Sign-in is Microsoft single sign-on |
| Write access to your systems | None |
| Open web | The tools are marked as not calling it. Exchange rates are supplied by the caller |
| Script | Standard-library Python, returned for the sandbox to run |

**Independent audits.** Saifs AI does not yet hold an independently audited certificate such as SOC 2 or ISO 27001. If your review needs a completed security questionnaire, request it through the [contact page](https://saifs.ai/contact-us).

## Responsible use

This connector calculates tax, payroll and workforce figures. Those results support a person. They do not replace one.

- **Not tax, legal or financial advice.** GST and TDS answers show the formula, the rule version and the effective date. They are not a return, an assessment or a filing.
- **Dated rules.** GST slabs and TDS or TCS sections come from a dated table, or from a rate you supply. An old `as_of` date uses the rules for that date. Check the effective date on the answer before you rely on it.
- **Payroll and gratuity are estimates under the stated formula.** The payroll tool applies the formula it names, including leave accrual and the gratuity rules it carries. It does not compute salary income tax, provident fund or ESI. It is not a determination of what a person is owed.
- **Workforce figures describe a group.** Attrition, absenteeism, cost per hire and utilisation are rates from the totals you send. The tool does not hire, fire or score a person.
- **No interest mathematics.** EMI, compound interest, NPV, IRR and loan schedules are refused.
- **A person acts.** The connector does not pay anyone, file anything, or post anything.

## Token usage and cost

The script is the large part of the call. With `calls_path`, the rows stay in the sandbox, so they are not sent through the model as tool inputs. With `calls`, the values are part of the request and part of the script, so the size grows with the batch.

<!-- TODO Saifs AI: measure tokens before publishing numbers. The table below is the shape the page needs. Do not fill it with estimates. Tokenizer, sample and date must be recorded, for a small batch, for 500 calls, and for a later run that reuses the script. Include where sending calls uses fewer tokens than the file route. State the reference price per million input and output tokens and the month. Note hosts that bill a fixed amount per action. -->

**Which route to choose, before those measurements are published**

- **One or two sums:** `calls` is the simple route. The values are sent to Saifs AI.
- **A batch, or a rule that the numbers stay in your tenant:** `calls_path`.
- **The same script, many times in one session:** keep the script in the sandbox and change the calls file.

**Hosts that bill per action.** Some agent hosts charge a fixed amount for each tool call and do not bill by token. On those hosts, a smaller script does not reduce that charge. Choose `calls_path` so the numbers stay in the sandbox, not to cut a per-action fee.

## Prerequisites

- A Saifs AI account, created by signing in with Microsoft single sign-on
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Access to an agent host that can call this connector and run Python in a sandbox
- The values for the calculation you want. The catalogue names the fields
- For a batch that stays in your tenant: code execution enabled, and a JSON calls file

## How to get credentials

No keys or secrets are needed. When you create a connection, you are redirected to sign in with your Microsoft account and approve access. If the connection later expires, sign in again from the connection page.

## Get started with your connector

1. In the agent host, open your agent and add a tool.
2. Search for **Business Formulas Calculator** and select it.
3. Create a connection and sign in.
4. Add this to the agent's instructions: "For any GST, margin, markup, discount, invoice, break-even, percentage or other business formula, call Business Formulas Calculator. Do not calculate the figure yourself."
5. Ask for the catalogue first, so the agent uses the real input names. For example: "List the GST operations and their inputs."
6. Ask for the figure in plain language. For example: "What price gives a 25 percent margin on a cost of 750?"

Treat tax, payroll and invoice results as workings to check. A person reviews them before a document is sent or a payment is made.

## For administrators

Enabling this connector gives the organisation one approved way to answer percentages, profit, GST, invoices and everyday business formulas, inside the agents your makers already build.

**Why this is an administrator decision.** Without a calculation tool, makers ask an agent to work the figure out in prose. A language model can give a different number on each run, and the result has no formula, rounding rule or rule date. A governed connector replaces that with one method for every agent and team.

- **You choose who can connect it.** Tenant and environment permissions apply. Only users who can create a connection can call the tools.
- **You keep your system of record.** The connector calculates and explains. It does not post invoices, file returns or pay staff.
- **You can check the number.** The same inputs return the same numbers. Each answer shows the formula, the inputs, the rounding, the rule version and the effective date.
- **You control whether the values are sent.** Tell makers to use `calls_path` when the numbers must stay in the tenant. Values inside `calls` are sent so they can be written into the script.
- **You set data policy as usual.** Place the connector in the Business, Non-business or Blocked group with data loss prevention policies.
- **There are no secrets to rotate.** Expired connections are renewed by signing in again.

**Review checklist**

- Decide whether makers may send values in `calls`, or must use a sandbox file
- Assign the connector to the correct data policy group
- Review the privacy policy and terms
- Read any self-assessment listed under Security and compliance, once it is published
- Review a returned script if your policy requires it
- For payroll and tax agents, confirm a person still approves the result

## For developers

Call `business_calculator_list_formulas` before you invent an input name. The area tools share one request shape. The fields inside `inputs` are defined per operation, and the catalogue is where those names are published.

### Shared request fields

| Field | Meaning | Needed for |
| --- | --- | --- |
| `tools` | Optional list of calculators to keep in the script. Default is every calculator in that area | A smaller script |
| `calls` | Array of calculations, maximum 500. Each object needs `tool` | Running those sums, and including their calculators |
| `calls_path` | Path of a JSON calls file already in the sandbox | Keeping the numbers out of the request |
| `full_catalog` | Boolean, default false. When true, the script also carries descriptions and worked examples | A larger script with the notes inside it |
| `id` | Optional label on a call, echoed on the answer | Matching a row in a batch |
| `tool` | Calculator name. Short forms such as `gst` are accepted | Choosing the formula family |
| `operation` | Operation name from the catalogue. The schema example is `split_inclusive` | Choosing the formula |
| `inputs` | Object of input fields. Percents are percents: 18 means 18 percent | The values the formula needs |
| `rounding` | `half_up` (default), `bankers`, `down`, `up`, `nearest_rupee`, `nearest_50_paise`, or an object with mode, places and step | The rounding rule on the answer |
| `currency` | Currency label | Display on that answer |
| `locale` | Locale label | Format of that answer |
| `as_of` | Date `YYYY-MM-DD` | Which dated GST, TDS or payroll rule applies |
| `rate_table_version` | Optional table version | Pinning a rule table |
| `date_order` | `DMY` or `MDY` | Reading dates in the inputs |

### Catalogue fields

| Field | Meaning | Needed for |
| --- | --- | --- |
| `tool` | One calculator, for example `gst_calculator` | The operations and inputs for that calculator |
| `operation` | Used with `tool` | A plain-language explanation and a worked example |
| `query` | Search words, for example markup, ageing or gratuity | Finding which calculator owns a question |

### Options and defaults

| Option | Default | Purpose |
| --- | --- | --- |
| `rounding` | `half_up` | Used when a call does not set one |
| `full_catalog` | false | Leaves descriptions and worked examples out of the script |
| `calls` | None | No sums are baked in until you send them |
| `tools` | All calculators in the area | Narrow this when you want a shorter script |
| Percent values | The number is the percent | 18 means 18 percent, not 0.18 |
| Exchange rate | None. You supply it | The tool does not fetch a live rate |
| Interest mathematics | Refused | EMI, NPV, IRR, discounting and loan schedules |

<!-- TODO Saifs AI: publish default currency, default locale, and the date used when as_of is omitted. They are not in the tool schema. -->

### Sample request

This request uses only fields from the published schema. It asks for the GST catalogue. It does not guess the input names inside a calculation.

```json
{
  "tool": "gst_calculator",
  "query": "split"
}
```

A calculation request has this shape. Fill `inputs` with the names the catalogue returns. Do not copy field names from memory.

```json
{
  "tools": ["gst_calculator"],
  "calls": [
    {
      "id": "split-1",
      "tool": "gst_calculator",
      "operation": "split_inclusive",
      "inputs": {},
      "rounding": "half_up",
      "as_of": "2026-11-17"
    }
  ]
}
```

`split_inclusive` is the operation name given as an example in the tool schema. The keys inside `inputs` are not listed in the schema.

<!-- TODO Saifs AI: replace the empty inputs object with a real call, and paste a shortened real response. On 17 November 2026 the endpoint returned 401 without a signed-in connection, so no response body was captured. Do not publish invented result fields. -->

### What each answer contains

The tool text states these fields. A captured response is still required before this page shows a sample body.

| Field | Use it for |
| --- | --- |
| Result | The figure |
| Formula | The working, so the figure can be checked |
| Inputs used | The values the formula actually took |
| Rounding | The rule applied |
| Rule version | Which table or formula version ran |
| Effective date | The date those rules apply to |
| Explanation | One plain sentence on the catalogue tool, when you ask it to explain an operation |

## Known issues and limitations

- **Calculation only.** It does not post, file, pay, or update any system.
- **Not tax, legal or financial advice.** Check the rule version and the effective date. A person reviews the result before it is sent or paid.
- **Interest mathematics is refused.** EMI, simple or compound interest, NPV, IRR, present or future value, discounting and loan schedules.
- **GST and TDS follow a dated table or your rate.** The GST tool describes slabs of 5, 18 and 40 percent on a date. TDS and TCS use the section and the date. This page is not a statement of the statute.
- **Payroll limits.** The payroll tool describes leave accrual of 1 day per 20 worked, and gratuity under the formula it names, including a cap of 20 lakh rupees. It does not compute salary income tax, provident fund or ESI.
- **No live exchange rate.** Currency conversion uses the rate you supply.
- **LTV is not discounted.** The marketing tool refuses discounted LTV. It also does not do attribution modelling.
- **One item, not a warehouse plan.** EOQ, reorder point and safety stock use the figures you send. A multi-product buy list is Inventory Intelligence, not this connector.
- **Trend is descriptive.** Moving average, straight-line trend and correlation are included. A seasonal forecast is not.
- **Batch size.** One script accepts up to 500 calls.
- **No column mapping.** Headers in a spreadsheet are not matched for you. The catalogue names the input fields.
- **Regions.** Not available in US Government (GCC, GCC High, DoD) or China (21Vianet) clouds.

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| The tool cannot run, or the endpoint rejects the call | The connection is missing or expired. An unsigned call to the endpoint was rejected | Open the connection and sign in again |
| The tool refuses an interest or loan calculation | EMI, NPV, IRR, discounting and loan schedules are out of scope | Use a different working. This connector will not run those formulas |
| GST is asked on the pricing tool | GST sits on `gst_tds_invoice_calculators` | Call that tool, or name `gst_calculator` in `calls` so it is added |
| The agent returns a number and no formula | The agent did not call the tool | Add the instruction in Get started |
| A batch fails after a large paste | `calls` allows 500 items | Split the batch, or use `calls_path` |
| The script does not run | Code execution is not enabled on the agent host | Enable code execution. The script needs the Python standard library |

<!-- TODO Saifs AI: replace any row above if a signed-in call shows a different message. No error code was captured. -->

## FAQ

**Do I need a specialist to use this?**
No. A user asks in plain language for percentages, profit, GST, invoices and everyday business formulas. The formulas run in the script. A person still reviews tax and payroll figures before they are used.

**What is the least I need to send?**
A tool name is enough to start the catalogue. A calculation also needs the operation and the inputs that operation lists.

**Does my data leave my environment?**
Not if you use `calls_path`. The file stays in the sandbox. If you send `calls`, those inputs are sent to Saifs AI so they can be written into the script. The arithmetic runs in the sandbox on both routes.

**Will it change anything in my systems?**
No. It does not post invoices, file returns, pay staff, or write to any system.

**I already have an accounting MCP server. Why add this?**
That server gives the agent your records. This connector gives the agent the formula, including GST, margin, invoice totals and break-even, and it works on numbers from any source.

**Why do I get the same answer every time?**
The script applies a fixed formula. The same inputs and settings return the same numbers. The agent's wording around the numbers may vary.

**What happens if my data is incomplete?**
Interest mathematics is refused rather than estimated. For other gaps, use the catalogue to see the inputs the operation expects. Do not assume a missing rate will be invented.

**Can I check how a result was produced?**
Yes. Each answer includes the formula, the inputs used, the rounding, the rule version and the effective date.

**How large a batch can it handle?**
Up to 500 calls in one script.

**Can it run on a schedule?**
Yes, by calling it from a scheduled flow. The connector itself does not run on a timer.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment).

## Support

**Business Formulas Calculator - Saifs AI** is published by Saifs AI.

- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Privacy: [https://saifs.ai/privacy_policy/business-formulas-calculator](https://saifs.ai/privacy_policy/business-formulas-calculator)
- Terms: [https://saifs.ai/eula](https://saifs.ai/eula)
