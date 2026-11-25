# Duplicate Invoice Detector - Saifs AI

**Duplicate Invoice Detector - Saifs AI** helps accounts payable teams decide **have we paid this already** before a bill is paid again. You send open and paid invoices, and a vendor master when you have one. It returns likely duplicate pairs and groups, with a score, a reason, and the money at risk.

It is built for accounts payable teams that still catch a second copy of a bill by eye. They need the same flags when they run the file again.

**In plain language:** you give it the bills you are about to pay, and the bills you already hold. It tells you which ones a person should check before money goes out.

**For technical readers:** the match uses Fellegi-Sunter record linkage, edit distance on invoice numbers, and Jaro-Winkler on vendor names.

It flags a bill for review. It does not block or reverse a payment. It is not an audit opinion, and it is not a fraud investigation of a person.

**Publisher:** Saifs AI
**Website:** [https://saifs.ai](https://saifs.ai)
**Pricing:** [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
**Contact and help:** [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
**Privacy:** [https://saifs.ai/privacy_policy/duplicate-invoice-detector](https://saifs.ai/privacy_policy/duplicate-invoice-detector)
**Terms:** [https://saifs.ai/eula](https://saifs.ai/eula)
**Sign-in:** Microsoft single sign-on (OAuth 2.0). No API keys.
**MCP endpoint:** [https://duplicate-payments-mcp-microsoft.saifs.ai/mcp](https://duplicate-payments-mcp-microsoft.saifs.ai/mcp)

<!-- TODO Saifs AI: publish the product page, then add a Learn more line here and in Support. https://saifs.ai/duplicate-invoice-detector returned HTTP 404 on 24 November 2026. Do not link it until it opens. -->

| If you are | Start with |
| --- | --- |
| A business owner, controller, or accounts payable lead | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [Where the data comes from](#where-the-data-comes-from) |
| An administrator or security reviewer | [Data handling and security](#data-handling-and-security), [Security and compliance](#security-and-compliance), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [Where the data comes from](#where-the-data-comes-from), [For developers](#for-developers) |
| A finance or audit reviewer checking the method | [Inside the calculation](#inside-the-calculation), [Responsible use](#responsible-use) |

## The problem it solves

Most accounts payable teams still look for a second copy of a bill by eye. The same invoice can be keyed with a prefix, a slash, or leading zeros. One supplier can sit in the vendor master twice. A scan and an email of the same PDF can both be entered. A reminder can be keyed as a new bill.

An accounts system usually stops an exact duplicate. That is the same vendor and the same invoice number. It misses the rest. The money often leaves before anyone notices. Finding it later is a recovery task, not a payment control.

Duplicate Invoice Detector makes that check available to any AI agent:

- **Have we paid this already:** a verdict on a new bill against bills already entered or paid.
- **What to hold in this payment run:** duplicate invoices in the run, before money goes out.
- **What was paid twice:** a recovery list of paid duplicates, with the evidence for each flag.
- **Which supplier records are the same supplier:** vendor duplicates in the vendor master.
- **Why two bills were flagged:** the score and the reason, on the pair you name.

## Why choose it: the same flags every time

The same invoices and the same settings are the same request. The tool is marked idempotent. The match is a fixed method. It is not written afresh by a language model each time you ask.

That matters because a controller holds a payment on a flag. The flag has to be the same when the file has not changed. The agent's wording around the flag may vary. The flag does not.

| What it does | Established method |
| --- | --- |
| Flags a bill that looks like one already entered or paid | Fellegi-Sunter record linkage |
| Compares invoice numbers that differ by a digit, a prefix, or leading zeros | Edit distance |
| Compares vendor names that are not an exact string match | Jaro-Winkler |
| Keeps a date window and an amount band you can set | Amount and date window. Defaults are on the tool. |
| Updates the weights from pairs a person has confirmed | A caller-held rules snapshot. Learned weights. Counts only. |
| Returns groups, money at risk, a recovery list, and vendor duplicates | The same match, reported for the job you ask for |

**It does not guess.** The call has two thresholds, `flag_probability` (default 0.9) and `review_probability` (default 0.5). A signed-in response was not available, so this page does not state which scores are flagged and which stay on review. Either way, the tool does not block the bill.

**It flags and does not act.** It never blocks a payment, never reverses one, and never writes to an ERP or accounting system.

## Who should use this

| Role | How it helps |
| --- | --- |
| **Accounts payable accountants** | Check a payment run for duplicate bills before the payments go out |
| **Finance controllers** | See the money at risk, and a recovery list of amounts already paid twice |
| **CFOs** | A short list of duplicate payments, with the reason, before cash leaves |
| **Internal auditors** | An accounts payable audit list they can rerun on the same export |
| **Shared-service centres** | One duplicate invoice check across the entities they pay for |
| **CA firms** | The same check on a client's Tally or other accounting export |
| **Business owners** | Which bills to hold, without building the match themselves |
| **Agent makers and developers** | A tested duplicate payment check inside an agent, so the agent does not invent the match |

**Best fit:** accounts payable teams, controllership, internal audit, shared-service centres, and CA firms that pay supplier bills from an export.

**Not required:** a separate audit suite, a data science team, or an extra module in the accounts system. An invoice export is enough to start. A vendor master, payment lines, and earlier bills make the check sharper.

## Use cases

Each use case below can be built as an agent. The agent types are listed in [Agents you can build](#agents-you-can-build). Each one is a `job` on `duplicate_payments`, not a separate tool. Omit `job` and the tool runs what the inputs allow.

**1. Check a payment run before you pay**
Scenario: Friday's run has hundreds of open bills, and a few may already be in the books.
Outcome: a pre-payment verdict on each new bill, with a score and a reason.
Ask: "Check this week's payment run for duplicate bills."

**2. One bill, before it is approved**
Scenario: a single invoice has just arrived from a supplier.
Outcome: a verdict against earlier bills, so a person can answer have we paid this already.
Ask: "Have we paid this already?"

**3. Find duplicate payments in a period**
Scenario: the year-end file holds open and paid bills.
Outcome: duplicate pairs and groups for that period, with the money at risk and the amount already paid twice.
Ask: "Find duplicate payments we made last year."

**4. Recovery list**
Scenario: some duplicates were paid before anyone noticed.
Outcome: a recovery list of those paid duplicates, with the evidence for each flag.
Ask: "Which duplicate payments can we claim back, and what is the evidence?"

**5. Why these two bills matched**
Scenario: a reviewer has a pair id from an earlier run and needs the reason.
Outcome: the score and the fields that made the two invoices look like the same bill.
Ask: "Why do these two invoices look like the same bill?"

**6. Vendor duplicates**
Scenario: the same supplier may have been created twice in the vendor master.
Outcome: vendor-master records that are probably the same supplier.
Ask: "Which vendor records are probably the same supplier?"

**7. Money at risk, by vendor**
Scenario: a controller wants the size of the problem, not only the pairs.
Outcome: a duplicate report of money at risk and amounts paid twice.
Ask: "How much is at risk from duplicate bills, by vendor?"

**8. Learn from confirmed pairs**
Scenario: reviewers have marked pairs as duplicate or not.
Outcome: an updated rules snapshot of learned weights. The snapshot is counts only.
Ask: "Update the duplicate rules from the pairs we confirmed."

**9. A large export**
Scenario: the file is over the hosted limits. That is over 20,000 invoices, 2,000 new bills, 20,000 vendor-master rows, or 50,000 payments.
Outcome: the same check runs in the agent's own sandbox, and the rows stay there.
Ask: "Scan this large invoice export for duplicate payments."

## Agents you can build

Each agent is one agent in your agent host with this connector added as a tool. The last column shows what else to add.

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Pre-payment agent | Checks a new bill, or a payment run, before money goes out | The open bills and the earlier invoice history | 1, 2 |
| Period scan agent | Finds duplicate invoices and duplicate payments in a past period | The open and paid invoice export | 3 |
| Recovery agent | Lists paid duplicates to claim back, with the evidence | Paid invoices, and payment lines when you have them | 4 |
| Pair explanation agent | Explains one flagged pair from its pair id | The pair id from an earlier result | 5 |
| Vendor master agent | Lists vendor duplicates that are probably the same supplier | The vendor master. Hash any bank account before you send it. | 6 |
| Money-at-risk agent | Reports money at risk and amounts already paid twice | The invoice export, grouped in the way you ask | 7 |
| Rules agent | Sends confirmed pairs and returns an updated rules snapshot | The last `rules_snapshot`, and the reviewer verdicts | 8 |
| Large-file agent | Runs the local engine on a full export | Code execution on the agent host, and `numpy` | 9 |
| ERP payment-run agent | Reads bills from the ERP, then flags rows for a person | The ERP's own MCP server. This connector does not write the ERP. | 1, 3 |
| Scheduled check agent | Runs the check on a recurrence and sends the list for review | A Power Automate flow with a recurrence trigger, plus email or Teams. Store `rules_snapshot` between runs. | 1, 3 |

The tools run on request. Scheduling comes from the flow, not from the connector. There is no alert inside the server.

## At a glance

| Item | Detail |
| --- | --- |
| Tools | `duplicate_payments` (hosted check) and `duplicate_payments_get_engine` (local engine) |
| Jobs inside either tool | Set `job` to `check_before_pay`, `duplicate_report`, `explain_pair`, `find_duplicates`, `recovery_list`, or `vendor_duplicates`. Omit `job` to run what the inputs allow. |
| Minimum input | Nothing is required. Send the arrays for the job you want. |
| Hosted tool size | Up to 20,001 invoice rows, 2,000 new bills, 20,001 history rows, 20,001 vendor-master rows, 50,001 payment rows, and 5,000 confirmed pairs |
| Larger files | Local engine. It runs in your agent's code sandbox and needs only `numpy`. Use it over 20,000 invoices, 2,000 new bills, 20,000 vendor-master rows, or 50,000 payments. |
| Writes to your systems | None. Both tools are marked read-only and not destructive. |
| Engine version | Not named in the tool schema. |

<!-- TODO Saifs AI: if a live response includes an engine version field, name it in At a glance. Do not copy metadata.model_version from another product. -->

## Where the data comes from

Duplicate Invoice Detector does not connect to your systems. The agent brings the rows to it. Any source that can supply invoices, a vendor master, or payments can feed it. The connector needs only rows. It has not been certified against each system named below.

| Data source | How the agent gets the data | Best route |
| --- | --- | --- |
| Spreadsheet or text export from accounting software (for example Tally, QuickBooks, Zoho Books, Xero) | The user uploads the file to the agent | Local engine for large files, hosted tool for files inside the limits |
| An ERP that has its own MCP server (for example Dynamics 365, Business Central, SAP, NetSuite) | The agent reads invoices, vendors, and payments with the ERP's tools, then passes the rows on | Hosted tool up to the limits above. Above that, export a file and use the local engine. |
| Dataverse tables | The Dataverse MCP server or the Dataverse connector | Same as above |
| SQL database, Excel workbook online, or SharePoint list | A Power Platform connector on the same agent or flow | Same as above |
| A scheduled flow | A Power Automate flow reads the source on a schedule and calls this connector | Hosted tool, if the file is inside the limits |
| The previous run of this connector | Pass `rules_snapshot` from the last result. The caller stores it. | Either route |
| Reviewer verdicts | Pass `confirmed_pairs` as pair id and whether it is a duplicate | Either route |

**Works alongside your ERP's MCP server.** That server gives the agent the ERP's records and functions. This connector adds the duplicate invoice and duplicate payment check. No audit module has to be configured. The ERP supplies the rows. Duplicate Invoice Detector flags bills for review. A person decides whether to pay, hold, or recover. Neither tool writes back.

**What to pull from the source**

| Data in your system | Field it fills | Why it matters |
| --- | --- | --- |
| Open and paid bills | `invoices` | Vendor, invoice number, date, amount, PO, description, status, and paid date. Credit notes go in as negative rows. |
| A new bill, or the payment run | `invoice` | The bills to check before paying. Up to 2,000 on the hosted tool. |
| Earlier bills | `history` | What the new bills are compared with |
| Vendor master | `vendor_master` | Vendor code, name, GSTIN or PAN, city or pincode, and a hashed bank account |
| Payments against invoices | `payments` | Invoice reference or vendor plus invoice number, amount, and date |
| Last run | `rules_snapshot` | Learned weights from the last run. Counts only. |
| Reviewer decisions | `confirmed_pairs` | `pair_id` and whether the pair is a duplicate. Or the two invoice refs. |
| One pair to explain | `pair_id` | Used with `explain_pair` |

**Column names can stay as exported.** The tool says it accepts ERP and Tally headers as exported. If a header is read as the wrong field, send `mapping` as header to standard field.

<!-- TODO Saifs AI: if the engine maps named headers without renaming, list those headers and the response field that reports the mapping. Until a signed-in call or the engine script shows them, do not publish an alias table. -->

**File formats for the local engine.** CSV, TSV, or JSON. The engine reads the files named in the call. Save an Excel sheet as CSV first, or ask the agent to convert it in the sandbox. The script prints the decision cards and writes `duplicate_payments_result.json` and `rules_snapshot.json`.

## How it works

The connector offers two routes to the same duplicate payment check. The agent picks the route by the size of the file.

| | Hosted tool | Local engine |
| --- | --- | --- |
| Tool name | `duplicate_payments` | `duplicate_payments_get_engine` |
| Use when | Inside the row caps in At a glance | Over those caps, or when the rows must stay in your environment |
| What the agent sends | The rows and the settings | File paths and settings only |
| What comes back | Decision cards, pairs and groups, money at risk, pre-payment verdicts, a recovery list, vendor duplicates, and an updated rules snapshot | A ready-to-run Python script |
| Where the check runs | Saifs AI endpoint | Your agent's code-execution sandbox |
| Where the invoice rows go | To the Saifs AI endpoint for the check | The tool states that no data reaches the server on this path |
| Requires | Nothing extra | An agent host with code execution, and `numpy` |

**Local engine, step by step**

1. The user uploads the file, or points the agent at it.
2. The agent calls `duplicate_payments_get_engine` with the file paths and any settings in `params`.
3. The connector returns the calculation script. Save it exactly as given. Do not shorten it, retype it, or replace it with other code.
4. The agent runs the script in its sandbox against the files.
5. The script prints the decision cards and writes `duplicate_payments_result.json` and `rules_snapshot.json` in the sandbox.
6. The agent reports the flags to the user. Keep `rules_snapshot.json` and pass it on the next run.

### Inside the connector

1. The user asks whether a bill was already paid, or asks for a payment-run check, a recovery list, or vendor duplicates.
2. The agent chooses a tool and calls the connector. The call goes to the Saifs AI MCP server over HTTPS at `https://duplicate-payments-mcp-microsoft.saifs.ai/mcp`.
3. The server checks the signed-in account.
4. The hosted tool runs the check on the rows and returns the result. The engine tool returns the same check as a script, for the agent to run in its sandbox.
5. The agent reads the result and explains it in plain language. The flags come from the engine. The wording comes from the agent.

### Inside the calculation

Both routes run the same check. A job runs when you set `job`, or when the inputs allow it. Omit `job` and the tool runs what the inputs allow. Decision cards come first.

| Step | What the engine does | Method |
| --- | --- | --- |
| 1. Read the export | Accepts ERP and Tally headers as exported. `mapping` overrides a header that was read as the wrong field. `date_order` of `DMY` or `MDY` sets how an ambiguous date is read. | Stated on the tool |
| 2. Take the job | `check_before_pay` uses the new bill and history. `find_duplicates` scans the invoice file. `duplicate_report` reports the money. `recovery_list` lists paid duplicates. `vendor_duplicates` reads the vendor master. `explain_pair` uses `pair_id`. | The `job` value |
| 3. Compare invoice numbers | Treats numbers that differ by a digit, a prefix, a separator, or leading zeros as candidates, not as an automatic match | Edit distance |
| 4. Compare amounts | Uses `amount_tolerance`. The default is 0.005, which is half of one percent. The allowed range is 0 to 0.2. | Amount window |
| 5. Compare dates | Uses `window_days`. The default is 60. The allowed range is 1 to 3660. | Date window |
| 6. Compare vendors | Uses the vendor code, the name, and GSTIN or PAN when you send them. Name matching uses `name_similarity`. The default is 0.93. The allowed range is 0.8 to 0.999. | Jaro-Winkler |
| 7. Score the pair | Scores how strongly the fields point to the same bill. `flag_probability` defaults to 0.9 (range 0.5 to 0.9999). `review_probability` defaults to 0.5 (range 0.01 to 0.99). The exact cut each score uses is not stated on the schema. | Fellegi-Sunter record linkage |
| 8. Learned weights | If you send `rules_snapshot`, the weights from the last run are used. `confirmed_pairs` are reviewer verdicts. The snapshot that comes back is learned weights and counts only. | Caller-held snapshot |
| 9. Report the result | Returns pairs and groups, money at risk, amounts paid twice, pre-payment verdicts, a recovery list with evidence, and vendor duplicates. `max_listed` (default 200) caps how many rows are listed. | Stated on the tool |

**Why the answer does not change.** The tool is marked idempotent. It accepts `seed` (default 7, minimum 0). This page does not describe a random step beyond that seed. The same inputs are the same request. The agent's sentences around the flags may change.

**What happens when data is missing.** No field is required, so the call can still run. The tool runs what the inputs allow. Without `history`, a pre-payment check has no earlier bills to compare. Without `rules_snapshot`, weights learned last time are not applied. Without `vendor_master`, the vendor-duplicate job has no master to read. Defaults in the table above are the ones on the schema.

**Where the working is shown.** Decision cards are first. Each pair carries a score and a reason. `explain_pair` is the job that explains one `pair_id`. This page does not name the JSON keys for those fields. See the sample response note in [For developers](#for-developers).

<!-- TODO Saifs AI: confirm from a signed-in call or the engine script whether recurring bills (rent, retainers) are excluded, whether credit notes are netted before a flag, and whether partial payments are summed per invoice. The schema says to send credit notes as negative rows and to send a payments array. It does not state those three behaviours. Do not claim them until the script shows them. Also confirm what a score below review_probability (default 0.5) does. -->

## Data handling and security

| Question | Local engine | Hosted tool |
| --- | --- | --- |
| Do invoice rows, vendor rows, or payments reach Saifs AI? | No. The tool states that no data reaches the server on this path. | Yes. The rows passed in the call are sent for the check. |
| Where are they processed? | Your agent host's code sandbox | `duplicate-payments-mcp-microsoft.saifs.ai`, over HTTPS |
| What happens to the rows after the call? | They stayed in your sandbox. The script writes `duplicate_payments_result.json` and `rules_snapshot.json` there. | Not stated on the tool. See the note below. |
| What about the rules snapshot? | The caller keeps `rules_snapshot.json`. | The tool describes it as learned weights and counts only. This page does not say the server stores it. |
| What can the script do? | It reads the local files named in the call and writes the two JSON files above. It needs `numpy`. | Not used on this path. |

<!-- TODO Saifs AI: confirm hosted-tool retention in writing before this sentence is made stronger. Say whether invoice rows, vendor rows, and payments are discarded after the request, and whether any billing or audit log keeps a hash, the account, or the rows. Also confirm the hosting region, rate limits, and request timeout. Do not copy the audit-hash sentence from the Inventory Intelligence listing unless this server does the same. The public privacy page does not state invoice-row retention. -->

**What "sandbox" means.** The sandbox is the code-execution environment of your own agent host, such as Copilot Studio or Azure AI Foundry. It belongs to your tenant, not to Saifs AI. Your host's data policies still apply to it.

**Use the local engine only.** If invoice rows must not leave your environment, turn off the hosted tool. In Copilot Studio, open the agent's **Tools** tab, select the Duplicate Invoice Detector server, turn off **Allow all**, and switch off `duplicate_payments`. Leave `duplicate_payments_get_engine` on. The agent can then use only the local engine. A tool added to the server later stays off until you enable it. This setting is per agent.

**Sign-in.** Users sign in with Microsoft single sign-on. Saifs AI holds only the account details needed for sign-in and billing. No API key is created, shared, or stored.

**No write access.** The connector has no action that creates, changes, or deletes an invoice, a payment, or a vendor. Both tools are marked read-only and not destructive. The local engine is marked open-world because it reads files in the caller's sandbox and writes two JSON files there. It still does not write to an ERP.

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
| Customer invoice rows stored by the hosted tool | Not confirmed here. The local engine keeps rows in your sandbox. |
| Passwords held | None. Sign-in is Microsoft single sign-on. |
| Write access to your systems | None |
| Local engine | The script is returned in full for review before it runs. It needs `numpy`. |

**Independent audits.** Saifs AI does not yet hold an independently audited certificate such as SOC 2 or ISO 27001. If your review needs a completed security questionnaire, request it through the [contact page](https://saifs.ai/contact-us).

## Responsible use

This connector is a duplicate invoice and duplicate payment check. It produces a review list. A person decides what the flag means. The connector does not.

Say "flagged for review." Do not say the supplier was paid twice until a person has checked the pair. A score is a signal, not proof. It is not an audit opinion, a regulatory determination, or accounting advice.

Do not use it as a fraud investigation of a person. The tool states that this is out of scope. Do not use it to match payments to bank statements. That is also out of scope.

It does not block a payment, and it does not reverse one. A pre-payment verdict is for the person who releases the run.

Send a bank account only as a hash. The vendor master field is described that way. Vendor names, GSTIN, and PAN can identify a person when the supplier is a proprietor. Send them only when the match needs them.

There is no promise of zero duplicates. A clear file means none were flagged at these settings. It does not mean none exist.

## Token usage and cost

With the hosted tool, the rows are part of the tool call, so token use grows with the file. With the local engine, the agent receives a script and the rows stay in the sandbox, so token use does not grow with the file.

<!-- TODO Saifs AI: measure tokens before publishing figures. Use one named tokenizer on three sizes: a small file, the hosted limit (20,001 invoices, 2,000 new bills, 20,001 vendor-master rows, or 50,001 payments), and a larger file. Count both directions. Include the script, the write into the sandbox, and the printed decision cards. Record input and output tokens, a dated reference price per million, the first-run and later-run multipliers, and a monthly example. State where the hosted tool uses fewer tokens. Do not copy the Inventory Intelligence token table. -->

Until those figures are measured, this page does not state a token count or a dollar cost.

**Which route to choose, before the token table exists**

- **Inside the hosted limits, and a single question:** the hosted tool. It is one call.
- **Over 20,000 invoices, 2,000 new bills, 20,000 vendor-master rows, or 50,000 payments:** the local engine. It is the only route.
- **The rows must stay in your environment:** the local engine. Turn the hosted tool off.
- **The same large file, many times in one session:** the local engine, if the sandbox keeps the script.

**Hosts that bill per action.** Some hosts charge a fixed amount per tool call and do not bill by token. On those hosts, a smaller token count may not reduce the bill. The local engine may also use one extra action to run the script. Choose it there for file size and for where the data stays, not for a lower token bill.

## Prerequisites

- A Saifs AI account, created by signing in with Microsoft single sign-on
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Access to Copilot Studio, Microsoft 365 Copilot, Azure AI Foundry, Power Automate, Power Apps, or Azure Logic Apps
- An invoice export. A vendor master, payment lines, and earlier bills make the check more complete.
- For the local engine: code execution enabled on the agent host, and `numpy` available there

## How to get credentials

No keys or secrets are needed. When you create a connection, you are redirected to sign in with your Microsoft account and approve access. If the connection later expires, sign in again from the connection page.

## Get started with your connector

1. In Copilot Studio, open your agent and go to **Tools**, then **Add a tool**.
2. Search for **Duplicate Invoice Detector** and select it.
3. Create a connection and sign in.
4. Add this to the agent's instructions: "For any question about duplicate invoices, duplicate payments, whether a bill was already paid, a recovery list, or vendor duplicates, call Duplicate Invoice Detector. Do not decide that two bills match yourself. Say flagged for review. Do not block or reverse a payment."
5. Provide the rows by pasting them, uploading them, or pointing the agent at a file. Include earlier bills when you want a pre-payment check. Hash bank accounts before you send a vendor master.
6. Ask in plain language. Two examples are "Check this week's payment run for duplicate bills" and "Have we paid this already?"

Treat the result as a review list. A person checks the flagged bills before a payment is held or a recovery is requested.

## For administrators

Enabling this connector gives the organisation one approved way to decide whether a bill has already been paid. It does that inside the agents your makers already build.

**Why this is an administrator decision.** Without a calculation tool, makers ask an agent to scan a spreadsheet for duplicate payments. A language model can flag a different bill on the next run, and the result cannot be compared week to week. A governed connector uses one method for every agent and team.

- **You choose who can connect it.** Tenant and environment permissions apply. Only users who can create a connection can call the tools.
- **You keep your system of record.** The connector flags and explains. It does not block a payment, reverse a payment, or write back to any system.
- **You can check the flag.** The same file and settings can be run again. The tool is marked idempotent. A missing input limits the job. It is not filled in with a confirmed duplicate.
- **You control where data goes.** Turn off the hosted tool per agent to keep invoice rows in your environment.
- **You set data policy as usual.** Place the connector in the Business, Non-business, or Blocked group with Power Platform data loss prevention policies.
- **There are no secrets to rotate.** Expired connections are renewed by signing in again.

**Review checklist**

- Decide whether the hosted tool is allowed, or local engine only
- Assign the connector to the correct data policy group
- Confirm that makers will hash bank accounts, and will not send a raw account number
- Review the privacy policy and terms
- Read the self-assessments listed under Security and compliance, once they are published
- Review the local engine script if your policy requires it
- Confirm the hosted-tool retention note in Data handling and security before a wide rollout

## For developers

No field is required. Send the arrays for the job you want. Set `job` to run one check. Omit `job` to run what the inputs allow.

Duplicate payment detection on this connector is the review list from `duplicate_payments`. It is not a block on the payment run. For a file over the hosted limits, call `duplicate_payments_get_engine` instead.

### Bills, history, vendors, and payments

The schema does not fix the column names inside a row. Send the export headers. Use `mapping` when a header must be forced to a standard field.

| Input | Limit on the hosted tool | Needed for |
| --- | --- | --- |
| `invoices` | 20,001 rows | Open and paid bills. Include vendor, invoice number, date, amount, PO, description, status (`open` or `paid`), and paid date. Credit notes are negative rows. |
| `invoice` | 2,000 bills | One new bill, or a payment run, to check before paying |
| `history` | 20,001 rows | Earlier invoices to compare with the new bills |
| `vendor_master` | 20,001 rows | Vendor code, name, GSTIN or PAN, city or pincode, and a hashed bank account |
| `payments` | 50,001 rows | Payments against invoices: invoice ref, or vendor plus invoice number, plus amount and date |
| `confirmed_pairs` | 5,000 objects | Reviewer verdicts: `pair_id` and `is_duplicate`, or the two invoice refs |
| `pair_id` | One string | The pair to explain |
| `rules_snapshot` | One object | Learned weights from the last run. Counts only. |

### Jobs

| Job | What you send | What you get |
| --- | --- | --- |
| `check_before_pay` | `invoice`, and `history`. Add `vendor_master` when you have it. | A pre-payment verdict |
| `find_duplicates` | `invoices`. Add `vendor_master` and `rules_snapshot` when you have them. | Pairs and groups for the period |
| `duplicate_report` | The result inputs the report needs, usually the invoice file | Money at risk and amounts paid twice |
| `explain_pair` | `pair_id` | Why that pair was flagged |
| `recovery_list` | Paid duplicates, from `invoices` and `payments` when you have them | A recovery list with evidence |
| `vendor_duplicates` | `vendor_master` | Vendor records that are probably the same supplier |

<!-- TODO Saifs AI: confirm the minimum arrays each job actually requires, from a signed-in call. The schema marks no field as required. The job names and input descriptions are what this table uses. -->

### Options and defaults

| Option | Default | Purpose |
| --- | --- | --- |
| `job` | Omit, and the tool runs what the inputs allow | One of the six job names |
| `window_days` | 60 | How far apart two bills can be and still be compared. From 1 to 3660. |
| `amount_tolerance` | 0.005 | Amount band, as a share of the amount. From 0 to 0.2. 0.005 is half of one percent. |
| `flag_probability` | 0.9 | Score at or above this is flagged. From 0.5 to 0.9999. |
| `review_probability` | 0.5 | Lower edge of the review band. From 0.01 to 0.99. |
| `name_similarity` | 0.93 | How close two vendor names must be. From 0.8 to 0.999. |
| `date_order` | Not set on the schema | `DMY` or `MDY` for an ambiguous date |
| `mapping` | None | Header to standard field, when a column was read as the wrong field |
| `currency_symbol` | Empty. The symbol on the cards follows the currency the data is written in. | A rupee symbol uses lakh and crore grouping. A dollar sign is also accepted. Display only. |
| `as_of` | Not set on the schema | Analysis date |
| `seed` | 7 | Integer seed. Minimum 0. |
| `max_listed` | 200 | How many rows to list. From 1 to 100,000. |

<!-- TODO Saifs AI: confirm from a live call whether mixed currencies in one file are kept separate, converted, or refused. The schema describes a display symbol only. It does not describe a conversion. -->

### Sample request

```json
{
  "job": "check_before_pay",
  "invoice": {
    "Vendor Name": "Northwind Supplies",
    "Invoice No": "INV-0012",
    "Invoice Date": "2026-11-20",
    "Amount": 48000,
    "Status": "open"
  },
  "history": [
    {
      "Vendor Name": "Northwind Supplies",
      "Invoice No": "0012",
      "Invoice Date": "2026-11-17",
      "Amount": 48000,
      "Status": "paid",
      "Paid Date": "2026-11-18"
    }
  ],
  "window_days": 60,
  "currency_symbol": "$",
  "as_of": "2026-11-24"
}
```

`job`, `invoice`, `history`, `window_days`, `currency_symbol`, and `as_of` are keys on `duplicate_payments`. The names inside each bill are export headers. The schema does not fix those inner names. This sample shows the shape of a pre-payment check. It is not a result.

### Sample response (shortened)

A signed-in call was not available while this page was written. A call to `https://duplicate-payments-mcp-microsoft.saifs.ai/mcp` without a token returned HTTP 401 on 24 November 2026. No response body is shown here, because a real result was not returned.

<!-- TODO Saifs AI: replace this note with a shortened real response from duplicate_payments. Include one pair flagged for review, the score, and the field that gives the reason. Do not invent field names. Do not paste a response from another product. -->

The tool states what a result contains. Decision cards come first. Then pairs and groups, with scores and reasons. Then money at risk, amounts paid twice, pre-payment verdicts, a recovery list with evidence, vendor duplicates, and an updated rules snapshot. The local engine prints the decision cards. It writes the full set to `duplicate_payments_result.json` and `rules_snapshot.json`.

### Response sections

| What comes back | Use it for |
| --- | --- |
| Decision cards | The plain-language actions to read first. They are flags for review, not a block on payment. |
| Pairs and groups | The duplicate invoices, with a score and a reason |
| Money at risk and paid twice | How much is still open, and how much has already gone out |
| Pre-payment verdicts | The check on a new bill or a payment run |
| Recovery list | Paid duplicates to claim back, with the evidence |
| Vendor duplicates | Vendor-master records that are probably the same supplier |
| Updated rules snapshot | Learned weights and counts to store and send on the next call |
| `duplicate_payments_result.json` | The full local-engine result, written in the sandbox |
| `rules_snapshot.json` | The local-engine snapshot, written in the sandbox |

<!-- TODO Saifs AI: replace the first column with the JSON keys from a live response. -->

## Known issues and limitations

- **Review list only.** It does not block a payment, reverse a payment, or update any system.
- **Not proof.** A flag is a signal for a person. It is not an audit opinion, a regulatory determination, or accounting advice.
- **Out of scope.** It is not a fraud investigation of a person. It does not match payments to bank statements.
- **The tools run on request.** They do not watch the ledger overnight, and they do not send an alert. Use a scheduled flow, and pass the rules snapshot back, for a repeated check.
- **Hosted tool size.** 20,001 invoice rows, 2,000 new bills, 20,001 history rows, 20,001 vendor-master rows, 50,001 payment rows, and 5,000 confirmed pairs. Above that, use the local engine.
- **Thresholds.** `flag_probability` defaults to 0.9. `review_probability` defaults to 0.5. `amount_tolerance` defaults to 0.005. `window_days` defaults to 60. `name_similarity` defaults to 0.93. The schema does not state the score cut each threshold applies.
- **Learned weights.** If `rules_snapshot` is omitted, the next run does not use weights from the last one. The caller keeps the snapshot.
- **Listed rows.** `max_listed` defaults to 200. Raise it, up to 100,000, when the hosted result must list more rows. The local engine writes the full result to `duplicate_payments_result.json`.
- **Currency.** The symbol labels the cards. A rupee symbol uses lakh and crore grouping. This page does not claim a conversion between currencies.
- **Bank accounts.** Send them only as a hash.
- **No zero-duplicate promise.** A run with no flags means none were flagged at these settings.
- **Regions.** Not available in US Government (GCC, GCC High, DoD) or China (21Vianet) clouds.

<!-- TODO Saifs AI: rate limits, timeouts, and the service region are not in the tool schema. Add them here when confirmed. Also add recurring-bill, credit-note, and partial-payment behaviour once the engine script confirms it. -->

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| The tool cannot run, or the call returns unauthorized | The connection is missing or expired. A call with no token returned HTTP 401 on 24 November 2026. | Open the connection and sign in again |
| The pre-payment check has nothing to compare | `history` was not sent with `invoice` | Send the earlier bills as `history` |
| Learned weights did not apply | `rules_snapshot` was omitted, or it was not the snapshot from the last run | Store the snapshot and send the same one next time |
| A vendor pair looks wrong | The bank account was sent in clear text, or two different suppliers share a short name | Send a hashed bank account, and raise `name_similarity` if 0.93 is too loose for this file |
| The agent says two bills match without calling the tool | The instructions do not require the tool | Add the instruction in Get started |
| The hosted call refuses a large file | The file is over 20,001 invoices, 2,000 new bills, 20,001 vendor rows, or 50,001 payments | Call `duplicate_payments_get_engine` and run the script unchanged in the sandbox |
| The local engine does not run | Code execution is off, or the script was edited | Enable code execution, save the script exactly as returned, and install `numpy` in that sandbox |
| Dates land on the wrong day | An ambiguous date was read in the other order | Set `date_order` to `DMY` or `MDY` |

## FAQ

**Do I need an auditor to use this?**
No. A user asks in plain language, and the agent returns a review list. Fellegi-Sunter record linkage, edit distance, and Jaro-Winkler run inside the tool. An accounts payable audit is still a person's job. The tool prepares the list.

**What is the least data I need?**
Nothing is required to call the tool. A new bill plus earlier bills is what a pre-payment check needs. An invoice export is what a period scan needs. A vendor master is what the vendor-duplicate job needs.

**Does my data leave my environment?**
Not with the local engine. The tool states that no data reaches the server on that path. With the hosted tool, the rows in the call are sent to Saifs AI for the check. You can turn the hosted tool off.

**Will it change anything in my systems?**
No. It does not block a payment, reverse a payment, or write to an ERP. A person acts on the list.

**I already have an ERP MCP server. Why add this?**
That server gives the agent your ERP's records and functions. This connector gives the agent the duplicate invoice check, and it works on rows from any export.

**Why do I get the same flags every time?**
The tool is marked idempotent, and the match is a fixed method. The same inputs return the same flags. `seed` defaults to 7. The agent's sentences around the flags may change.

**What happens if my data is incomplete?**
The tool still runs on what you sent. A job that needs a missing array has nothing to compare. Weights from the last run apply only when you send `rules_snapshot`. The page does not add a default the schema does not state.

**Can I check how a flag was produced?**
Yes. Ask for `explain_pair` with the `pair_id`. The result is a score and a reason. It is not a conclusion that the bill was paid twice.

**How large a file can it handle?**
The hosted tool stops at the limits in At a glance. Larger files use the local engine. `max_listed` (default 200) caps how many rows the hosted result lists.

**Can it run on a schedule?**
Yes. The tools run on request. Call the connector from a Power Automate flow with a recurrence trigger, and store the rules snapshot between runs. The connector does not send the alert itself.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment). Microsoft Premium connector licensing applies separately.

**Does a flag mean we paid the bill twice?**
No. It means the bill is flagged for review. Have we paid this already is the question the list is for. A person decides.

## Support

**Duplicate Invoice Detector - Saifs AI** is published by Saifs AI.

- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Privacy: [https://saifs.ai/privacy_policy/duplicate-invoice-detector](https://saifs.ai/privacy_policy/duplicate-invoice-detector)
- Terms: [https://saifs.ai/eula](https://saifs.ai/eula)
- MCP endpoint: `https://duplicate-payments-mcp-microsoft.saifs.ai/mcp`

<!-- OPEN QUESTIONS
1. Hosted-tool retention, any audit or billing hash, hosting region, rate limits, and timeout. Not stated on the live tool schema. The privacy page at https://saifs.ai/privacy_policy/duplicate-invoice-detector returned HTTP 200 on 24 November 2026 and does not state invoice-row retention.
2. Whether the local engine script makes network calls. The tool states that no data reaches the server on that path.
3. A shortened real response, including JSON field names, from a signed-in call. The endpoint returned HTTP 401 without a token on 24 November 2026.
4. Engine version field name, if one exists. Do not copy metadata.model_version from Inventory Intelligence.
5. Whether recurring bills are excluded, whether credit notes are netted, and whether partial payments are summed. Not stated on the schema beyond negative credit-note rows and a payments array.
6. What a score below review_probability (default 0.5) does.
7. The minimum arrays each job requires. The schema marks nothing as required.
8. Named header aliases, and the response field that reports a mapping. Not listed in the schema.
9. Whether mixed currencies in one file are kept separate, converted, or refused. No conversion is claimed.
10. Token counts. Not measured. Do not copy another product's table.
11. Product page URL. https://saifs.ai/duplicate-invoice-detector returned HTTP 404 on 24 November 2026.
-->
