# MSME 43B(h) Payment Checker - Saifs AI

**MSME 43B(h) Payment Checker - Saifs AI** helps accounts payable and finance teams decide **which MSME bills to pay by when, and what section 43B(h) will disallow**. You send purchase bills, payments, and the vendor MSME details you hold. It returns coverage, a statutory due date, a pay-by list, and the year-end disallowance.

It is built for Indian companies that buy from micro and small suppliers, and for CA firms that review those ledgers at tax audit.

**In plain language:** you give it the purchase register and the Udyam details from the certificates you hold. It tells you which MSME bills to pay by when, and what section 43B(h) will disallow.

**For technical readers:** the dates follow MSMED Act section 15, deemed acceptance, and the section 43B(h) year-end test. The tool also labels that disallowance as section 37(2)(g). The rule tables are dated and versioned.

It flags and dates. It does not pay a supplier. It does not compute interest under section 16 of the MSMED Act. It is not tax or legal advice. A chartered accountant reviews the figures.

**Publisher:** Saifs AI
**Website:** [https://saifs.ai](https://saifs.ai)
**Pricing:** [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
**Contact and help:** [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
**Privacy:** [https://saifs.ai/privacy_policy/msme-43bh-payment-checker](https://saifs.ai/privacy_policy/msme-43bh-payment-checker)
**Terms:** [https://saifs.ai/eula](https://saifs.ai/eula)
**Sign-in:** Microsoft single sign-on (OAuth 2.0). No API keys.
**MCP endpoint:** [https://msme-43bh-mcp-microsoft.saifs.ai/mcp](https://msme-43bh-mcp-microsoft.saifs.ai/mcp)

<!-- TODO Saifs AI: publish the product page, then add a Learn more line here and in Support. https://saifs.ai/msme-43bh-checker, https://saifs.ai/msme-43bh-payment-checker, and https://saifs.ai/msme-43bh-checker-tutorial each returned HTTP 404 on 25 November 2026. Do not link them until one opens. -->

| If you are | Start with |
| --- | --- |
| A business owner, CFO, or accounts payable lead | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [Where the data comes from](#where-the-data-comes-from) |
| An administrator or security reviewer | [Data handling and security](#data-handling-and-security), [Security and compliance](#security-and-compliance), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [Where the data comes from](#where-the-data-comes-from), [For developers](#for-developers) |
| A finance or tax reviewer checking the method | [Inside the calculation](#inside-the-calculation), [Responsible use](#responsible-use) |

## The problem it solves

Under the MSMED Act, a buyer must pay a micro or small supplier on time. With no written agreement, the limit is 15 days from acceptance. With a written agreement, it is the agreed date, and that date cannot pass 45 days.

Since FY 2023-24, section 43B(h) adds a tax cost. A covered bill still unpaid past that limit at the year end is disallowed that year. It becomes deductible in the year it is paid. Paying before the income-tax return due date does not save it.

Teams still count from the invoice date. They treat a medium enterprise or a trader as covered. They miss deemed acceptance. They assume return-date relief that this section does not give. The MSE payables sit in a purchase register, and the due dates are worked out by hand.

MSME 43B(h) Payment Checker makes that check available to any AI agent:

- **Which MSME bills to pay by when:** a pay-by list of covered bills falling due, with days left and amount.
- **What the statutory due date is:** 15 days from acceptance, or the agreed term, capped at 45 days.
- **What section 43B(h) will disallow:** the year-end disallowance by vendor, and the year it becomes deductible.
- **Who is covered:** micro and small suppliers, from the Udyam details you send.
- **What the tax audit file needs:** figures for Form 3CD clause 22 and MSME Form I.

## Why choose it: the same dates every time

The same bills, payments, vendor list, and settings return the same dates and the same disallowance. The hosted tool is marked idempotent. The figures come from dated rule tables. They are not written afresh by a language model each time you ask.

That matters because accounts payable pays real suppliers, and a tax auditor will ask why a bill was allowed. They need a date they can rerun. The agent's wording around the numbers may vary. The numbers do not.

| What it does | Established method |
| --- | --- |
| Sets the payment deadline at 15 days, or at the agreed term capped at 45 days | MSMED Act section 15 |
| Returns the day of acceptance as actual, deemed, or objection removed | Deemed acceptance, from the delivery, goods-receipt, service, objection, and removal dates you send |
| Decides coverage from the Udyam category, activity, and registration date you send | Udyam category tables. The result carries the rule version. |
| Works out the year-end disallowance by vendor | Section 43B(h) year-end test, also labelled section 37(2)(g) |
| Builds the pay-by list, and the Form 3CD clause 22 and MSME Form I figures | The same statutory due dates, summed for the period |
| Matches a payment to the bill you name, or to the oldest open bill | Match on voucher, bill reference, Agst Ref, or invoice number |

**It does not guess.** If you omit the category, the tool does not treat the vendor as covered. If you omit the major activity, it does not assume the vendor is a non-trader. Both switches default to off. A missing Udyam registration date is not treated as "registered before the supply" unless you turn that switch on.

**It recommends and does not act.** It never pays a supplier, never posts a payment, and never writes to Tally, an ERP, or Dataverse. It never computes interest under section 16 of the MSMED Act.

<!-- TODO Saifs AI: confirm from a signed-in call the exact words used when a vendor cannot be classified, and when only an invoice date was sent. The schema defaults say a missing category and a missing activity are not filled in. The schema does not publish the status text. Do not print "cannot classify" or "estimate: acceptance date missing" as product labels until a response shows them. -->

## Who should use this

| Role | How it helps |
| --- | --- |
| **Accounts payable accountants** | A Monday pay-by list of MSME bills, with days left and the amount |
| **Finance controllers** | The section 43B(h) disallowance by vendor, while there is still time to pay |
| **CFOs of Indian companies** | The year-end disallowance by vendor, before the close |
| **Tax auditors** | Acceptance dates, statutory due dates, and Form 3CD clause 22 figures for the file |
| **CA firms** | The same dated rules on each client ledger during tax audit |
| **Shared-service centres** | One method for every entity, with the vendor category visible on the run |
| **Tally and ERP partners** | A check their agents can call before a person proposes a payment |
| **Agent makers and developers** | A tested due-date and disallowance calculation, so the agent does not count the days itself |

**Best fit:** Indian buyers of goods or services from micro and small suppliers, CA firms in audit season, shared-service centres, and Tally or ERP partners. The buyers are accounts payable teams in India.

**Not required:** a tax module or a custom rule engine. A purchase register, a payment list, and the Udyam details you already hold are enough to start.

## Use cases

Each use case below can be built as an agent. The agent types are listed in [Agents you can build](#agents-you-can-build).

**1. Weekly pay-by list**
Scenario: it is Monday, and accounts payable must choose which MSME bills to pay this week.
Outcome: a pay-by list of covered bills falling due inside the horizon, with days left and amount. The horizon defaults to 7 days.
Ask: "Which MSME supplier bills must we pay this week?"

**2. The 45-day cap**
Scenario: a written agreement gives the supplier 60 days.
Outcome: the statutory due date uses the 45-day cap. A term above 45 days does not extend it.
Ask: "Our agreement says 60 days. When is this MSME bill actually due?"

**3. Explain one bill**
Scenario: a supplier or a tax auditor asks why one bill has that due date.
Outcome: the acceptance type, the statutory due date, the status, and the reason for that bill.
Ask: "Explain the due date and status for this supplier invoice."

**4. Year-end disallowance**
Scenario: the financial year is closing, and some covered bills are still open.
Outcome: the section 43B(h) disallowance by vendor, and the year each amount becomes deductible.
Ask: "How much will section 43B(h) disallow for the year ending 31 March 2026?"

**5. Form 3CD and MSME Form I**
Scenario: the tax audit needs clause 22, and the half-year return needs its MSME figures.
Outcome: outstanding and overdue amounts for Form 3CD clause 22 and MSME Form I. MSME Form I is the half-yearly MSME-1 return.
Ask: "Prepare the MSME figures for Form 3CD clause 22 and the MSME-1 return."

**6. Who is covered**
Scenario: the vendor list mixes micro, small, medium, traders, and suppliers with no Udyam record.
Outcome: coverage from the category, the major activity, and the Udyam registration date you send.
Ask: "Which suppliers are covered micro or small enterprises, and which are not?"

**7. Deemed acceptance**
Scenario: the goods were delivered, and the file has a delivery date and no goods-receipt date.
Outcome: the day of acceptance comes back as actual, deemed, or objection removed, from the dates you sent.
Ask: "What is the statutory due date when we have delivery, and no goods-receipt date?"

**8. A payment with no bill reference**
Scenario: the payment export has a vendor and an amount, and no Agst Ref.
Outcome: the amount is applied to that vendor's oldest open bill. Name the bill when you can, so the match is the one you intend.
Ask: "Apply this week's payments and show which MSME bills are still unpaid."

**9. A full purchase register**
Scenario: the Tally or ERP export is larger than the hosted caps.
Outcome: the same engine runs in the agent's sandbox. On that path the rows are not sent to Saifs AI.
Ask: "Run the section 43B(h) check on this full purchase register."

## Agents you can build

Each agent is one agent in your agent host with this connector added as a tool. The last column shows what else to add.

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Weekly pay-by agent | Returns the pay-by list for the horizon you set | A purchase file, or a source from [Where the data comes from](#where-the-data-comes-from) | 1, 2 |
| Bill explanation agent | Explains one bill's acceptance type, statutory due date, and status | `bill_ref` for the voucher or the invoice, plus the ledger inputs | 3, 7 |
| Year-end disallowance agent | Returns the section 43B(h) disallowance by vendor for `fy_end` | Payments as well as bills, and the financial year end | 4 |
| Tax-audit figures agent | Returns Form 3CD clause 22 and MSME Form I amounts | The same ledger, and the period you want reported | 5 |
| Vendor coverage agent | Marks which suppliers are covered micro or small enterprises | The vendor master, including Udyam category, activity, and registration date | 6 |
| Payment-match agent | Applies payments to the named bill, or to the oldest open bill | Payment rows with Agst Ref, bill reference, or invoice number where you have them | 8 |
| Large-ledger agent | Runs the local engine on a full export | Code execution on the agent host | 9 |
| Accounting-system agent | Reads bills, payments, and vendors from the system of record, then hands the list to a person | That system's own MCP server. A person still makes the payment. | 1, 6 |
| Scheduled pay-by agent | Sends the pay-by list on a recurrence, without anyone asking | A Power Automate flow with a recurrence trigger, and email or Teams. The flow must pass the last `rules_snapshot`. | 1 |

The tools run on request. Scheduling comes from the flow, not from the connector. The connector does not pay the supplier.

## At a glance

| Item | Detail |
| --- | --- |
| Tools | `msme_43bh_checker` (hosted calculation) and `msme_43bh_get_engine` (local engine) |
| Jobs on the hosted tool | `classify_vendors`, `due_dates`, `explain_bill`, `form_3cd_msme1_figures`, `pay_by_list`, `year_end_disallowance`. Omit `job` and the tool runs what the inputs allow. |
| Minimum input | Nothing on the schema is marked required. A due-date run needs bills and vendor details. A disallowance run also needs `fy_end` and the payments. |
| Hosted tool size | Up to 20,001 bills. Up to 50,001 payments. Up to 20,001 vendors. Up to 5,001 agreements. |
| Listed rows | `max_listed` defaults to 200. The range is 1 to 100,000. |
| Larger files | Local engine, for ledgers over 20,000 bills, 50,000 payments, or 20,000 vendors, or when the rows must stay in your environment. It needs `numpy`. |
| Writes to your systems | None. The local engine writes `msme_43bh_result.json` and `msme_rules_snapshot.json` in the sandbox. Pass `rules_snapshot` back on the next run. |
| Out of scope | Paying a supplier. Posting a payment. Interest under section 16 of the MSMED Act. Calls to the Udyam portal. |

<!-- TODO Saifs AI: if a live response includes an engine version field, name it in At a glance. The tool says the rule tables are dated and versioned, and that rules_snapshot carries the rule version. Do not copy metadata.model_version from Inventory Intelligence. -->

## Where the data comes from

MSME 43B(h) Payment Checker does not connect to your systems. The agent brings the rows to it. Any source that can supply bills, payments, and a vendor list can feed it.

| Data source | How the agent gets the data | Route that suits |
| --- | --- | --- |
| A spreadsheet or CSV export from accounting software (for example Tally) | The user uploads the file to the agent | Local engine for large files. Hosted tool inside the caps. |
| An ERP that has its own MCP server (for example Dynamics 365 Finance, Business Central, or SAP) | The agent reads bills, payments, and vendors, then passes the rows on | Hosted tool inside the caps. Above them, export a file and use the local engine. |
| Dataverse tables | The Dataverse MCP server or the Dataverse connector | Same as above |
| A SQL database, an Excel workbook online, or a SharePoint list | A connector added to the same agent or flow | Same as above |
| A scheduled flow | A Power Automate flow reads the source on a schedule and calls this connector | Hosted tool, if the ledger is inside the caps. Pass the last rules snapshot from the flow's own store. |
| Udyam certificates the company already holds | A person types or pastes the Udyam number, category, activity, and registration date | Either route. This connector does not call the Udyam portal. |

The connector needs only rows. It has not been certified against each system named above. Any other tool that returns those rows can feed it the same way.

**Works alongside your accounting system's MCP server.** That server gives the agent the system's records and functions. This connector adds the statutory due date, the pay-by list, and the section 43B(h) disallowance. The system supplies the ledger. This connector calculates the dates. A person makes the payment.

**What to pull from the source**

| Data in your system | Field it fills | Why it matters |
| --- | --- | --- |
| Purchase bills: voucher, vendor, GSTIN, supplier invoice number and date, amount, taxable value | `bills` | The ledger being tested. Debit notes go in as negative amounts. |
| Delivery date, goods-receipt date, or service-completion date | Dates on `bills` | The actual acceptance date |
| Objection date and the date the objection was removed | Dates on `bills` | Deemed acceptance, or acceptance after the objection was removed |
| Agreed credit days, in writing | `agreement_days` on the bill, or `agreements` | The term that is then capped at 45 days |
| Payments: date, amount, vendor, and the bill they settle | `payments` | On-time, late, or still open. Agst Ref, bill reference, or invoice number names the bill. |
| Vendor code, name, GSTIN, Udyam number, category, major activity, registration date | `vendor_master` | Covered or not covered. Category is micro, small, medium, or not MSME. Activity is manufacturing, services, or trading. |
| NIC code, and investment or turnover | Optional vendor fields | A cross-check only. They do not replace the category you send. |
| Financial year end, and the date to treat as today | `fy_end`, `as_of` | The year-end test and the pay-by list |
| The snapshot from the last run | `rules_snapshot` | Vendor categories and the rule version, carried forward by the caller |

**Headers can stay as exported.** The tool describes a purchase register as exported. `mapping` overrides a header when it was read as the wrong field. `date_order` can be `DMY` or `MDY`. This page does not list every header alias, because the schema does not list them.

<!-- TODO Saifs AI: if the engine maps named headers without renaming, list those headers and the response field that reports the mapping. Until a signed-in call or the engine script shows them, do not publish an alias table. Also confirm the default for date_order when it is omitted. The schema lists DMY and MDY and does not state a default. -->

**File formats for the local engine.** CSV, TSV, or JSON. The engine reads the files named in the call: bills, payments, vendor master, agreements, and the rules snapshot. Save an Excel sheet as CSV first, or ask the agent to convert it in the sandbox. The script prints the decision cards and writes `msme_43bh_result.json` and `msme_rules_snapshot.json`.

## How it works

The connector offers two routes to the same calculation. The agent picks the route by the size of the data.

| | Hosted tool | Local engine |
| --- | --- | --- |
| Tool name | `msme_43bh_checker` | `msme_43bh_get_engine` |
| Use when | Inside the caps in [At a glance](#at-a-glance) | Over 20,000 bills, 50,000 payments, or 20,000 vendors, or when the rows must stay in your environment |
| What the agent sends | The rows and the settings | File paths, and other inputs in `params` |
| What comes back | The full result | A ready-to-run Python script. The tool says it is numpy-only. |
| Where the calculation runs | Saifs AI endpoint | Your agent's code-execution sandbox |
| Where your bills and vendor rows go | To the Saifs AI endpoint for the calculation | Nowhere. They stay in the sandbox. The tool states that no data reaches the server on this path. |
| Requires | Nothing extra | An agent host with code execution, and `numpy` |

**Local engine, step by step**

1. The user uploads or points the agent to the bills file, and to payments and the vendor master when they exist.
2. The agent calls `msme_43bh_get_engine` with those paths and any settings in `params`.
3. The connector returns the calculation script. Save it exactly as given. Do not shorten it or rewrite it.
4. The agent runs the script in its sandbox against the files.
5. The script prints the decision cards and writes `msme_43bh_result.json` and `msme_rules_snapshot.json` in the sandbox.
6. The agent reports the cards to the user. A person pays the supplier. The caller keeps the snapshot for the next run.

### Inside the connector

1. The user asks which bills to pay this week, or what the year-end disallowance is.
2. The agent chooses a tool and calls the connector, which passes the call to the Saifs AI MCP server over HTTPS at `https://msme-43bh-mcp-microsoft.saifs.ai/mcp`.
3. The server checks the signed-in account.
4. The hosted tool runs the calculation on the rows and returns the result. The engine tool returns the same calculation as a script, for the agent to run in its sandbox.
5. The agent reads the result and explains it in plain language. The dates and amounts come from the engine. The wording comes from the agent.

### Inside the calculation

Both routes are the same engine. The tool states what that engine returns. A signed-in response was not available, so this page does not name the JSON keys inside the result.

| Step | What the engine does | Method |
| --- | --- | --- |
| 1. Read the ledger | Takes bills, payments, the vendor master, and agreements as exported. `mapping` can correct a header. `as_of` is the date for status and for the pay-by list. | The call |
| 2. Classify the vendor | Reads category (micro, small, medium, or not MSME), major activity (manufacturing, services, or trading), and the Udyam registration date. Returns coverage for each bill. | Udyam fields you supply. `assume_not_trader` defaults to false. `assume_registered_before_supply` defaults to false. Investment and turnover are a cross-check only. |
| 3. Day of acceptance | Uses delivery, goods-receipt, or service-completion, plus objection and removal dates when you send them. Returns actual, deemed, or objection removed. | Deemed acceptance. The exact test that picks deemed is not published on the schema. See the note under this table. |
| 4. Statutory due date | 15 days from acceptance when there is no written term. The agreed term when there is one. The term is capped at 45 days. | MSMED Act section 15. `day_count` defaults to `plus_n` (acceptance date plus N). `inclusive` counts the acceptance day as day 1. |
| 5. Apply payments | Settles the bill named by Agst Ref, bill reference, or invoice number. If none is sent, applies the amount to the oldest open bill. Debit notes are negative amounts. | Oldest-open-bill allocation |
| 6. Pay-by list | Lists covered bills falling due within `horizon_days`. The default horizon is 7 days. The maximum is 366. | `pay_by_list`. `max_listed` defaults to 200. |
| 7. Year-end test | For `fy_end`, returns the disallowance by vendor under section 43B(h) and section 37(2)(g). | Section 43B(h) year-end test. The three branches below are the published rule. Confirm them on a live result before you rely on the wording. |
| 8. Reporting figures | Returns amounts for Form 3CD clause 22 and MSME Form I. | `form_3cd_msme1_figures` |
| 9. One bill | Explains the bill you name with `bill_ref` (voucher number or invoice number). | `explain_bill` |
| 10. Decision cards and snapshot | Puts decision cards first. Returns a vendor snapshot the caller can send back as `rules_snapshot`. | Dated, versioned rule tables |

The year-end test, as section 43B(h) states it, has three branches.

- Paid by the statutory due date: deductible in that year.
- The due date has passed and the bill is unpaid at the year end, or it was paid after the year end: disallowed that year, and deductible in the year it is paid.
- The due date falls after the year end: not yet overdue, so this year has no disallowance for that bill.

An agreed term of 60 days does not survive the cap. Acceptance on 5 February 2026 plus 45 days is 22 March 2026. That date is calendar arithmetic on the rule the tool names. It is not a response pasted from the server.

**Why the answer does not change.** The hosted tool is marked idempotent. The steps above are date tests and rule-table lookups. The tool does not name a random step. No language model writes the due date. The agent's wording around the numbers may vary. The numbers do not.

**What happens when an input is missing.** Nothing on the schema is required. If `job` is omitted, the tool runs what the inputs allow. The defaults above are the ones the schema states. A missing category is not filled in. A missing major activity is not treated as non-trader. A missing registration date is not treated as before the supply. A payment with no bill reference goes to the oldest open bill.

**Where the working is shown.** Decision cards come first. Then coverage, the day of acceptance, the statutory due date, the status, the pay-by list, the disallowance by vendor, Form 3CD clause 22, MSME Form I, and the vendor snapshot. This page does not name the JSON keys for those fields. See the sample response note in [For developers](#for-developers).

<!-- TODO Saifs AI: confirm from a signed-in call or the engine script: the exact test that chooses actual, deemed, or objection removed, including whether the objection window is 15 days; the status text for a missing category and for a missing acceptance date; whether more than a stated share of unclassified bills blocks the total (do not publish 30 percent until the script shows it); the three year-end branches above, including that a payment after year end and before the return due date stays disallowed for the unpaid year; where day_count and the rule version are shown; and the Udyam investment and turnover ceilings in force. Do not print those ceilings on this page until the script shows the dated table, because the ceilings change. The schema says investment and turnover are a cross-check only. -->

## Data handling and security

| Question | Local engine | Hosted tool |
| --- | --- | --- |
| Do bills, payments, or vendor rows reach Saifs AI? | No. The tool states that no data reaches the server on this path. | Yes. The rows passed in the call are sent for the calculation. |
| Where are they processed? | Your agent host's code sandbox | `msme-43bh-mcp-microsoft.saifs.ai`, over HTTPS |
| What happens to the rows after the call? | They stayed in your sandbox. The script writes `msme_43bh_result.json` and `msme_rules_snapshot.json` there. You keep the snapshot. | Not stated on the tool. See the note below. |
| What about the snapshot? | The caller keeps `msme_rules_snapshot.json` for the next run. | You pass the last snapshot back in as `rules_snapshot`. This page does not say the server stores it. |
| What can the script do? | It reads the local files named in the call and writes the two JSON files above. The tool says the script is numpy-only. | Not used on this path. |

<!-- TODO Saifs AI: confirm hosted-tool retention in writing before this sentence is made stronger. Say whether bill, payment, and vendor rows are discarded after the request, and whether any billing or audit log keeps a hash, the account, or the rows. Also confirm the hosting region, rate limits, and request timeout. Do not copy the audit-hash sentence from the Inventory Intelligence listing unless this server does the same. The privacy page at https://saifs.ai/privacy_policy/msme-43bh-payment-checker returned HTTP 200 on 25 November 2026 and does not state ledger-row retention. It is a general template. It mentions Facebook and Google sign-in, Stripe, and a contact at support@multiplewords.in. The connector package uses Microsoft single sign-on and taher@saifytech.com. Reconcile the page with the package before certification. -->

**What "sandbox" means.** The sandbox is the code-execution environment of your own agent host, such as Copilot Studio or Azure AI Foundry. It belongs to your tenant, not to Saifs AI. Your host's data policies still apply to it.

**Use the local engine only.** If the ledger must not leave your environment, turn off the hosted tool. In Copilot Studio, open the agent's **Tools** tab, select the MSME 43B(h) Payment Checker server, turn off **Allow all**, and switch off `msme_43bh_checker`. Leave `msme_43bh_get_engine` on. The agent can then use only the local engine. A tool added to the server later stays off until you enable it. This setting is per agent.

**Sign-in.** Users sign in with Microsoft single sign-on. Saifs AI holds only the account details needed for sign-in and billing. No API key is created, shared, or stored.

**No write access.** The connector has no action that creates, changes, or deletes a bill, a payment, a vendor, or a journal. Both tools are marked read-only and not destructive. The local engine is marked open-world because it reads files in the caller's sandbox and writes two JSON files there. It still does not write to Tally, an ERP, or Dataverse.

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
| Customer bill and vendor rows stored by the hosted tool | Not confirmed here. The local engine keeps rows in your sandbox. |
| Passwords held | None. Sign-in is Microsoft single sign-on. |
| Write access to your systems | None |
| Local engine | The script is returned in full for review before it runs. It needs `numpy`. |

**Independent audits.** Saifs AI does not yet hold an independently audited certificate such as SOC 2 or ISO 27001. If your review needs a completed security questionnaire, request it through the [contact page](https://saifs.ai/contact-us).

## Responsible use

This connector checks MSME payment dates and the section 43B(h) disallowance. It supports a human decision. It does not make one. A chartered accountant reviews the tax figures before they are filed.

The rule tables are versioned and dated. Say the figures were calculated under the rule version on the result. Do not say the return was accepted, the deduction was allowed, or the tax audit was cleared.

It does not compute interest under section 16 of the MSMED Act. It does not pay a supplier. It does not call the Udyam portal or any other government portal. Coverage comes only from the Udyam details you send.

There is no promise that a tax officer will accept the figure. A clear working means the dates can be rerun on these inputs. It does not mean the vendor category was the right one. You confirm the certificate.

Send business records you are allowed to process. Proprietor names, where the ledger has them, are part of the request. The tool does not need a personal identifier beyond the vendor record you already keep.

## Token usage and cost

With the hosted tool, the rows are part of the tool call, so token use grows with the file. With the local engine, the agent receives a script and the rows stay in the sandbox, so token use does not grow with the file.

<!-- TODO Saifs AI: measure tokens before publishing figures. Use one named tokenizer on three sizes: a small file, the hosted limit (20,001 bills, 50,001 payments, or 20,001 vendors), and a larger file. Count both directions. Include the script, the write into the sandbox, and the printed decision cards. Record input and output tokens, a dated reference price per million, the first-run and later-run multipliers, and a monthly example. State where the hosted tool uses fewer tokens. Do not copy the Inventory Intelligence token table. -->

Until those figures are measured, this page does not state a token count or a dollar cost.

**Which route to choose, before the token table exists**

- **Inside the hosted limits, and a single question:** the hosted tool. It is one call.
- **Over 20,000 bills, 50,000 payments, or 20,000 vendors:** the local engine. It is the only route.
- **The rows must stay in your environment:** the local engine. Turn the hosted tool off.
- **The same large file, many times in one session:** the local engine, if the sandbox keeps the script.

**Hosts that bill per action.** Some hosts charge a fixed amount per tool call and do not bill by token. On those hosts, a smaller token count may not reduce the bill. The local engine may also use one extra action to run the script. Choose it there for file size and for where the data stays, not for a lower token bill.

## Prerequisites

- A Saifs AI account, created by signing in with Microsoft single sign-on
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Access to Copilot Studio, Microsoft 365 Copilot, Azure AI Foundry, Power Automate, Power Apps, or Azure Logic Apps
- Purchase bills and the Udyam details you hold. Payments, agreements, `fy_end`, and `as_of` make the disallowance and the pay-by list complete.
- For the local engine: code execution enabled on the agent host, and `numpy` available there

## How to get credentials

No keys or secrets are needed. When you create a connection, you are redirected to sign in with your Microsoft account and approve access. If the connection later expires, sign in again from the connection page.

## Get started with your connector

1. In Copilot Studio, open your agent and go to **Tools**, then **Add a tool**.
2. Search for **MSME 43B(h) Payment Checker** and select it.
3. Create a connection and sign in.
4. Add this to the agent's instructions: "For any question about MSME supplier due dates, a pay-by list, section 43B(h), Form 3CD clause 22, or MSME Form I, call MSME 43B(h) Payment Checker. Do not count the days yourself. Do not pay a supplier. Do not compute interest."
5. Provide the rows by pasting them, uploading them, or pointing the agent at a file. Include the Udyam category, major activity, and registration date. Include payments when you want the disallowance.
6. Ask in plain language. Two examples are "Which MSME supplier bills must we pay this week?" and "How much will section 43B(h) disallow for this financial year?"

Treat the result as dates and figures under the rules on the result. A chartered accountant reviews them before a return is filed. A person makes the payment.

## For administrators

Enabling this connector gives the organisation one approved way to decide which MSME bills to pay by when, and what section 43B(h) will disallow. It does that inside the agents your makers already build.

**Why this is an administrator decision.** Without a calculation tool, makers ask an agent to count MSME due dates from a spreadsheet. A language model can give a different date on the next run, and the result cannot be compared at tax audit. A governed connector uses one dated rule table for every agent and team.

- **You choose who can connect it.** Tenant and environment permissions apply. Only users who can create a connection can call the tools.
- **You keep your system of record.** The connector calculates and explains. It does not pay a supplier or write back to any system.
- **You can check the number.** The same file and settings can be run again. The tool is marked idempotent. A missing category is not treated as covered.
- **You control where data goes.** Turn off the hosted tool per agent to keep bill and vendor rows in your environment.
- **You set data policy as usual.** Place the connector in the Business, Non-business, or Blocked group with Power Platform data loss prevention policies.
- **There are no secrets to rotate.** Expired connections are renewed by signing in again.

**Review checklist**

- Decide whether the hosted tool is allowed, or local engine only
- Assign the connector to the correct data policy group
- Confirm that makers send Udyam details they are allowed to use, and that they do not expect a portal lookup
- Confirm that the caller, not the server, stores `msme_rules_snapshot.json` between runs
- Review the privacy policy and terms
- Read the self-assessments listed under Security and compliance, once they are published
- Review the local engine script if your policy requires it
- Confirm the hosted-tool retention note in Data handling and security before a wide rollout

## For developers

Nothing on the schema is marked required. Send the arrays the job needs. The tool says decision cards come back first. This page does not name the other JSON keys until a signed-in call is pasted below.

<!-- TODO Saifs AI: confirm the minimum arrays each job actually requires, from a signed-in call. The schema marks no field as required. The job names and input descriptions are what this table uses. -->

### Bill fields

One object per bill, in `bills`. Up to 20,001 items. Debit notes are negative amounts.

| Field | Meaning | Needed for |
| --- | --- | --- |
| Voucher number, supplier invoice number | The bill reference. `bill_ref` accepts either. | The pay-by list, and `explain_bill` |
| Vendor code, vendor name, GSTIN | The link to `vendor_master` | Coverage |
| Invoice date | The supplier's invoice date | A date on the bill. It is not, by itself, the acceptance date. |
| Delivery date, goods-receipt date, or service-completion date | When the buyer received the supply | The actual acceptance date |
| Objection date, removal date | A written objection, and when it was removed | Deemed acceptance, or acceptance after the objection was removed |
| Amount, taxable value | The bill value | The pay-by list, the disallowance, and Form 3CD clause 22 |
| Agreed credit days, in writing | The contractual term | The statutory due date, before the 45-day cap |

### Payments, vendors, and agreements

| Input | Meaning | Needed for |
| --- | --- | --- |
| `payments` | Date, amount, vendor, and the bill settled. Up to 50,001 rows. If the bill is not named, the amount goes to the oldest open bill. | Status, and the year-end disallowance |
| `vendor_master` | Vendor code, name, GSTIN, Udyam number, category, major activity, and Udyam registration date. Up to 20,001 rows. Optional: category valid-from, NIC, investment, turnover, and written credit days. | Coverage. Investment and turnover are a cross-check only. |
| `agreements` | Optional written terms by vendor: vendor reference and agreement days. Up to 5,001 rows. | The agreed term, where it is not already on the bill |
| `rules_snapshot` | Vendor categories and the rule version from the last run | The next run, if you want that snapshot carried forward |

### Jobs

Set `job` to run one part. Omit it and the tool runs what the inputs allow.

| Job | What it returns |
| --- | --- |
| `classify_vendors` | Coverage from the vendor master and the Udyam fields |
| `due_dates` | Acceptance, the statutory due date, and status |
| `explain_bill` | The working for the bill in `bill_ref` |
| `pay_by_list` | Covered bills due within the horizon |
| `year_end_disallowance` | The section 43B(h) disallowance by vendor for `fy_end`, also labelled section 37(2)(g) |
| `form_3cd_msme1_figures` | Form 3CD clause 22 and MSME Form I figures |

### Options and defaults

| Option | Default | Purpose |
| --- | --- | --- |
| `fy_end` | None | Financial year end for the year-end test, such as 2026-03-31 |
| `as_of` | None stated | The date used for status and for the pay-by list |
| `horizon_days` | 7 | How far ahead the pay-by list looks. Minimum 0. Maximum 366. |
| `day_count` | `plus_n` | `plus_n` is acceptance date plus N. `inclusive` counts the acceptance day as day 1. |
| `assume_not_trader` | false | When true, a micro or small vendor with no major activity is treated as a non-trader |
| `assume_registered_before_supply` | false | When true, a missing Udyam registration date is treated as before the supply |
| `date_order` | None stated | `DMY` or `MDY`, when the export's dates are ambiguous |
| `mapping` | None | Header to standard field, when a column was read as the wrong field |
| `currency_symbol` | Empty | A symbol on the cards, such as Rs. No currency conversion. |
| `max_listed` | 200 | How many rows to list. Minimum 1. Maximum 100,000. |
| `bill_ref` | None | The voucher or invoice to explain |
| `rules_snapshot` | None | The snapshot from the last run |

<!-- TODO Saifs AI: confirm from a live call whether an omitted as_of uses the server date, and whether an omitted date_order assumes DMY. The schema describes both and states a default for neither. -->

### Sample request

The rows below are made up. They show the shape of a small call. They are not a customer ledger. Header names follow the words on the tool. `mapping` can correct a header the engine reads differently.

```json
{
  "bills": [
    {
      "voucher_no": "PUR-1042",
      "vendor_code": "V-18",
      "invoice_no": "SC-221",
      "invoice_date": "2026-02-01",
      "delivery_date": "2026-02-05",
      "amount": 184000,
      "agreement_days": 60
    }
  ],
  "payments": [],
  "vendor_master": [
    {
      "vendor_code": "V-18",
      "name": "Northwind Components",
      "udyam_category": "small",
      "major_activity": "manufacturing",
      "udyam_reg_date": "2024-06-01"
    }
  ],
  "fy_end": "2026-03-31",
  "as_of": "2026-02-16",
  "horizon_days": 45,
  "day_count": "plus_n",
  "currency_symbol": "Rs"
}
```

### Sample response (shortened)

A signed-in call was not available. On 25 November 2026 the endpoint `https://msme-43bh-mcp-microsoft.saifs.ai/mcp` returned HTTP 401 without a token. This page does not invent a response or its field names.

<!-- TODO Saifs AI: replace this note with a shortened real response from msme_43bh_checker. Include one bill's coverage, acceptance type, statutory due date, status, one pay-by row, and the field that explains the date. Do not invent field names. Do not paste a response from another product. -->

### Response sections

The tool states what comes back. The names in the first column are the words on the tool, not confirmed JSON keys.

| What comes back | Use it for |
| --- | --- |
| Decision cards | The plain-language action, first |
| Coverage per bill | Whether that supplier's bill is in the MSME test |
| Day of acceptance | Actual, deemed, or objection removed |
| Statutory due date | The MSMED Act section 15 date, after the 45-day cap |
| Status | Where the bill stands on `as_of` |
| Pay-by list | Covered bills due inside the horizon, with days left and amount |
| Year-end disallowance by vendor | What section 43B(h) will disallow, also labelled section 37(2)(g) |
| Form 3CD clause 22 and MSME Form I | The tax-audit and half-yearly return figures |
| Vendor snapshot | The categories and rule version to send back as `rules_snapshot` |

<!-- TODO Saifs AI: replace the first column with the JSON keys from a live response. -->

## Known issues and limitations

- **Flags and dates only.** It does not pay a supplier, post a payment, or update any system.
- **Not tax or legal advice.** A chartered accountant reviews the figures. A result is not a statement that the deduction was allowed.
- **No section 16 interest.** The tool never computes it.
- **No portal lookup.** Udyam status comes from the fields you send. The connector does not call a government portal.
- **Hosted tool size.** 20,001 bills, 50,001 payments, 20,001 vendors, and 5,001 agreements. Above 20,000 bills, 50,000 payments, or 20,000 vendors, use the local engine.
- **Missing vendor facts stay missing.** Category, major activity, and registration date are not invented. The two assume-switches default to false.
- **Payments with no bill reference.** The amount is applied to the oldest open bill for that vendor.
- **Listed rows.** `max_listed` defaults to 200. Raise it when the pay-by list looks short.
- **Currency.** `currency_symbol` is a label on the cards. There is no conversion between currencies.
- **One file at a time.** The schema has no multi-entity field. Run one call per entity, and keep each entity's snapshot.
- **India rules.** The rule table is versioned. This page does not quote Udyam investment or turnover ceilings.
- **The caller keeps the snapshot.** Without `rules_snapshot`, the next run does not carry the last vendor categories and rule version forward.
- **Regions.** Not available in US Government (GCC, GCC High, DoD) or China (21Vianet) clouds.

<!-- TODO Saifs AI: rate limits, timeouts, and the service region are not in the tool schema. Add them here when confirmed. Also add the refusal text once the engine script confirms it. -->

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| The tool cannot run, or asks you to sign in again | The connection has expired | Open the connection and sign in again |
| The agent gives a due date without calling the tool | The agent instructions do not require the tool | Add the instruction in Get started |
| A vendor you expected to be covered is not | Category or major activity was blank, and both assume-switches are false | Send the Udyam category and the major activity. Turn on `assume_not_trader` only after you have checked that the vendor is not a trader. |
| A supplier registered late is treated as not yet registered on the supply date | `assume_registered_before_supply` is false, and the registration date was missing or is after the supply | Send `udyam_reg_date`. Turn the switch on only when you have checked the certificate. |
| A payment cleared the wrong bill | The payment had no Agst Ref, bill reference, or invoice number | Send the bill reference. Otherwise the tool uses the oldest open bill. |
| The pay-by list stops after 200 bills | `max_listed` is still at its default | Raise `max_listed`, up to 100,000 |
| The local engine does not run | Code execution is not enabled, or `numpy` is missing | Enable code execution and `numpy`, or use the hosted tool inside the caps |

<!-- TODO Saifs AI: replace any row above if a live call uses different status text. Do not add error codes that the server has not returned. -->

## FAQ

**Do I need a tax specialist on every bill?**
No. A user asks in plain language, and the agent returns dates and a pay-by list. A chartered accountant reviews the figures before they are filed.

**What is the least data I need?**
Bills and the Udyam details for those vendors. Delivery or goods-receipt dates, written credit days, and payments make the statutory due date and the disallowance complete.

**Does my data leave my environment?**
Not with the local engine. The tool states that no data reaches the server on that path. With the hosted tool, the rows in the call are sent to Saifs AI for the calculation. You can turn the hosted tool off.

**Will it change anything in my systems?**
No. It does not pay a supplier and it does not write to Tally, an ERP, or Dataverse.

**I already have Tally or an ERP with its own MCP server. Why add this?**
That server gives the agent your records. This connector gives the agent the statutory due date, the pay-by list, and the section 43B(h) disallowance. It works on rows from any source.

**Why do I get the same dates every time?**
The hosted tool is marked idempotent. The same input and settings return the same numbers. The agent's wording around the numbers may vary.

**What happens if a vendor category or an acceptance date is missing?**
The tool does not fill the category in. It does not assume a blank activity means "not a trader". Send the missing fact, or set the matching switch only after you have checked it. The exact status words are confirmed from a live response, not guessed on this page.

**Can I check how a statutory due date was produced?**
Yes. Ask for one bill. The tool returns the acceptance type, the statutory due date, and the reason. The rule version travels with the snapshot.

**How large a file can it handle?**
The hosted tool allows 20,001 bills, 50,001 payments, 20,001 vendors, and 5,001 agreements. Larger ledgers use the local engine.

**Can it run on a schedule?**
Yes. Call it from a flow with a recurrence trigger. The tools themselves run only when called. Pass the last `rules_snapshot` from the flow's own store.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment).

**Does it compute interest on a late MSME bill?**
No. It does not compute interest under section 16 of the MSMED Act.

## Support

**MSME 43B(h) Payment Checker - Saifs AI** is published by Saifs AI.

- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Privacy: [https://saifs.ai/privacy_policy/msme-43bh-payment-checker](https://saifs.ai/privacy_policy/msme-43bh-payment-checker)
- Terms: [https://saifs.ai/eula](https://saifs.ai/eula)
- Sign-in: Microsoft single sign-on (OAuth 2.0). No API keys.
- MCP endpoint: [https://msme-43bh-mcp-microsoft.saifs.ai/mcp](https://msme-43bh-mcp-microsoft.saifs.ai/mcp)

<!-- TODO Saifs AI: add the Learn more link here after the product page is published. See the note at the top of this file. -->
