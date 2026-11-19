# Financial Fraud Detection - Saifs AI

**Financial Fraud Detection - Saifs AI** helps finance, audit, and procurement teams decide **which entries to review** for possible fraud or error. You send a ledger, a payment run, a journal, an expense file, or company statements. It returns decision cards, Benford digit tests, entries to review, duplicate groups, a Beneish M-Score, and a watch snapshot.

It is built for internal audit, controllership, accounts payable, procurement, forensic accounting, and expense teams. They need the same flags when they run the file again.

**In plain language:** you give it the export you already have. It tells you which entries to review, and the statistical reason for each flag.

**For technical readers:** the checks use Benford's law, chi-square, mean absolute deviation (MAD), z-scores, duplicate matching, and the Beneish M-Score.

It flags items for human review. It does not accuse anyone. It is not an audit opinion or a regulatory determination.

**Publisher:** Saifs AI
**Website:** [https://saifs.ai](https://saifs.ai)
**Pricing:** [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
**Contact and help:** [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
**Privacy:** [https://saifs.ai/privacy_policy/financial-fraud-detection](https://saifs.ai/privacy_policy/financial-fraud-detection)
**Terms:** [https://saifs.ai/eula](https://saifs.ai/eula)
**Sign-in:** Microsoft single sign-on (OAuth 2.0). No API keys.
**MCP endpoint:** [https://financial-anomaly-mcp-microsoft.saifs.ai/mcp](https://financial-anomaly-mcp-microsoft.saifs.ai/mcp)

<!-- TODO Saifs AI: publish the product page, then add a Learn more line here and in Support. https://saifs.ai/financial-fraud-detection returned HTTP 404 on 17 November 2026. Do not link it until it opens. -->

| If you are | Start with |
| --- | --- |
| A business owner, controller, or audit lead | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [Where the data comes from](#where-the-data-comes-from) |
| An administrator or security reviewer | [Data handling and security](#data-handling-and-security), [Security and compliance](#security-and-compliance), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [Where the data comes from](#where-the-data-comes-from), [For developers](#for-developers) |
| A finance or audit reviewer checking the method | [Inside the calculation](#inside-the-calculation), [Responsible use](#responsible-use) |

## The problem it solves

Most teams still scan invoices, expense claims, journal entries, and vendor payments by eye. A copied amount, a split invoice, or a digit pattern is hard to see in a long list. Fraud detection that depends on scrolling misses what a count would show. Anomaly detection done by habit does not test the digits.

Journal entry testing with Benford's law, and an earnings screen with the Beneish M-Score, are standard forensic accounting checks. They usually sit in audit software that many finance teams never set up. A language model asked to "look at the ledger" can describe a pattern and still get the count wrong.

Financial Fraud Detection makes those checks available to any AI agent:

- **Which entries to review:** unusual amounts, possible duplicates, and split invoices, each with a reason.
- **Whether the digits fit:** a Benford's law test on amounts that span many sizes, and a refusal when the column is the wrong kind of number.
- **Whether two years of statements stand out:** a Beneish M-Score against a stated cutoff.
- **What changed since last time:** a comparison with a snapshot you keep, not a watch that runs by itself.

## Why choose it: the same flags every time

Financial Fraud Detection is deterministic. The same rows and the same settings, including the same seed, produce the same flags. The tests are fixed statistical methods. They are not written afresh by a language model each time you ask.

That matters because an auditor or a controller acts on a list of entries. The list has to be the same on Tuesday as it was on Monday, when the file has not changed. The agent's wording around the flags may vary. The flags do not.

| What it does | Established method |
| --- | --- |
| Tests whether leading digits match the pattern expected in naturally occurring amounts | Benford's law, compared with chi-square and mean absolute deviation (MAD) |
| Flags amounts that sit far from the typical amount for that vendor or account | z-score, once enough peer payments exist |
| Flags possible duplicate payments inside a day window | Matching on amount, vendor, date, and invoice number when you have it |
| Flags split invoices and amounts sitting just below an approval limit | Rules that run only when you send approval limits |
| Scores two years of statements for earnings-quality red flags | Beneish M-Score, 8-variable or 5-variable |
| Compares this batch with the last one | A caller-held watch snapshot of counts and salted hashes |

**It does not guess.** Benford's law is refused for ids, codes, rates, invoice numbers, phone numbers, and scores. Banks, insurers, and NBFCs are skipped on the M-Score unless you turn that off. Split-invoice checks are skipped when approval limits are missing. The tool reports what was missing. It does not fill the gap with a silent score.

**It flags and does not act.** Every result is a signal for a person, not proof of fraud or error. It never posts a journal, blocks a payment, or writes to an ERP or accounting system.

## Who should use this

| Role | How it helps |
| --- | --- |
| **Internal auditors** | Journal entry testing on the quarter's export, with a list of entries to review and the reason for each flag |
| **Controllers and finance leads** | Anomaly detection on the ledger, plus a Beneish M-Score when two years of statements are available |
| **Accounts payable and procurement** | Possible duplicate payments and split invoices before a payment run |
| **Forensic accountants** | The same forensic accounting tests, with the statistical basis attached, for a human review |
| **Expense-management teams** | Unusual claim amounts and a Benford's law test on the amounts, framed as items to review |
| **Business owners** | A short list of what to look at first, without a specialist sitting in the review |
| **Agent makers and developers** | A tested fraud detection calculation inside an agent, so the agent does not invent the statistics |

**Best fit:** internal audit, controllership, accounts payable, procurement, forensic accounting practices, and expense teams that review ledgers, payments, or statements on a cycle.

**Not required:** an audit software suite, a data science team, or an ERP. An export with amounts is enough to start. Invoice numbers, vendors, approval limits, and two years of statements make the review sharper.

## Use cases

Each use case below can be built as an agent. The agent types are listed in [Agents you can build](#agents-you-can-build).

**1. Journal entry testing**
Scenario: an internal auditor has the quarter's journal export.
Outcome: entries flagged for review, with the statistical reason, so a person can open the vouchers.
Ask: "Flag unusual journal entries this quarter."

**2. Benford's law on expenses**
Scenario: expense claims span many amounts, and nobody can count first digits by hand.
Outcome: a Benford's law digit test, and a refusal if the column is an id, a code, or a rate.
Ask: "Run Benford's law on these expenses."

**3. Duplicate payments before a run**
Scenario: accounts payable is about to pay vendors.
Outcome: possible duplicate payments inside the day window. The check is stronger when the invoice number is present.
Ask: "Find possible duplicate payments before we pay."

**4. Split invoices and approval limits**
Scenario: invoices sometimes sit just under a known approval limit.
Outcome: split-invoice clusters and amounts just below a limit. This check runs only when approval limits are in the call.
Ask: "Check this payment run for split invoices under our approval limits."

**5. Beneish M-Score on the statements**
Scenario: a controller has two years of figures, in standard lines or Schedule III names.
Outcome: a Beneish M-Score against the cutoff, or a skip when the company is a bank, an insurer, or an NBFC.
Ask: "Check this company's Beneish M-Score."

**6. Explain one flag**
Scenario: a reviewer has the list and needs the basis for one row before they call it fraud.
Outcome: the statistical basis for that entry, invoice, row number, or company. The wording stays "flagged for review."
Ask: "Explain why this entry was flagged for review."

**7. This period against the last snapshot**
Scenario: last month's watch snapshot was saved, and this month's ledger is ready.
Outcome: a comparison of this batch with that snapshot. The server does not keep the snapshot.
Ask: "What changed in this ledger since the last snapshot?"

**8. Find suspicious entries in claims**
Scenario: an expense team has a claim file with amounts, dates, and who submitted them.
Outcome: unusual amounts and digit tests, as items for a person. Not a finding that a claim is false.
Ask: "Find suspicious entries in this expense file."

**9. A full ledger export**
Scenario: the file is over about 20,000 transactions, 100,000 numbers, or 400 statement rows.
Outcome: the same fraud detection checks run in the agent's sandbox. The rows stay there.
Ask: "Run anomaly detection on this full ledger export."

## Agents you can build

Each agent is one agent in your agent host with this connector added as a tool. The last column shows what else to add.

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Journal review agent | Runs journal entry testing and returns the entries to review | A journal or ledger file, or a data source from [Where the data comes from](#where-the-data-comes-from) | 1, 8 |
| Benford agent | Runs Benford's law on a column of amounts and says when the test does not apply | The amount column, and `field_name` so an id or a rate can be refused | 2 |
| Duplicate and split agent | Finds possible duplicate payments and, when limits are sent, split invoices | Approval limits, if the split check should run | 3, 4 |
| Earnings screen agent | Scores two years of statements with the Beneish M-Score | Statement rows for two years, with sales and total assets | 5 |
| Flag explanation agent | Returns the statistical basis for one flagged item | The entry id, invoice number, row, or company, and `job` set to `explain_flag` | 6 |
| Period comparison agent | Compares this batch with the last watch snapshot | The snapshot file from the previous run, and the same `snapshot_salt` | 7 |
| Large-ledger agent | Runs the local engine on a full export | Code execution on the agent host, and `numpy` | 9 |
| ERP review agent | Reads journals or payments from the ERP, then flags rows for a person | The ERP's own MCP server. This connector does not write the ERP. | 1, 3 |
| Scheduled review agent | Runs the checks on a recurrence and sends the list for review | A Power Automate flow with a recurrence trigger, plus email or Teams. Store the watch snapshot between runs. | 1, 7 |

The tools run on request. Scheduling comes from the flow, not from the connector. There is no alert inside the server.

## At a glance

| Item | Detail |
| --- | --- |
| Tools | `financial_anomaly` (hosted calculation) and `financial_anomaly_get_engine` (local engine) |
| Checks inside either tool | Set `job` to one of `benford_analysis`, `flag_anomalous_transactions`, `detect_duplicate_payments`, `earnings_manipulation_score`, `explain_flag`, or `watch_transactions`. Omit `job` to run every check the inputs allow. |
| Minimum input | Nothing is required. Send whatever columns you have. An amount, or debit and credit, is what the entry checks need. |
| Hosted tool size | About 20,000 transactions, 100,000 numbers, and 400 statement rows. Transactions and invoices together stay within 20,000 rows. Each of those arrays allows 20,001 objects, which fits a header row plus about 20,000 value rows when only one array is sent. |
| Larger files | Local engine. It runs in your agent's code sandbox and needs only `numpy`. |
| Writes to your systems | None. Both tools are marked read-only and not destructive. |
| Engine version | Not named in the tool schema. |

<!-- TODO Saifs AI: if a live response includes an engine version field, name it in At a glance. Do not copy metadata.model_version from another product. -->

## Where the data comes from

Financial Fraud Detection does not connect to your systems. The agent brings the rows to it. Any source that can supply a ledger, a payment run, a journal, claims, or statements can feed it. The connector needs only rows. It has not been certified against each system named below.

| Data source | How the agent gets the data | Best route |
| --- | --- | --- |
| Spreadsheet or text export from accounting or ERP software (for example Tally, QuickBooks, Zoho, SAP, Oracle, Xero) | The user uploads the file to the agent | Local engine for large files, hosted tool for files inside the limits |
| An ERP that has its own MCP server (for example Dynamics 365, Business Central, SAP, NetSuite) | The agent reads journals, payments, or statements with the ERP's tools, then passes the rows on | Hosted tool up to the limits above. Above that, export a file and use the local engine. |
| Dataverse tables | The Dataverse MCP server or the Dataverse connector | Same as above |
| SQL database, Excel workbook online, or SharePoint list | A Power Platform connector on the same agent or flow | Same as above |
| A scheduled flow | A Power Automate flow reads the source on a schedule and calls this connector | Hosted tool, if the file is inside the limits |
| The previous run of this connector | Pass `watch_snapshot` from the last result. The caller stores it. | Either route |

**Works alongside your ERP's MCP server.** That server gives the agent the ERP's records and functions. This connector adds the forensic accounting checks. No audit module has to be configured. The ERP supplies the rows. Financial Fraud Detection flags entries for review. A person decides what to do. Neither tool writes back.

**What to pull from the source**

| Data in your system | Field it fills | Why it matters |
| --- | --- | --- |
| Journal, payment, or expense lines | `transactions` | Amounts, dates, vendors, and invoice numbers for the entry checks |
| Accounts payable invoices | `invoices` | Checked together with transactions. The two arrays together stay within 20,000 rows. |
| One column of measured amounts | `numbers` and `field_name` | A Benford's law test only. `field_name` is how ids, codes, and rates are refused. |
| Two years of statements | `financials` | The Beneish M-Score. Sales and total assets are required for both years. |
| Approval limits | `approval_limits` | Without them, split-invoice and just-below-limit checks are skipped |
| Last run | `watch_snapshot` | Counts and salted hashes only. Used to compare this batch with the last one. |

**Column names can stay as they are.** The tool description tells the agent not to rename Tally, QuickBooks, Zoho, SAP, Oracle, Xero, or CSV headers. These aliases are named on the tool. If a column is mapped to the wrong field, send `mapping`, for example `{"Particulars": "vendor"}`.

| Field | Headers recognised (from the tool schema) |
| --- | --- |
| `entry_id` | Vch No, JE Number, Num, Document No |
| `date` | Voucher Date, GL Date, Txn Date. YYYY-MM-DD is preferred. |
| `vendor` | Particulars, Name, Paid To |
| `user` | Entered By, Employee, Claimant |
| `company` | Entity, Ticker |
| `year` | A fiscal year such as FY 2023-24 is read as 2024 |
| `sales` | Revenue from Operations, Turnover |
| `receivables` | Debtors, Sundry Debtors |
| `ppe` | Net Block, Tangible Assets |
| `long_term_debt` | Non-current Borrowings |
| `net_income` | PAT, Profit for the Year |

`cogs` can be built from gross profit, or from materials plus purchases plus the inventory change, when the cost line itself is missing.

**File formats for the local engine.** CSV, TSV, or JSON. A header row followed by value rows is accepted. The transactions path defaults to `ledger.csv` when no path is given. Save an Excel sheet as CSV first, or ask the agent to convert it in the sandbox. The script writes `financial_anomaly_result.json` and `watch_snapshot.json`.

## How it works

The connector offers two routes to the same anomaly detection checks. The agent picks the route by the size of the file.

| | Hosted tool | Local engine |
| --- | --- | --- |
| Tool name | `financial_anomaly` | `financial_anomaly_get_engine` |
| Use when | About 20,000 transactions or fewer, 100,000 numbers or fewer, and 400 statement rows or fewer | Anything larger, or when the rows must stay in your environment |
| What the agent sends | The rows and the settings | File paths and settings only |
| What comes back | Decision cards, digit tests, entries to review, duplicate groups, an M-Score when statements were sent, and an updated watch snapshot | A ready-to-run Python script |
| Where the calculation runs | Saifs AI endpoint | Your agent's code-execution sandbox |
| Where the financial rows go | To the Saifs AI endpoint for the calculation | The tool states that the data never leaves the user's machine on this route |
| Requires | Nothing extra | An agent host with code execution, and `numpy` |

**Local engine, step by step**

1. The user uploads the file, or points the agent at it.
2. The agent calls `financial_anomaly_get_engine` with the file path and any settings.
3. The connector returns the calculation script. Save it exactly as given. Do not shorten it, retype it, or replace it with other code.
4. The agent runs the script in its sandbox against the file.
5. The script prints the decision cards and writes `financial_anomaly_result.json` and `watch_snapshot.json` in the sandbox.
6. The agent reports the flags to the user. Keep `watch_snapshot.json` and pass it on the next run.

### Inside the connector

1. The user asks which entries to review, or asks for a Benford test, a duplicate check, an M-Score, or a comparison with the last run.
2. The agent chooses a tool and calls the connector. The call goes to the Saifs AI MCP server over HTTPS at `https://financial-anomaly-mcp-microsoft.saifs.ai/mcp`.
3. The server checks the signed-in account.
4. The hosted tool runs the checks on the rows and returns the result. The engine tool returns the same checks as a script, for the agent to run in its sandbox.
5. The agent reads the result and explains it in plain language. The flags come from the engine. The wording comes from the agent.

### Inside the calculation

Both routes run the same checks. A check runs when its inputs are in the call, or when `job` names it. Omit `job` and every check the inputs allow will run.

| Step | What the engine does | Method |
| --- | --- | --- |
| 1. Read the export | Accepts a header row, keeps the column names you sent, and applies `mapping` when a header was read as the wrong field. `date_order` of `DMY` or `MDY` tells it how to read an ambiguous date. Omit `date_order` and the export decides. | Stated on the tool |
| 2. Benford digit test | Compares observed digit frequencies with the frequencies Benford's law expects. The expected share of leading digit d is log10(1 + 1/d). Amounts at or below `min_value` (default 10) are ignored. | Benford's law, chi-square, and MAD |
| 3. Refuse the wrong column | Does not run Benford's law on ids, codes, rates, invoice numbers, phone numbers, or scores. `field_name` is used for that refusal on a `numbers` column. | Applicability check |
| 4. Unusual amounts | Compares an amount with peer payments for that vendor or account, once at least `peer_min` peers exist (default 15, minimum 5). | z-score |
| 5. Possible duplicates | Looks for payments that may be the same within `duplicate_window_days` (default 60). Ignores amounts at or below `min_duplicate_amount` (default 1). An invoice number makes this a stronger check. | Matching inside the window |
| 6. Split invoices and limits | Looks for split invoices and for amounts sitting just below a limit. Runs only when `approval_limits` is sent. A cluster can span `split_window_days` (default 3). | Approval-limit rules |
| 7. Beneish M-Score | Needs two years. Sales and total assets are required in both. With operating cash flow, the 8-variable score is used and the default cutoff is -1.78. Without it, the 5-variable score is used and the cutoff is -2.22. Above the cutoff, the year is flagged for review. | Beneish M-Score |
| 8. Companies the M-Score was not built for | Skips banks, insurers, and NBFCs when `exclude_financial_companies` is true, which is the default. | Stated exclusion |
| 9. Watch snapshot | Compares this batch with `watch_snapshot` from the last run. The snapshot is counts and salted hashes. `forget` (default 1) makes this batch the next baseline. Reuse `snapshot_salt` or the next comparison will not match. The server does not store the snapshot. | Caller-held snapshot |
| 10. Explain one item | With `job` set to `explain_flag`, explains an entry id, an invoice number, `{"row": n}`, or `{"company": name}`. | The check that flagged it |
| 11. Decision cards | Turns the results into actions for a person. `max_listed` (default 100) caps how many review rows are listed. The totals still count every flag. | Fixed cards filled with the calculated results |

**Why the answer does not change.** The steps above are fixed tests. There is no language model in the calculation. Sampling checks use `seed` (default 7). The tool states that the same inputs and the same seed give the same result. A new snapshot, a new salt, or a new seed is a new input.

**What happens when data is missing.** Nothing is required, so the call still runs. The tool reports what was missing. Defaults in the table above are the ones on the schema. They are listed again under [Options and defaults](#options-and-defaults). This page does not add a default the schema does not state.

**Where the working is shown.** Decision cards carry the plain-language action. `explain_flag` is the check that returns the basis for one item. This page does not name a JSON field for that basis. See the sample response note in [For developers](#for-developers).

<!-- TODO Saifs AI: confirm the numeric conformity bands for MAD and chi-square, and the exact "just below a limit" band, from a signed-in call or from the engine script. They are not in the tool schema. Do not publish Nigrini cutoff numbers until the script shows them. -->

## Data handling and security

| Question | Local engine | Hosted tool |
| --- | --- | --- |
| Do ledger rows, amounts, or statements reach Saifs AI? | No. The tool states that the data never leaves the user's machine on this route. | Yes. The rows passed in the call are sent for the calculation. |
| Where are they processed? | Your agent host's code sandbox | `financial-anomaly-mcp-microsoft.saifs.ai`, over HTTPS |
| What happens to the rows after the call? | They stayed in your sandbox. The script writes `financial_anomaly_result.json` and `watch_snapshot.json` there. | Not stated on the tool, beyond the snapshot. See the note below. |
| What about the watch snapshot? | The caller keeps `watch_snapshot.json`. | The tool states that the server stores nothing for that snapshot. The caller sends it back next time. Counts and salted hashes only. |
| What can the script do? | It reads the local files and writes the two JSON files above. It needs `numpy`. | Not used on this path. |

<!-- TODO Saifs AI: confirm hosted-tool retention in writing before this sentence is made stronger. Say whether ledger rows, amounts, and statements are discarded after the request, and whether any billing or audit log keeps a hash, the account, or the rows. Also confirm the hosting region, rate limits, and request timeout. Do not copy the audit-hash sentence from the Inventory listing unless this server does the same. The schema line "The server stores nothing" is on the watch snapshot, and this page scopes it that way. -->

**What "sandbox" means.** The sandbox is the code-execution environment of your own agent host, such as Copilot Studio or Azure AI Foundry. It belongs to your tenant, not to Saifs AI. Your host's data policies still apply to it.

**Use the local engine only.** If ledger rows must not leave your environment, turn off the hosted tool. In Copilot Studio, open the agent's **Tools** tab, select the Financial Fraud Detection server, turn off **Allow all**, and switch off `financial_anomaly`. Leave `financial_anomaly_get_engine` on. The agent can then use only the local engine. A tool added to the server later stays off until you enable it. This setting is per agent.

**Sign-in.** Users sign in with Microsoft single sign-on. Saifs AI holds only the account details needed for sign-in and billing. No API key is created, shared, or stored.

**No write access.** The connector has no action that creates, changes, or deletes a journal, a payment, an invoice, or a statement. Both tools are marked read-only and not destructive.

**The script is returned in full.** Your security team can read it before allowing it to run. It is Python and needs `numpy`. Do not shorten it or rewrite it.

<!-- TODO Saifs AI: confirm whether the local script makes network calls. The tool says the financial data never leaves the user's machine on that route. This page does not claim the script itself makes no network calls. -->

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
| Customer ledger rows stored by the hosted tool | Not confirmed here. The watch snapshot is not stored on the server. The local engine keeps rows in your sandbox. |
| Passwords held | None. Sign-in is Microsoft single sign-on. |
| Write access to your systems | None |
| Local engine | The script is returned in full for review before it runs. It needs `numpy`. |

**Independent audits.** Saifs AI does not yet hold an independently audited certificate such as SOC 2 or ISO 27001. If your review needs a completed security questionnaire, request it through the [contact page](https://saifs.ai/contact-us).

## Responsible use

This connector is for fraud detection and anomaly detection as a review list. A person decides what the flag means. The connector does not.

Say "flagged for review." Do not say "fraudulent." A Benford result, a z-score, a duplicate group, or a Beneish M-Score is a signal, not proof. It is not an audit opinion, a regulatory determination, or legal or financial advice.

Do not send emails, phone numbers, addresses, or ID numbers. The tool states that those values must not be in the call. `user` and `approver` can be a name. Send them only when the review needs them. A code or a role is enough when a name is not required.

Do not use the list to accuse an employee, a vendor, or a company in a message that goes out as fact. The list is the start of a human review.

Benford's law does not apply to every column. Assigned numbers, ids, codes, rates, and scores are the wrong input. The tool is built to refuse those. Do not force the test by renaming the column to look like an amount.

The M-Score was not built for banks, insurers, or NBFCs. The default is to skip them. Turning that off does not make the score valid for those businesses. It only stops the skip.

## Token usage and cost

With the hosted tool, the rows are part of the tool call, so token use grows with the file. With the local engine, the agent receives a script and the rows stay in the sandbox, so token use does not grow with the file.

<!-- TODO Saifs AI: measure tokens before publishing figures. Use one named tokenizer on three sizes: a small file, the hosted limit (about 20,000 transactions, 100,000 numbers, or 400 statement rows), and a larger file. Count both directions. Include the script, the write into the sandbox, and the printed decision cards. Record input and output tokens, a dated reference price per million, the first-run and later-run multipliers, and a monthly example. State where the hosted tool uses fewer tokens. Do not copy the Inventory token table. -->

Until those figures are measured, this page does not state a token count or a dollar cost.

**Which route to choose, before the token table exists**

- **Inside the hosted limits, and a single question:** the hosted tool. It is one call.
- **Above about 20,000 transactions, 100,000 numbers, or 400 statement rows:** the local engine. It is the only route.
- **The rows must stay in your environment:** the local engine. Turn the hosted tool off.
- **The same large file, many times in one session:** the local engine, if the sandbox keeps the script.

**Hosts that bill per action.** Some hosts charge a fixed amount per tool call and do not bill by token. On those hosts, a smaller token count may not reduce the bill. The local engine may also use one extra action to run the script. Choose it there for file size and for where the data stays, not for a lower token bill.

## Prerequisites

- A Saifs AI account, created by signing in with Microsoft single sign-on
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Access to Copilot Studio, Microsoft 365 Copilot, Azure AI Foundry, Power Automate, Power Apps, or Azure Logic Apps
- A ledger, payment, journal, or expense export, a column of amounts, or two years of statements
- For the local engine: code execution enabled on the agent host, and `numpy` available there

## How to get credentials

No keys or secrets are needed. When you create a connection, you are redirected to sign in with your Microsoft account and approve access. If the connection later expires, sign in again from the connection page.

## Get started with your connector

1. In Copilot Studio, open your agent and go to **Tools**, then **Add a tool**.
2. Search for **Financial Fraud Detection** and select it.
3. Create a connection and sign in.
4. Add this to the agent's instructions: "For questions about unusual entries, Benford's law, duplicate payments, split invoices, a Beneish M-Score, or a change since the last run, call Financial Fraud Detection. Do not calculate the statistics yourself. Say flagged for review, never fraudulent."
5. Provide the rows by pasting them, uploading them, or pointing the agent at a file. Include `approval_limits` if you want split-invoice checks. Include `watch_snapshot` if you are comparing with the last run.
6. Ask in plain language, for example "Flag unusual journal entries this quarter" or "Find suspicious entries in this expense file."

Treat the result as a review list. A person checks the flagged entries before anyone is accused or any entry is reversed.

## For administrators

Enabling this connector gives the organisation one approved way to decide which entries to review. It does that inside the agents your makers already build.

**Why this is an administrator decision.** Without a calculation tool, makers ask an agent to scan a spreadsheet for fraud. A language model can flag a different row on the next run, and the result cannot be compared month to month. A governed connector uses one method for every agent and team.

- **You choose who can connect it.** Tenant and environment permissions apply. Only users who can create a connection can call the tools.
- **You keep your system of record.** The connector flags and explains. It does not post journals, block payments, or write back to any system.
- **You can check the flag.** The same file, settings, and seed can be run again. A missing input is reported, not filled in with a score.
- **You control where data goes.** Turn off the hosted tool per agent to keep ledger rows in your environment.
- **You set data policy as usual.** Place the connector in the Business, Non-business, or Blocked group with Power Platform data loss prevention policies.
- **There are no secrets to rotate.** Expired connections are renewed by signing in again.

**Review checklist**

- Decide whether the hosted tool is allowed, or local engine only
- Assign the connector to the correct data policy group
- Confirm that makers will not send emails, phone numbers, addresses, or ID numbers
- Review the privacy policy and terms
- Read the self-assessments listed under Security and compliance, once they are published
- Review the local engine script if your policy requires it
- Confirm the hosted-tool retention note in Data handling and security before a wide rollout

## For developers

No field is required. Send the arrays for the checks you want. Other checks run only when their inputs are present. Set `job` to run one check. Omit `job` to run every check the inputs allow.

Fraud detection on this connector is the review list from `financial_anomaly`. It is not a posting to the ledger. For a file over the hosted limits, call `financial_anomaly_get_engine` instead.

### Transaction fields

Up to 20,001 objects on the hosted tool, and transactions plus invoices together stay within 20,000 rows. A header row followed by value rows is accepted.

| Field | Meaning | Needed for |
| --- | --- | --- |
| `entry_id` | Voucher, journal, or payment id | A review list a person can open, and `explain_flag` |
| `date` | Posting or payment date | Duplicates, splits, and the watch comparison |
| `amount` | Line amount, as written | Benford's law, z-scores, duplicates, and limit checks |
| `debit`, `credit` | Debit and credit when there is no single amount | The same checks, from the two columns |
| `vendor`, `vendor_code` | Party name or code | Peer groups, duplicates, and split clusters |
| `invoice_no` | Invoice or bill number | A stronger duplicate check |
| `account` | GL account, ledger, or expense category | Peer groups by account |
| `user`, `approver` | Who entered it, and who approved it | Context on the review row. Omit when a name is not needed. |
| `department` | Department, cost centre, or branch | Context on the review row |
| `description` | Narration, memo, or remarks | Context on the review row |
| `currency` | Currency code when the amount has no symbol | A label on the row. This page does not claim a conversion. |

Do not put emails, phone numbers, addresses, or ID numbers in any of these fields.

### Invoices, numbers, and statements

| Input | Limit on the hosted tool | Needed for |
| --- | --- | --- |
| `invoices` | Same shape as transactions. Together with transactions, within 20,000 rows. | Duplicate and split checks on the AP file |
| `numbers` | 100,000 values | A Benford's law test on one column. Do not send ids, invoice numbers, phone numbers, or scores. |
| `field_name` | One string | Names the numbers column so the test can be refused |
| `financials` | 400 statement rows, or one object with `current` and `prior` | The Beneish M-Score. Needs two years. |
| `item` | An id, an invoice number, `{"row": n}`, or `{"company": name}` | `explain_flag` only |
| `watch_snapshot` | The object from the last run | `watch_transactions`, or a full run that should compare periods |
| `approval_limits` | A list of limits, such as `[10000, 100000]` | Split invoices and amounts just below a limit |

Statement lines the schema names: `company`, `year`, `sales`, `cogs`, `receivables`, `current_assets`, `ppe`, `total_assets`, `depreciation`, `sga`, `current_liabilities`, `long_term_debt`, `net_income`, `cfo`. Sales and total assets are required for both years. Without `cfo`, the 5-variable M-Score is used.

### Options and defaults

| Option | Default | Purpose |
| --- | --- | --- |
| `job` | Omit, and every check the inputs allow will run | One of the six check names |
| `min_value` | 10 | Ignore amounts at or below this in the digit test. Minimum 0. |
| `duplicate_window_days` | 60 | How far apart two payments can be and still count as a possible duplicate. From 0 to 3660. |
| `split_window_days` | 3 | How many days a split-invoice cluster can span. From 0 to 366. |
| `min_duplicate_amount` | 1 | Ignore possible duplicates at or below this amount. Minimum 0. |
| `peer_min` | 15 | Minimum peer payments before an amount is compared with that vendor or account. Minimum 5. |
| `m_score_threshold` | -1.78 | Cutoff for the 8-variable Beneish M-Score. Above this is flagged for review. From -10 to 10. The 5-variable model uses -2.22. |
| `exclude_financial_companies` | true | Skip banks, insurers, and NBFCs |
| `date_order` | Omit, and the export decides | `DMY` or `MDY` for an ambiguous date |
| `mapping` | None | Header to standard field, when a column was mapped wrong |
| `currency_symbol` | Empty. The symbol on the amounts is used. | Symbol on the cards. A rupee symbol uses lakh and crore wording. A dollar sign is also accepted. |
| `as_of` | Today | Analysis date, YYYY-MM-DD |
| `forget` | 1 | How fully the new batch replaces the watch baseline. From 0 to 1. 1 keeps this batch as the next baseline. |
| `snapshot_salt` | Empty | Secret mixed into the snapshot hashes. Reuse it every run. |
| `seed` | 7 | Seed for the sampling checks. Minimum 0. The same inputs and seed give the same result. |
| `max_listed` | 100 | How many review rows to list. From 1 to 100,000. Totals still count every flag. |

<!-- TODO Saifs AI: confirm from a live call whether mixed currencies in one file are kept separate, converted, or refused. The schema describes a symbol and a currency code. It does not describe a conversion. Also confirm the exact gap that counts as "just below" an approval limit. -->

### Sample request

```json
{
  "transactions": [
    {
      "entry_id": "JE-1044",
      "date": "2026-11-12",
      "amount": 49950,
      "vendor": "Northwind Supplies",
      "invoice_no": "INV-8831",
      "account": "Office expenses",
      "description": "November supplies"
    }
  ],
  "approval_limits": [50000, 100000],
  "currency_symbol": "$",
  "as_of": "2026-11-17"
}
```

Every key in this sample is a key on `financial_anomaly`. `job` is omitted, so every check these inputs allow will run. There are no `numbers` and no `financials`, so a numbers-only Benford test and the Beneish M-Score do not run from this sample. One row is not enough for a peer z-score, because `peer_min` defaults to 15. The sample shows the shape of the call, not a finished review list.

### Sample response (shortened)

A signed-in call was not available while this page was written. A call to `https://financial-anomaly-mcp-microsoft.saifs.ai/mcp` without a token returned HTTP 401 on 17 November 2026. No response body is shown here, because a real result was not returned.

<!-- TODO Saifs AI: replace this note with a shortened real response from financial_anomaly. Include one item flagged for review, the Benford result or the refusal, and the field that says why. Do not invent field names. Do not paste a response from another product. -->

The tool states what a result contains. The local engine prints decision cards and writes the full set to `financial_anomaly_result.json`, plus `watch_snapshot.json`.

### Response sections

| What comes back | Use it for |
| --- | --- |
| Decision cards | The plain-language actions to read first. They say flagged for review, not fraudulent. |
| Benford digit tests | Observed digit frequencies against Benford's law, using chi-square and MAD, or a refusal when the column does not qualify |
| Entries to review | The rows a person opens. `max_listed` caps the list. The totals still count every flag. |
| Duplicate groups | Possible duplicate payments inside the day window |
| Split-invoice and just-below-limit rows | Present when `approval_limits` was sent. Absent as a check when it was not. |
| Beneish M-Score | The score for each company-year that qualified, against -1.78 or -2.22 |
| Updated watch snapshot | Counts and salted hashes to store and send on the next call |
| `financial_anomaly_result.json` | The full local-engine result, written in the sandbox |
| `watch_snapshot.json` | The local-engine snapshot, written in the sandbox |

<!-- TODO Saifs AI: replace the first column with the JSON keys from a live response. -->

## Known issues and limitations

- **Review list only.** It does not post journals, block payments, or update any system.
- **Not an accusation.** A flag is a signal, not proof. It is not an audit opinion, a regulatory determination, or legal or financial advice.
- **The tools run on request.** They do not watch a ledger overnight, and they do not send an alert. Use a scheduled flow, and pass the snapshot back, for a repeated check.
- **Hosted tool size.** About 20,000 transactions, 100,000 numbers, and 400 statement rows. Transactions and invoices together stay within 20,000 rows. Above that, use the local engine.
- **Benford's law has a scope.** Ids, codes, rates, invoice numbers, phone numbers, and scores are refused. Amounts at or below 10 are ignored unless you change `min_value`.
- **Thin peer groups.** A z-score needs at least 15 peer payments by default. Below that, the amount is not compared with the peer group.
- **Duplicates.** Without an invoice number, the duplicate check is weaker. Payments at or below 1 are ignored unless you change `min_duplicate_amount`.
- **Approval limits.** If they are omitted, split-invoice and just-below-limit checks are skipped.
- **M-Score scope.** Two years are required, with sales and total assets in both. Banks, insurers, and NBFCs are skipped by default. Without operating cash flow, the 5-variable score and the -2.22 cutoff are used.
- **The snapshot is yours.** If you omit `watch_snapshot`, this batch is not compared with the last one. A different `snapshot_salt` breaks the comparison.
- **Listed rows.** `max_listed` defaults to 100. Raise it, up to 100,000, when the hosted result must list more rows. The local engine writes the full result to `financial_anomaly_result.json`.
- **Currency.** The symbol labels the cards. A rupee symbol uses lakh and crore wording. This page does not claim a conversion between currencies.
- **Personal data.** Do not send emails, phone numbers, addresses, or ID numbers.
- **Regions.** Not available in US Government (GCC, GCC High, DoD) or China (21Vianet) clouds.

<!-- TODO Saifs AI: rate limits, timeouts, and the service region are not in the tool schema. Add them here when confirmed. -->

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| The tool cannot run, or the call returns unauthorized | The connection is missing or expired. A call with no token returned HTTP 401 on 17 November 2026. | Open the connection and sign in again |
| The Benford test is refused | The column is an id, a code, a rate, an invoice number, a phone number, or a score | Send measured amounts, and set `field_name` to the real column name |
| No split-invoice rows | `approval_limits` was not sent | Send the limits, such as `[10000, 100000]` |
| No peer comparison on an amount | Fewer than `peer_min` payments for that vendor or account. The default is 15. | Send a longer history, or lower `peer_min` no further than 5 if that is a real choice |
| The M-Score is missing | Fewer than two years, sales or total assets missing, or the company is a bank, an insurer, or an NBFC | Send both years with those lines, or read the skip before you turn `exclude_financial_companies` off |
| This month does not compare with last month | `watch_snapshot` was omitted, or `snapshot_salt` changed | Store the snapshot and send the same salt |
| The agent gives a statistic without calling the tool | The instructions do not require the tool | Add the instruction in Get started |
| The hosted call refuses a large file | The file is over about 20,000 transactions, 100,000 numbers, or 400 statement rows | Call `financial_anomaly_get_engine` and run the script unchanged in the sandbox |
| The local engine does not run | Code execution is off, or the script was edited | Enable code execution, save the script exactly as returned, and install `numpy` in that sandbox |

## FAQ

**Do I need a forensic accountant to use this?**
No. A user asks in plain language, and the agent returns a review list. Benford's law, the z-score, the duplicate match, and the Beneish M-Score run inside the tool. A forensic accounting review is still a person's job. The tool prepares the list.

**What is the least data I need?**
Nothing is required to call the tool. An amount, or a debit and a credit, is what the entry checks need. A column of amounts is enough for a Benford's law test. Two years of statements, with sales and total assets, are what the M-Score needs.

**Does my data leave my environment?**
Not with the local engine. The tool states that the data never leaves the user's machine on that route. With the hosted tool, the rows in the call are sent to Saifs AI for the calculation. You can turn the hosted tool off.

**Will it change anything in my systems?**
No. It does not post a journal, block a payment, or write to an ERP. A person acts on the list.

**I already have an ERP MCP server. Why add this?**
That server gives the agent your ERP's records and functions. This connector gives the agent the fraud detection checks, including journal entry testing, and it works on rows from any export.

**Why do I get the same flags every time?**
The checks are fixed formulas, and the sampling checks use a seed. The default seed is 7. The same inputs and the same seed return the same flags. The agent's sentences around them may change.

**What happens if my data is incomplete?**
The tool still runs. It reports what was missing. It skips split checks without approval limits. It skips the M-Score for banks, insurers, and NBFCs. It refuses Benford's law on a column where the test does not apply.

**Can I check how a flag was produced?**
Yes. Ask for `explain_flag` on the entry, the invoice, the row, or the company. The basis is statistical. It is not a conclusion that fraud occurred.

**How large a file can it handle?**
The hosted tool stops at the limits in At a glance. Larger files use the local engine. `max_listed` (default 100) caps how many rows the hosted result lists.

**Can it run on a schedule?**
Yes. The tools run on request. Call the connector from a Power Automate flow with a recurrence trigger, and store the watch snapshot between runs. The connector does not send the alert itself.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment). Microsoft Premium connector licensing applies separately.

**Does a flag mean the entry is fraudulent?**
No. It means the entry is flagged for review. Fraud detection here is the list and the reason. A person decides.

## Support

**Financial Fraud Detection - Saifs AI** is published by Saifs AI.

- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Privacy: [https://saifs.ai/privacy_policy/financial-fraud-detection](https://saifs.ai/privacy_policy/financial-fraud-detection)
- Terms: [https://saifs.ai/eula](https://saifs.ai/eula)
- MCP endpoint: `https://financial-anomaly-mcp-microsoft.saifs.ai/mcp`

<!-- OPEN QUESTIONS
1. Hosted-tool retention, any audit or billing hash, hosting region, rate limits, and timeout. Not stated on the live tool schema. "The server stores nothing" is written on the watch snapshot only.
2. Whether the local engine script makes network calls. The tool states that financial data does not leave the user's machine on that route.
3. A shortened real response, including JSON field names, from a signed-in call. The endpoint returned HTTP 401 without a token on 17 November 2026.
4. Engine version field name, if one exists. Do not copy metadata.model_version from Inventory Intelligence.
5. Numeric MAD and chi-square conformity bands, and the exact gap that counts as just below an approval limit.
6. Whether mixed currencies in one file are kept separate, converted, or refused. No conversion is claimed.
7. Token counts. Not measured. Do not copy another product's table.
8. Product page URL. https://saifs.ai/financial-fraud-detection returned HTTP 404 on 17 November 2026. Privacy https://saifs.ai/privacy_policy/financial-fraud-detection returned HTTP 200.
9. Header aliases beyond the ones named in the tool schema. Only the named aliases are listed.
-->
