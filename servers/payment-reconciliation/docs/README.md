# Payment Reconciliation & Match - Saifs AI

**Payment Reconciliation & Match - Saifs AI** helps finance teams **match payments to invoices** and **reconcile the bank**. You send receipts and open invoices. Add bank lines or GST rows when you have them. It returns proposed matches with a confidence and a reason, plus deductions, exceptions, and a report.

It is built for accounts teams that still match receipts, bills, and bank lines in a spreadsheet. They need the same result when they run the file again.

It proposes the match. It does not post it. It is not tax or accounting advice.

**Publisher:** Saifs AI
**Website:** [https://saifs.ai](https://saifs.ai)
**Pricing:** [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
**Contact and help:** [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
**Privacy:** [https://saifs.ai/privacy_policy/payment-reconciliation-match](https://saifs.ai/privacy_policy/payment-reconciliation-match)
**Terms:** [https://saifs.ai/eula](https://saifs.ai/eula)
**Sign-in:** Microsoft single sign-on (OAuth 2.0). No API keys.

<!-- TODO Saifs AI: publish the product page, then add a Learn more line here and in Support. https://saifs.ai/payment-reconciliation-match returned HTTP 404 on 17 November 2026. Do not link it until it opens. -->

| If you are | Start with |
| --- | --- |
| A business owner, controller, or accounts lead | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [Where the data comes from](#where-the-data-comes-from) |
| An administrator or security reviewer | [Data handling and security](#data-handling-and-security), [Security and compliance](#security-and-compliance), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [Where the data comes from](#where-the-data-comes-from), [For developers](#for-developers) |
| A finance or audit reviewer checking the method | [Inside the calculation](#inside-the-calculation) |

## The problem it solves

Most teams still match money to documents by eye. A receipt that covers three invoices, a part payment, or a name that does not match the master takes the time. Over a month, the unmatched items sit until someone clears them by hand.

Exact amount plus exact reference is the match many accounting exports already do. The hard rows are the ones that fail that test. In India, an unmatched supplier invoice on GSTR-2B can also leave input tax credit unclaimed until someone compares the files.

Payment Reconciliation & Match makes that comparison available to any AI agent:

- **Invoice matching for receipts:** which open invoices a receipt clears, including a combination of invoices.
- **Cash application:** the proposed match, the deduction, and the amount still open.
- **Bank reconciliation:** bank lines against the ledger, including a batch or a split.
- **GSTR-2B reconciliation:** portal rows against the purchase register, with input tax credit at risk by supplier.
- **What a person still has to see:** short payments, overpayments, and receipts that match no one.

Accounts receivable automation, for this connector, is that comparison. The combination search and the deduction test are the slow rows. The tool prepares them. A person posts them.

## Why choose it: the same answer every time

Payment matching is deterministic. The same receipts, open items, bank lines, and settings produce the same proposed matches. The numbers come from fixed matching methods. They are not written afresh by a language model each time you ask.

That matters because an accountant posts from the list. The list has to be the same on Tuesday as it was on Monday, when the file has not changed. The agent's wording around the matches may vary. The matches do not.

| What it does | Established method |
| --- | --- |
| Assigns an exact or reference match so one invoice is not used twice | Hungarian assignment |
| Finds a set of invoices that add up to one receipt | Subset-sum over paise |
| Matches a payer name that is not an exact copy of the master | Jaro-Winkler and abbreviation matching |
| Allows a known tax deduction or a small bank charge | Tolerance-window rules |
| Compares GSTR-2B with the purchase register | Amount, date, and GSTIN tolerances |
| Turns confirmed matches into aliases for the next run | A `rules_snapshot` the caller stores and sends back |

**It does not guess.** A short payment is a dispute, not a match. An overpayment or an unidentified receipt is an exception. Each proposed match carries a confidence and a reason.

**It recommends and does not act.** It never posts a receipt, a journal, or a GST figure. It never writes to an ERP, a bank, or the GST portal.

## Who should use this

| Role | How it helps |
| --- | --- |
| **Accounts receivable accountants** at distributors and wholesalers | Daily payment matching of receipts to open invoices, with the reason on each row |
| **Accounts payable accountants** | The same invoice matching for supplier payments and open bills. Set `side` to `payable`. |
| **Finance controllers** | A bank reconciliation and an exception list that can be rerun on the same file at month end |
| **Shared-service centre teams** | One cash application method across entities, instead of a different spreadsheet in each team |
| **CA firm associates** | GSTR-2B reconciliation for client purchase registers, with input tax credit at risk named by supplier |
| **BPO finance-and-accounting teams** | High-volume payment matching, with the rows a person still has to clear called out |
| **Business owners and finance leads** | Unmatched value and the age of exceptions, without reading every narration |
| **Agent makers and developers** | A tested matching calculation, so the agent does not pair rows on its own |

**Best fit:** distributors, wholesalers, CA firms, shared-service centres, and BPO finance teams that match receipts, supplier payments, or bank lines every day. Accounts receivable automation here means the match list. The accountant still posts.

**Not required:** a cash-application module, a data science team, or a connection to the bank. An export of the rows is enough to start.

## Use cases

Each use case below can be built as an agent. The agent types are listed in [Agents you can build](#agents-you-can-build).

**1. Daily cash application**
Scenario: an accounts receivable accountant has today's receipts and the open invoice list.
Outcome: proposed matches, each with a confidence and a reason, and the receipts that still need a person.
Ask: "Match today's receipts to open invoices."

**2. One receipt, several invoices**
Scenario: a customer pays three invoices with one transfer.
Outcome: invoice matching by subset-sum, so the combination that adds up to the receipt is proposed, within the amount tolerance.
Ask: "Which invoices does this payment cover?"

**3. Part payment, TDS, and bank charges**
Scenario: the receipt is short of the invoice. It may be tax deducted at source (TDS), a bank charge, a part payment, or a dispute.
Outcome: a deduction test against the TDS rates and the bank-charge band. A short payment that does not fit is a dispute, not a cleared invoice.
Ask: "This receipt is short of the invoice. Is it TDS, a bank charge, or a dispute?"

**4. The payer name does not match the master**
Scenario: the narration says "SHARMA TRDRS" and the customer master says "Sharma Traders Pvt Ltd".
Outcome: payment matching on party code, on a learned alias, on an invoice number in the narration, or on the name score.
Ask: "Match these payer names to the customer master."

**5. Bank reconciliation**
Scenario: the bank statement and the bank ledger in the books disagree at month end.
Outcome: one-to-one, batch, and split matches, plus charges or interest not in the books, timing items, and the reconciliation statement.
Ask: "Reconcile the bank statement."

**6. GSTR-2B reconciliation**
Scenario: a CA firm compares the supplier GSTR-2B with the client's purchase register.
Outcome: each row is marked matched, mismatched on amount, date, or GSTIN, probable, missing in the books, or missing in 2B. Input tax credit at risk is listed by supplier.
Ask: "Run GSTR-2B vs the purchase register."

**7. The exception queue**
Scenario: some receipts are still open at the end of the day, and finance wants the oldest first.
Outcome: overpayments, unidentified receipts, and disputes, with ageing from `as_of`.
Ask: "Which receipts are still unmatched, and how old are they?"

**8. Supplier payments**
Scenario: accounts payable has a payment file and open bills.
Outcome: the same payment matching as receipts, with `side` set to `payable`.
Ask: "Match these supplier payments to open bills."

**9. A large statement**
Scenario: the file has more than 5,000 receipts or bank lines, or more than 20,000 open items, ledger lines, or GST rows.
Outcome: the same calculation runs in the agent's own sandbox through the local engine, and the financial rows stay there.
Ask: "Reconcile this bank export. It is larger than the hosted limit."

## Agents you can build

Each agent is one agent in your agent host with this connector added as a tool. The last column shows what else to add.

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Cash application agent | Accounts receivable automation for today's receipts, including invoice matching when one receipt covers several invoices. A person posts. | A receipt and invoice file, or a data source from [Where the data comes from](#where-the-data-comes-from) | 1, 2 |
| Deduction agent | Tests TDS and bank charges, and holds a short payment as a dispute | The same data source, plus known TDS rates if they differ from the defaults | 3 |
| Name-match agent | Links a narration to the party master, and keeps aliases you have confirmed | `rules_snapshot` from the previous run | 4 |
| Bank reconciliation agent | Matches the statement to the ledger and returns the reconciliation statement | Bank lines and the bank ledger | 5 |
| GST input-tax agent | Runs GSTR-2B reconciliation and lists input tax credit at risk by supplier | The GSTR-2B export and the purchase register | 6 |
| Exception agent | Lists disputes, overpayments, and unidentified receipts, oldest first | `as_of` set to the report date | 7 |
| Payables agent | Invoice matching for supplier payments and open bills | `side` set to `payable` | 8 |
| Large-file agent | Runs the local engine on a full export | Code execution on the agent host, and `numpy` | 9 |
| ERP reconciliation agent | Reads invoices, receipts, and the bank ledger from the ERP, calculates the matches, and hands them back for a person to post | The ERP's own MCP server | 1, 5, 8 |
| Scheduled reconciliation agent | Runs cash application or a bank reconciliation on a schedule, without anyone asking | A Power Automate flow with a recurrence trigger, and email or Teams | 1, 5 |

The tools run on request. Scheduling comes from the flow, not from the connector.

## At a glance

| Item | Detail |
| --- | --- |
| Tools | `payment_reconciliation` (hosted calculation) and `payment_reconciliation_get_engine` (local engine) |
| Minimum input | No single field is required. A useful call sends one pair: receipts with open items, bank lines with ledger entries, or GSTR-2B rows with the purchase register. |
| Hosted tool size | Up to 5,000 receipts, 5,000 bank lines, and 5,000 confirmed matches. Up to 20,000 open items, ledger entries, GSTR-2B rows, or purchase-register rows. |
| Larger files | Local engine, runs in your agent's code sandbox, needs only `numpy` |
| Writes to your systems | None. Both tools are read-only. |
| Pricing | [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment) |
| Engine version | Not named in the tool schema |

<!-- TODO Saifs AI: if a live response includes an engine version field, name it here. Do not copy metadata.model_version from another product. -->

## Where the data comes from

Payment Reconciliation & Match does not connect to your bank, your GST portal, or your ERP. The agent brings the rows to it.

The connector needs only rows. It has not been certified against Tally, Zoho Books, or any ERP named below. Those names are examples of where the export usually starts.

| Data source | How the agent gets the data | Best route |
| --- | --- | --- |
| A spreadsheet or CSV from Tally, Zoho Books, or another accounting export | The user uploads the file to the agent | Local engine for a large file, hosted tool for a small one |
| A bank statement export | The user uploads the statement. This connector does not sign in to the bank. | Same |
| A GSTR-2B file from the GST portal, plus the purchase register | The user uploads both exports. This connector does not sign in to the GST portal. | Same |
| An ERP that has its own MCP server (for example Dynamics 365 Finance, Business Central, SAP, or NetSuite) | The agent calls the ERP's MCP tools to read invoices, receipts, and the bank ledger, then passes the rows on | Hosted tool up to the limits above. Above that, export a file and use the local engine. |
| Dataverse tables | The Dataverse MCP server or the Dataverse connector | Same as above |
| A SQL database, an Excel workbook online, or a SharePoint list | A Power Platform connector added to the same agent or flow | Same as above |
| A scheduled flow | A Power Automate flow reads the source on a schedule and calls this connector | Hosted tool |
| The previous run of this connector | Pass the returned `rules_snapshot` back in, with any `confirmed_matches` | Either route |

**Works alongside your ERP's MCP server.** An ERP's MCP server gives the agent the ERP's records and functions. This connector adds the matching calculation. It does not need a cash-application module to be configured. The ERP supplies the rows, this connector proposes the matches, and a person posts in the ERP.

**What to pull from the source**

| Data in your system | Field it fills | Why it matters |
| --- | --- | --- |
| Receipts or customer payments | `receipts` | The money in, for cash application |
| Open invoices, bills, or credit notes | `open_items` | The documents a receipt can clear. Credit notes are allowed. |
| The bank statement | `bank_lines` | The bank side of the bank reconciliation |
| The bank account in the books | `ledger_entries` | The book side of the bank reconciliation |
| The GSTR-2B download | `gstr2b_rows` | The portal side of GSTR-2B reconciliation |
| The purchase register | `purchase_register` | The books side of GSTR-2B reconciliation |
| Matches a person has already accepted | `confirmed_matches` and `rules_snapshot` | Applies aliases and tolerances on the next run |

Map the export columns onto those fields. The tool schema does not publish a list of header aliases. This page does not claim that "Invoice Date" or "Voucher No" is recognised on its own.

<!-- TODO Saifs AI: if the engine maps common Tally, Zoho, or bank headers without renaming, document the headers and the field that reports the mapping. Until then, do not claim it. -->

**File formats for the local engine.** CSV, TSV, and JSON, as named on each path argument. Save an Excel sheet as CSV first, or ask the agent to convert it in the sandbox.

## How it works

The connector offers two routes to the same calculation. The agent picks the route by the size of the file.

| | Hosted tool | Local engine |
| --- | --- | --- |
| Tool name | `payment_reconciliation` | `payment_reconciliation_get_engine` |
| Use when | Up to 5,000 receipts or bank lines, and up to 20,000 open items, ledger lines, or GST rows | Anything larger, or when the rows must stay in your environment |
| What the agent sends | The rows and the settings | File paths and settings only |
| What comes back | The matches, exceptions, report, and updated `rules_snapshot` | A ready-to-run Python script |
| Where the calculation runs | Saifs AI endpoint | Your agent's code-execution sandbox |
| Where the financial rows go | To the Saifs AI endpoint for the calculation | Nowhere. The tool states that no financial data reaches the server on this path. |
| Requires | Nothing extra | An agent host with code execution, and `numpy` |

**Local engine, step by step**

1. The user uploads the files, or points the agent at them.
2. The agent calls `payment_reconciliation_get_engine` with the file paths and any settings.
3. The connector returns the calculation script. Save it exactly as given. Do not shorten it or rewrite it.
4. The agent runs the script in its sandbox against the files.
5. The script prints the decision cards and the counts, and writes every match, exception, and the updated rules to `reconciliation.json` in the sandbox.
6. The agent reports the decisions to the user.

### Inside the connector

1. The user asks the agent to match payments to invoices, or to reconcile the bank.
2. The agent chooses a tool and calls the connector's operation, which passes the call to the Saifs AI MCP server over HTTPS at `https://payment-reconciliation-mcp-microsoft.saifs.ai/mcp`.
3. The server checks the signed-in account.
4. The hosted tool runs the engine on the rows and returns the result. The engine tool returns the same engine as a script, for the agent to run in its sandbox.
5. The agent reads the result and explains it in plain language. The matches come from the engine. The wording comes from the agent.

### Inside the calculation

Both routes run the same engine. A layer runs when its rows are in the call. Receipts with open items are cash application. Bank lines with ledger entries are a bank reconciliation. GSTR-2B rows with the purchase register are a GSTR-2B reconciliation. Sending only one pair does not run the other two.

| Step | What the engine does | Method |
| --- | --- | --- |
| 1. Choose the books | Uses `side`. `receivable` matches receipts to open invoices. `payable` matches payments to bills. The default is `receivable`. | The setting on the call |
| 2. Find the party | Tries a party code, then an alias in `rules_snapshot`, then an invoice number in the narration, then the name. | Jaro-Winkler and abbreviation matching |
| 3. One-to-one matches | Settles exact and reference matches so one invoice is not used twice. | Hungarian assignment |
| 4. Combinations | Finds open items that add up to the receipt, within the amount tolerance. The tool states that this step is subset-sum over paise. | Subset-sum |
| 5. Deductions | Tests TDS rates and a bank charge against the gap. | Tolerance-window rules |
| 6. Part payments | A receipt can clear part of an open balance. The rest stays open. | Balance comparison |
| 7. Disputes and exceptions | A short payment is a dispute, not a match. An overpayment or an unidentified receipt is an exception. | Rules stated by the tool |
| 8. Bank statement | Matches lines one-to-one, as a batch, or as a split. Charges and interest that are not in the books, and timing items, go into the reconciliation statement. | Amount match inside the bank date window |
| 9. GSTR-2B | Marks a row matched, mismatched on amount, date, or GSTIN, probable, missing in the books, or missing in 2B. Sums input tax credit at risk by supplier. | Comparison within the GST tolerances |
| 10. Ageing | Reports unmatched items against `as_of`, when you send that date. | Age from the report date |
| 11. Next run | Builds an updated `rules_snapshot` from `confirmed_matches`. Aliases, TDS rates, and tolerances travel in that object. | Returned to the caller, who sends it next time |

**Why the answer does not change.** The steps above are fixed methods. There is no language model in the calculation. The same rows and the same settings return the same matches. A new `rules_snapshot`, or a new `confirmed_matches` list, is a new input. The next result can change with it.

**What happens when data is missing.** Each layer runs from the arrays you send. Defaults below are the ones stated on the tool. They are also listed under [Options and defaults](#options-and-defaults). When the schema states no default, this page does not invent one.

**Each match carries its reason.** The tool returns a confidence and a reason on a proposed match. The reason names how the party was found: code, learned alias, invoice number in the narration, or the name score. This page does not name a JSON field for that reason. See the sample response note in [For developers](#for-developers).

## Data handling and security

| Question | Local engine | Hosted tool |
| --- | --- | --- |
| Do receipts, bank lines, or GST rows reach Saifs AI? | No. The tool states that no financial data reaches the server on this path. | Yes. The rows passed in the call are sent for the calculation. |
| Where are they processed? | Your agent host's code sandbox | `payment-reconciliation-mcp-microsoft.saifs.ai`, over HTTPS |
| What happens to the rows after the call? | They stayed in your sandbox. | See the note below. This page does not state a retention period. |
| What can the script do? | It reads the local files and writes `reconciliation.json`. | Not used on this path. |

<!-- TODO Saifs AI: confirm hosted-tool retention in writing before this sentence is made stronger. Say whether receipt, bank, and GST rows are discarded after the request, and whether any billing or audit log keeps a hash, the account, or the rows. Also confirm the hosting region, rate limits, and request timeout. Do not copy the audit-hash sentence from the Inventory listing unless this server does the same. -->

**What "sandbox" means.** The sandbox is the code-execution environment of your own agent host, such as Copilot Studio or Azure AI Foundry. It belongs to your tenant, not to Saifs AI. Your host's data policies still apply to it.

**Use the local engine only.** If receipts, bank lines, or GST rows must not leave your environment, turn off the hosted tool. In Copilot Studio, open the agent's **Tools** tab, select the Payment Reconciliation & Match server, turn off **Allow all**, and switch off `payment_reconciliation`. Leave `payment_reconciliation_get_engine` on. The agent can then use only the local engine. A tool added to the server later stays off until you enable it. This setting is per agent.

**Sign-in.** Users sign in with Microsoft single sign-on. Saifs AI holds only the account details needed for sign-in and billing. No API key is created, shared, or stored.

**No write access.** The connector has no action that creates, changes, or deletes a receipt, an invoice, a journal, or a GST record. Both tools are marked read-only.

**The script is returned in full.** Your security team can read it before allowing it to run. It is Python and needs `numpy`. Do not shorten it or rewrite it.

<!-- TODO Saifs AI: confirm whether the local script makes network calls. The tool says no financial data reaches the server on that path. This page does not claim the script itself makes no network calls. -->

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
| Customer payment rows stored | Not on the local-engine path. The hosted path sends the rows for the calculation. Retention after the call is the open item in Data handling and security. |
| Passwords held | None. Sign-in is Microsoft single sign-on. |
| Write access to your systems | None |
| Local engine | The script is returned in full for review before it runs. It needs `numpy`. |

**Independent audits.** Saifs AI does not yet hold an independently audited certificate such as SOC 2 or ISO 27001. If your review needs a completed security questionnaire, request it through the [contact page](https://saifs.ai/contact-us).

## Responsible use

This connector proposes matches for receipts, bills, bank lines, and GSTR-2B. A person reviews the list and posts the entries. The connector does not post them.

It is not tax advice, accounting advice, or legal advice. A GSTR-2B result flags input tax credit at risk so a person can look. It is not a GST filing, and it is not a claim. It does not advise on when to pay a bill, or on a settlement discount. The tool states that it is not for invoice creation, payment timing, or tax advice.

Payer names can be personal data, including a proprietor whose name is the business name. Party codes are enough for a code match. Send a code when you have one, and send a name only when the match needs it.

A short payment, an overpayment, or an unidentified receipt is for a person. Do not treat a low confidence as a posted entry.

## Token usage and cost

With the hosted tool, the rows are part of the tool call, so token use grows with the file. With the local engine, the agent receives a script and the rows stay in the sandbox, so token use does not grow with the file.

<!-- TODO Saifs AI: measure tokens before publishing figures. Use one named tokenizer on three sizes: a small file, the hosted limit (5,000 receipts or bank lines, or 20,000 open items, ledger lines, or GST rows), and a larger file. Count both directions. Include the script, the write into the sandbox, and the printed decision cards. Record input and output tokens, a dated reference price per million, the first-run and later-run multipliers, and a monthly example. State where the hosted tool uses fewer tokens. Do not copy the Inventory token table. -->

Until those figures are measured, this page does not state a token count or a dollar cost.

**Which route to choose, before the token table exists**

- **Inside the hosted limits, and a single question:** the hosted tool. It is one call.
- **Above 5,000 receipts or bank lines, or above 20,000 open items, ledger lines, or GST rows:** the local engine. It is the only route.
- **The rows must stay in your environment:** the local engine. Turn the hosted tool off.
- **The same large file, many times in one session:** the local engine, if the sandbox keeps the script.

**Hosts that bill per action.** Some hosts charge a fixed amount per tool call and do not bill by token. On those hosts, a smaller token count may not reduce the bill. The local engine may also use one extra action to run the script. Choose it there for file size and for where the data stays, not for a lower token bill.

## Prerequisites

- A Saifs AI account, created by signing in with Microsoft single sign-on
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Access to Copilot Studio, Microsoft 365 Copilot, Azure AI Foundry, Power Automate, Power Apps, or Azure Logic Apps
- One pair of files to start: receipts and open invoices, a bank statement and the bank ledger, or a GSTR-2B file and the purchase register
- For the local engine: code execution enabled on the agent host, and `numpy` available there

## How to get credentials

No keys or secrets are needed. When you create a connection, you are redirected to sign in with your Microsoft account and approve access. If the connection later expires, sign in again from the connection page.

## Get started with your connector

1. In Copilot Studio, open your agent and go to **Tools**, then **Add a tool**.
2. Search for **Payment Reconciliation & Match** and select it.
3. Create a connection and sign in.
4. Add this to the agent's instructions: "For any question about matching receipts, invoices, bank lines, or GSTR-2B, call the Payment Reconciliation & Match tool. Do not match the rows yourself, and do not post entries."
5. Provide the rows by pasting them, uploading them, or pointing the agent at a file. Include a `rules_snapshot` if you have one from an earlier run.
6. Ask in plain language, for example "Match today's receipts to open invoices" or "Reconcile the bank statement."

Treat the result as decision support. A person checks the proposed matches, the disputes, and the GST gaps before anything is posted.

## For administrators

Enabling this connector gives the organisation one approved way to match payments to invoices and reconcile the bank. It does that inside the agents your makers already build.

**Why this is an administrator decision.** Without a calculation tool, makers ask an agent to pair a spreadsheet of receipts with a spreadsheet of invoices. A language model can pair the same receipt with a different invoice on the next run, and the result cannot be compared day to day. A governed connector uses one method for every agent and team.

- **You choose who can connect it.** Tenant and environment permissions apply. Only users who can create a connection can call the tools.
- **You keep your system of record.** The connector recommends and explains. It does not post receipts, journals, or GST returns, and it does not write back to any system.
- **You can check the match.** The same file and settings can be run again. A short payment is a dispute, not a silent clear.
- **You control where data goes.** Turn off the hosted tool per agent to keep receipts, bank lines, and GST rows in your environment.
- **You set data policy as usual.** Place the connector in the Business, Non-business, or Blocked group with Power Platform data loss prevention policies.
- **There are no secrets to rotate.** Expired connections are renewed by signing in again.

**Review checklist**

- Decide whether the hosted tool is allowed, or local engine only
- Assign the connector to the correct data policy group
- Review the privacy policy and terms
- Read the self-assessments listed under Security and compliance, once they are published
- Review the local engine script if your policy requires it
- Confirm the hosted-tool retention note in Data handling and security before a wide rollout

## For developers

No field is required. Send the pair of arrays for the job you want. Other layers run only when their arrays are present.

Accounts receivable automation on this connector is the match list from `payment_reconciliation`. It is not a post to the ledger. For a file over the hosted limits, call `payment_reconciliation_get_engine` instead.

### Receipts

Up to 5,000 objects on the hosted tool. The tool description names these fields: `id`, `date`, `amount` (or credit and debit), `narration`, payer name, `reference`, and an optional party code.

### Open items

Up to 20,000 objects. The tool description names invoice number, party code, party name, `date`, `amount` or `balance`, and taxable value. A credit note is allowed.

### Bank lines and ledger entries

Up to 5,000 bank lines and 20,000 ledger entries. Bank lines: `date`, `amount` or deposits and withdrawals, `narration`, cheque or UTR. Ledger entries: voucher number, `date`, debit and credit or a signed amount, `narration`, cheque number.

### GSTR-2B and the purchase register

Up to 20,000 rows each. Both use supplier GSTIN, invoice number, `date`, taxable value, IGST, CGST, SGST, cess, and ITC availability. GSTIN is the supplier's GST identification number. GSTR-2B is the purchase statement downloaded from the GST portal. This connector does not download it.

### Confirmed matches and rules

Up to 5,000 confirmed matches. The tool description names payer name or narration, party reference, party name, receipt amount, invoice total, and taxable total. `rules_snapshot` carries aliases, TDS rates, and tolerances from an earlier run. The caller stores it. The next call does not remember it unless you send it.

### Options and defaults

| Option | Default | Purpose |
| --- | --- | --- |
| `side` | `receivable` | `receivable` or `payable` |
| `as_of` | None in the schema | Report date, `YYYY-MM-DD`, used for ageing |
| `currency` | `INR` | Label on amounts. Send one currency. |
| `currency_symbol` | None in the schema | Symbol shown with those amounts, if you set it |
| `amount_tolerance` | 1.00, in the tool description | Rounding gap still treated as a match. Minimum 0. |
| `max_bank_charge` | 500, in the tool description | Largest bank charge tested. Minimum 0. |
| `max_charge_pct` | 0.01 | Largest bank charge as a share of the amount. From 0 to 0.2. |
| `default_tds_rates` | 1%, 2%, and 10%, in the tool description | TDS rates the deduction step tries |
| `date_window_days` | None in the schema | Days allowed between an invoice and a receipt. From 0 to 3,650, if you set it. |
| `max_invoice_age_days` | 730 | Maximum age of an open item, in days. From 1 to 36,500. |
| `max_items` | None in the schema | How many invoices one combination may include. From 1 to 12, if you set it. |
| `max_candidates` | 40 | How many open items are considered for one receipt. From 2 to 60. |
| `name_threshold` | None in the schema | Minimum name score. From 0.5 to 1, if you set it. |
| `auto_threshold` | 0.9 | Confidence value passed to the engine. From 0.5 to 1. |
| `short_payment_pct` | 0.1 | Setting used when a short payment is classified. From 0 to 0.5. |
| `bank_date_window_days` | 7 | Days allowed between a bank line and a ledger entry. From 0 to 365. |
| `bank_closing_balance` | None in the schema | Statement balance, if you want it on the reconciliation |
| `book_closing_balance` | None in the schema | Book balance, if you want it on the reconciliation |
| `gst_amount_tolerance` | 1 | GST amount gap still treated as matched. Minimum 0. |
| `gst_date_tolerance_days` | 0 | GST date gap, in days. From 0 to 365. The default is an exact date. |
| `max_listed` | 2000 | How many rows the result lists. From 1 to 100,000. |

<!-- TODO Saifs AI: confirm the exact effect of auto_threshold, short_payment_pct, max_items, date_window_days, and name_threshold from a live call. The numbers above are the schema defaults and bounds. The cutoff behaviour is not defined further in the schema. Also confirm that currency is a label only. -->

### Sample request

```json
{
  "receipts": [
    {
      "id": "R-1001",
      "date": "2026-11-16",
      "amount": 9800,
      "narration": "NEFT SHARMA TRDRS INV-441",
      "payer_name": "SHARMA TRDRS",
      "reference": "INV-441"
    }
  ],
  "open_items": [
    {
      "invoice_no": "INV-441",
      "party_code": "C-014",
      "party_name": "Sharma Traders Pvt Ltd",
      "date": "2026-11-10",
      "amount": 10000,
      "balance": 10000
    }
  ],
  "side": "receivable",
  "currency": "INR",
  "as_of": "2026-11-17"
}
```

`receipts`, `open_items`, `side`, `currency`, and `as_of` are keys in the tool schema. The fields inside each row are the names in the tool description, written with underscores. This sample asks for cash application only. It does not ask for a bank reconciliation or a GSTR-2B reconciliation, because those arrays are not in the call.

<!-- TODO Saifs AI: confirm the inner key names (payer_name, invoice_no, party_code, and the bank and GST keys) with one signed-in call. The schema allows any properties on a row. -->

### Sample response (shortened)

A signed-in call was not available while this page was written. A call to `https://payment-reconciliation-mcp-microsoft.saifs.ai/mcp` without a token returned HTTP 401. No response body is shown here, because a real result was not returned.

<!-- TODO Saifs AI: replace this note with a shortened real response from payment_reconciliation. Include one short payment or unidentified receipt, and quote the field that says why it was not matched. Do not invent field names. -->

The tool states what a result contains. Each part is returned for the arrays you sent. The local engine writes the full set to `reconciliation.json`.

### Response sections

| What comes back | Use it for |
| --- | --- |
| Proposed matches, each with a confidence and a reason | The list a person reviews before posting. The reason says code, alias, invoice number in the narration, or name score. |
| Combination matches | One receipt cleared against several invoices, from subset-sum |
| TDS and bank-charge deductions | The gap explained as a deduction, when it fits the rates and the charge band |
| Part payments | The amount cleared, and the balance left open |
| Disputes and exceptions | Short payments, overpayments, and unidentified receipts |
| Bank matches and the reconciliation statement | One-to-one, batch, and split matches, charges or interest not in the books, and timing items |
| GSTR-2B comparison | Matched, mismatched, probable, missing in the books, and missing in 2B, with input tax credit at risk by supplier |
| Report with ageing | Matched and unmatched value, aged from `as_of` |
| Updated `rules_snapshot` | Aliases, TDS rates, and tolerances to store and send on the next call |
| Decision cards | The plain-language actions the local engine prints. Read these first. |
| `reconciliation.json` | The full local-engine result, written in the sandbox |

<!-- TODO Saifs AI: replace the first column with the JSON keys from a live response. -->

## Known issues and limitations

- **Decision support only.** It does not post receipts, journals, or GST returns, and it does not update any system.
- **Not advice.** It is not tax, accounting, or legal advice. It does not create invoices, and it does not advise on payment timing or settlement discounts.
- **No bank or GST portal connection.** You upload the statement and the GSTR-2B file. The connector does not sign in to either system.
- **Hosted tool size.** 5,000 receipts, 5,000 bank lines, 5,000 confirmed matches, and 20,000 open items, ledger entries, GSTR-2B rows, or purchase-register rows. Above that, use the local engine.
- **The tools run on request.** They do not watch the bank overnight, and they do not send an alert. Use a scheduled flow for a daily run.
- **Aliases are not remembered on the server.** If you omit `rules_snapshot`, learned payer names from the last run are not applied.
- **Listed rows.** `max_listed` defaults to 2,000. Raise it, up to 100,000, when the hosted result must list more rows. The local engine writes every match to `reconciliation.json`.
- **Combination size.** One combination can include at most 12 invoices. There is no default for `max_items` in the schema.
- **Currency.** The default is INR. Send one currency in a call. This page does not claim a conversion between currencies.
- **GST dates.** `gst_date_tolerance_days` defaults to 0, so the dates must match unless you widen it. `gst_amount_tolerance` defaults to 1.
- **Personal data.** Narrations and payer names can identify a person. Send party codes where you can.
- **Regions.** Not available in US Government (GCC, GCC High, DoD) or China (21Vianet) clouds.

<!-- TODO Saifs AI: rate limits, timeouts, and the service region are not in the tool schema. Add them here when confirmed. -->

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| The tool cannot run, or the call returns unauthorized | The connection is missing or expired. A call with no token returns HTTP 401. | Open the connection and sign in again |
| A receipt you expected to clear is a dispute | The gap did not fit TDS or the bank-charge band, so the short payment was not treated as a match | Check `default_tds_rates`, `max_bank_charge`, and `short_payment_pct`, or leave it for a person |
| A known payer is unidentified | `rules_snapshot` was not sent, or the name score was weak and there was no party code | Send the snapshot from the last run, or send `party_code` on the receipt |
| The bank list repeats an item already matched | The bank lines or the ledger entries were incomplete | Send both sides, and the closing balances if you use them on the statement |
| The agent pairs rows without calling the tool | The instructions do not require the tool | Add the instruction in Get started |
| The hosted call refuses a large file | The file is over 5,000 receipts or bank lines, or over 20,000 open items, ledger lines, or GST rows | Call `payment_reconciliation_get_engine` and run the script in the sandbox |
| The local engine does not run | Code execution is off, or the script was edited | Enable code execution, save the script exactly as returned, and install `numpy` in that sandbox |

## FAQ

**Is this accounts receivable automation?**
Yes, for the match list. It proposes which receipt clears which invoice, including a combination. A person posts the ledger entry.

**Do I need a specialist to use this?**
No. A user asks in plain language, and the agent returns proposed matches. The subset-sum, Hungarian assignment, and Jaro-Winkler steps run inside the tool.

**What is the least data I need?**
One pair. Receipts and open invoices for cash application. Bank lines and ledger entries for a bank reconciliation. GSTR-2B rows and the purchase register for GSTR-2B reconciliation.

**Does my data leave my environment?**
Not with the local engine. With the hosted tool, the rows in the call are sent to Saifs AI for the calculation. You can turn the hosted tool off.

**Will it change anything in my systems?**
No. It does not post, and it does not write to an ERP, a bank, or the GST portal. A person posts.

**I already have an ERP MCP server. Why add this?**
That server gives the agent your ERP's records and functions. This connector gives the agent the payment matching, the bank reconciliation, and the GSTR-2B reconciliation, and it works on rows from any export.

**What happens if my data is incomplete?**
The tool still runs the layers you sent. It applies the defaults in Options and defaults. It does not turn a short payment into a match, and it does not invent a party for an unidentified receipt.

**Can I check how a match was produced?**
Yes. Each proposed match comes back with a confidence and a reason. The reason says whether the party was found by code, by alias, by an invoice number in the narration, or by the name score.

**How large a file can it handle?**
The hosted tool stops at the limits in At a glance. Larger files use the local engine. `max_listed` (default 2,000) caps how many rows the hosted result lists.

**Can it run on a schedule?**
Yes. The tools run on request. Call the connector from a Power Automate flow with a recurrence trigger.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment). Microsoft Premium connector licensing applies separately.

## Support

**Payment Reconciliation & Match - Saifs AI** is published by Saifs AI.

- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Privacy: [https://saifs.ai/privacy_policy/payment-reconciliation-match](https://saifs.ai/privacy_policy/payment-reconciliation-match)
- Terms: [https://saifs.ai/eula](https://saifs.ai/eula)

<!-- OPEN QUESTIONS
1. Hosted-tool retention, any audit or billing hash, hosting region, rate limits, and timeout. Not stated on the live tool schema.
2. Whether the local engine script makes network calls. The tool only states that financial data does not reach the server on that path.
3. A shortened real response, including JSON field names, from a signed-in call. The endpoint returned HTTP 401 without a token on 17 November 2026.
4. Inner row key names (payer_name, invoice_no, and the bank and GST keys).
5. The exact effect of auto_threshold, short_payment_pct, max_items, date_window_days, and name_threshold.
6. Whether currency is a label only, with no conversion.
7. Token counts. Not measured. Do not copy another product's table.
8. Engine version field name, if one exists.
9. Header aliases for Tally, Zoho, and bank exports, if the engine has them.
10. Product page URL. https://saifs.ai/payment-reconciliation-match returned HTTP 404 on 17 November 2026.
11. Confirm the Key Vault reference id shared_paymentreconciliation matches the secret registered for this connector.
-->
