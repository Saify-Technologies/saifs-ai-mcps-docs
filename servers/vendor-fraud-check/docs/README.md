# Vendor Bank Fraud Checker - Saifs AI

**Vendor Bank Fraud Checker - Saifs AI** helps accounts payable and treasury teams decide **which vendor bank changes to verify before you pay**. You send a vendor master, a change log, and a payment run. It returns decision cards, a risk score with reasons, identifier checks, shared-account clusters, and a list of what to verify before the money leaves.

It is built for accounts payable, treasury, controllership, internal audit, and shared-service centers. They need the same flags when they run the file again.

**In plain language:** you give it the vendor export you already have, plus the changes and the payment run. Bank account verification here means those checks. It does not call the bank.

**For technical readers:** the checks use check characters, record linkage, a vendor-account graph, robust amount outliers, and CUSUM on change rates.

It flags items for human review. It does not change vendor data, and it does not block, hold, or release a payment. A person verifies by a call-back to a number the company already holds.

**Publisher:** Saifs AI
**Website:** [https://saifs.ai](https://saifs.ai)
**Pricing:** [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
**Contact and help:** [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
**Privacy:** [https://saifs.ai/privacy_policy/vendor-bank-fraud-checker](https://saifs.ai/privacy_policy/vendor-bank-fraud-checker)
**Terms:** [https://saifs.ai/eula](https://saifs.ai/eula)
**Sign-in:** Microsoft single sign-on (OAuth 2.0). No API keys.
**MCP endpoint:** [https://vendor-fraud-check-mcp-microsoft.saifs.ai/mcp](https://vendor-fraud-check-mcp-microsoft.saifs.ai/mcp)

<!-- TODO Saifs AI: publish the product page, then add a Learn more line here and in Support. https://saifs.ai/vendor-bank-fraud-checker and https://saifs.ai/vendor-bank-detail-fraud-checker both returned HTTP 404 on 24 November 2026. Do not link either until one opens. -->

| If you are | Start with |
| --- | --- |
| A business owner, controller, or accounts payable lead | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [Where the data comes from](#where-the-data-comes-from) |
| An administrator or security reviewer | [Data handling and security](#data-handling-and-security), [Security and compliance](#security-and-compliance), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [Where the data comes from](#where-the-data-comes-from), [For developers](#for-developers) |
| A finance or audit reviewer checking the method | [Inside the calculation](#inside-the-calculation), [Responsible use](#responsible-use) |

## The problem it solves

The usual payment fraud is simple. Someone says the vendor bank account has changed, or a vendor master row is edited, and the next payment goes to the wrong place. That pattern is business email compromise. The ERP records the change. It rarely judges it.

Other signs sit in the same files. One bank account is shared by vendors that are not the same supplier. An employee account appears as a vendor account. A GSTIN fails its own check character. A bank change lands just before a large payment. Teams still find these by reading the change log.

Vendor Bank Fraud Checker makes that review available to any AI agent:

- **Which payments to verify before you pay:** a risk score, with reasons, on the payment run.
- **Whether the vendor bank change looks out of pattern:** timing against payments, a first payment to new details, and an unusual amount.
- **Whether the identifiers are well formed:** GSTIN, PAN, and IFSC checks, with no portal lookup.
- **Whether one account is doing too much work:** shared-account clusters, and a match to an employee account hash.
- **Who to call first:** a verify-before-paying list ranked by amount times risk.

## Why choose it: the same flags every time

Vendor Bank Fraud Checker is deterministic. The same rows and the same settings produce the same flags. The checks are fixed methods. They are not written afresh by a language model each time you ask.

That matters because accounts payable releases money. The list has to be the same on Tuesday as it was on Monday, when the file has not changed. The agent's wording around the flags may vary. The flags do not.

Bank account verification on this connector is that repeatable check of your own files. It is not a message to the vendor's bank, and it is not a GST portal lookup.

| What it does | Established method |
| --- | --- |
| Checks GSTIN, PAN, and IFSC structure, including the GSTIN check character and whether the PAN inside a GSTIN matches the vendor PAN | Check characters and published identifier formats |
| Finds the same account across vendors, and a vendor account that matches an employee hash | Record linkage and a vendor-account graph (connected components) |
| Scores a vendor bank change against the payments around it | Change timing, a first payment to the new details, and CUSUM on change rates |
| Flags an amount that sits far from that vendor's usual payments | Robust amount outliers (median absolute deviation) |
| Ranks what to verify first | Amount times risk, down to the level you set |

**It does not guess an owner.** A failed check character is a failed check. The tool does not fill a gap by contacting a bank or a government portal.

**It flags and does not act.** Every result is a signal for a person, not proof of fraud. It never edits the vendor master and never blocks a payment.

## Who should use this

| Role | How it helps |
| --- | --- |
| **Accounts payable leads** | A payment-run check before release, with the reason and the amount on each flag |
| **Treasury teams** | The same bank account verification on the run, so a changed account is a call, not a surprise |
| **Finance controllers and CFOs** | One method in front of every payment run, with a record of what was sent for verification |
| **Internal auditors** | Shared accounts, identifier errors, and change patterns, each with the evidence the tool returns |
| **Shared-service centers** | The same check across entities and a large vendor master, without a different rule for each team |
| **Business owners** | A short list of what to verify before you pay, without a fraud suite to configure |
| **Agent makers and developers** | A tested payment-run check inside an agent, so the agent does not invent the review |

**Best fit:** accounts payable, treasury, internal audit, and shared-service centers at companies that pay suppliers and keep a vendor master. Manufacturers, distributors, retailers, and service firms are the same shape of work if they export those files.

**Not required:** a fraud suite, a data science team, or an ERP. A vendor master is enough to start the identifier checks. A change log and a payment run make the bank account verification sharper.

## Use cases

Each use case below can be built as an agent. The agent types are listed in [Agents you can build](#agents-you-can-build).

**1. Check the payment run**
Scenario: accounts payable is about to release today's payments, and at least one vendor bank change is recent.
Outcome: flags on that payment run, with a reason on each payment and each change.
Ask: "Check today's payment run for risky bank changes."

**2. Validate the vendor master**
Scenario: vendor master data management has left GSTIN, PAN, and IFSC values that were never checked.
Outcome: check-character and format results, including whether the PAN inside a GSTIN matches the vendor PAN.
Ask: "Validate all GSTINs and IFSCs in our vendor master."

**3. Find shared accounts**
Scenario: several vendors might be paying into one account, or a vendor account might be an employee's.
Outcome: shared account clusters, and employee matches when you send employee account hashes.
Ask: "Are any bank accounts shared by more than one vendor?"

**4. Review one vendor bank change**
Scenario: a supplier email says the bank details changed, which is the usual business email compromise pattern.
Outcome: a risk score for that change, with the reasons. The question "was this bank change genuine?" is answered as a flag for a call-back, not as a yes.
Ask: "Was this bank change genuine?"

**5. Explain one flag**
Scenario: a reviewer has the list and needs the evidence before they call the vendor.
Outcome: what changed, when, and the role recorded on the change, for the flag or change id you send.
Ask: "Explain why this payment was flagged."

**6. Build the call list**
Scenario: there is no time to call every vendor before the payment run goes out.
Outcome: a verify-before-paying list ranked by amount times risk, from the level you set.
Ask: "Who should we call before we release this payment run?"

**7. Compare employee account hashes**
Scenario: internal audit wants to know whether a vendor account is also an employee account.
Outcome: matches on hashes. Employee accounts are hashed on arrival and are not returned.
Ask: "Does any vendor account match an employee account hash?"

**8. Record call-backs already done**
Scenario: some flags were verified by phone, and the next run should see that.
Outcome: pass those results in `confirmed_verifications`. The tool does not place the call.
Ask: "Update this run with the call-backs we already completed."

**9. A full vendor file**
Scenario: the file is over 20,001 vendor rows, 20,001 changes, 20,001 run payments, or 100,001 history payments.
Outcome: the same checks run in the agent's sandbox. On that route the rows are not sent to Saifs AI.
Ask: "Check this full vendor master and payment history."

## Agents you can build

Each agent is one agent in your agent host with this connector added as a tool. The last column shows what else to add.

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Payment-run agent | Scores the run and returns the call list | A vendor master, a change log, and the payment run, from [Where the data comes from](#where-the-data-comes-from) | 1, 6 |
| Identifier agent | Checks GSTIN, PAN, and IFSC on the vendor master | The vendor master export | 2 |
| Shared-account agent | Finds one account across vendors, and employee-hash matches | Employee account hashes, if that match should run | 3, 7 |
| Change-review agent | Scores one vendor bank change and explains one flag | `flag_id` when the question is about one flag | 4, 5 |
| Call-back agent | Feeds completed call-backs into the next check | `confirmed_verifications` from the team's own log | 8 |
| Large-file agent | Runs the local engine on a full export | Code execution on the agent host, and `numpy` | 9 |
| ERP payment agent | Reads the vendor master and the run from the ERP, then flags rows for a person | The ERP's own MCP server. This connector does not write the ERP. | 1, 3 |
| Scheduled payment-run agent | Runs the check on a recurrence and sends the list for review | A Power Automate flow with a recurrence trigger, plus email or Teams. Store `rules_snapshot` between runs. | 1, 6 |

The tools run on request. Scheduling comes from the flow, not from the connector. There is no alert inside the server.

## At a glance

| Item | Detail |
| --- | --- |
| Tools | `vendor_fraud_check` (hosted calculation) and `vendor_fraud_check_get_engine` (local engine) |
| Checks inside either tool | Set `job` to one of `check_payment_run`, `explain_flag`, `shared_accounts`, `validate_identifiers`, `vendor_change_risk`, or `verification_list`. Omit `job` to run what the inputs allow. |
| Minimum input | Nothing is required. Send the arrays for the job you want. A vendor master is the start of an identifier check. A payment run needs the payments. |
| Hosted tool size | 20,001 vendor rows, 20,001 change rows, 20,001 payments in the run, 100,001 history payments, 50,001 employee accounts, 50,001 employee account hashes, and 5,000 confirmed verifications. |
| Larger files | Local engine. It runs in your agent's code sandbox and needs only `numpy`. Use it also when bank details must stay in your environment. |
| Writes to your systems | None. Both tools are marked read-only and not destructive. |
| Engine version | Not named in the tool schema. |

<!-- TODO Saifs AI: if a live response includes an engine version field, name it in At a glance. Do not copy metadata.model_version from another product. -->

## Where the data comes from

Vendor Bank Fraud Checker does not connect to your systems. The agent brings the rows to it. Any source that can supply a vendor master, a change log, or a payment run can feed it. The connector needs only rows. It has not been certified against each system named below.

| Data source | How the agent gets the data | Best route |
| --- | --- | --- |
| Spreadsheet or text export from accounting or ERP software (for example Tally, QuickBooks, Zoho, SAP, Oracle, Xero) | The user uploads the file to the agent | Local engine for large files, hosted tool for files inside the limits |
| An ERP that has its own MCP server (for example Dynamics 365 Finance, Business Central, SAP, NetSuite) | The agent reads vendors, changes, and payments with the ERP's tools, then passes the rows on | Hosted tool up to the limits above. Above that, export a file and use the local engine. |
| Dataverse tables | The Dataverse MCP server or the Dataverse connector | Same as above |
| SQL database, Excel workbook online, or SharePoint list | A Power Platform connector on the same agent or flow | Same as above |
| A scheduled flow | A Power Automate flow reads the source on a schedule and calls this connector | Hosted tool, if the file is inside the limits |
| The previous run of this connector | Pass `rules_snapshot` from the last result. The caller stores it. | Either route |
| A penny-drop or call-back the company already did | Pass the account-holder name, or `confirmed_verifications`. This connector does not perform either check. | Either route |

**Works alongside your ERP's MCP server.** That server gives the agent the ERP's records and functions. This connector adds the payment-run check. No fraud module has to be configured. The ERP supplies the rows. Vendor Bank Fraud Checker flags them for review. A person decides what to do. Neither tool writes back.

Vendor master data management stays in your system of record. This connector reads the export. It does not become the vendor master.

**What to pull from the source**

| Data in your system | Field it fills | Why it matters |
| --- | --- | --- |
| Vendor master | `vendor_master` | Vendor code, name, GSTIN, PAN, bank account or its hash, IFSC, email, account-holder name, created date |
| Change log | `change_log` | What changed, the old and new value or hash, when, the role, who changed it, who approved it, the channel, the request email, and whether a call-back was done |
| The payment run | `payments` | Payment reference, vendor, amount, date, and the account on the line if you have it |
| Earlier payments | `payment_history` | Vendor, amount, date, and the account paid, so a new amount or a new account can be compared |
| Employee accounts | `employee_accounts` or `employee_account_hashes` | Hashed on arrival when you send the accounts. Hashes you already made need the same salt. |
| Call-backs already done | `confirmed_verifications` | Flag id, or vendor plus account, with result `verified` or `failed`, the date, and the role |
| Last run | `rules_snapshot` | Account history and verifications from the last call. The caller keeps it. |

**Column names can stay as they are.** The schema does not publish a list of header aliases. Send the export headers. If a header must be forced to a standard field, send `mapping`.

<!-- TODO Saifs AI: if the engine recognises headers such as Vendor Code or Account No without mapping, list them here from the engine script or a signed-in call. Do not copy the Inventory Intelligence header table. -->

**File formats for the local engine.** CSV, TSV, or JSON. The engine reads the files named in the call. Save an Excel sheet as CSV first, or ask the agent to convert it in the sandbox. The script prints the decision cards and writes `vendor_fraud_result.json` and `vendor_fraud_snapshot.json`.

## How it works

The connector offers two routes to the same checks. The agent picks the route by the size of the file, and by whether the bank details may leave your environment.

| | Hosted tool | Local engine |
| --- | --- | --- |
| Tool name | `vendor_fraud_check` | `vendor_fraud_check_get_engine` |
| Use when | Inside the row limits in At a glance | Anything larger, or when bank details must stay in your environment |
| What the agent sends | The rows and the settings | File paths and settings only |
| What comes back | Decision cards, risk scores with reasons, identifier checks, shared-account clusters, a verify-before-paying list, and a snapshot | A ready-to-run Python script |
| Where the calculation runs | Saifs AI endpoint | Your agent's code-execution sandbox |
| Where the vendor and payment rows go | To the Saifs AI endpoint for the calculation | The tool states that no data reaches this server on this path |
| Requires | Nothing extra | An agent host with code execution, and `numpy` |

**Local engine, step by step**

1. The user uploads the file, or points the agent at it.
2. The agent calls `vendor_fraud_check_get_engine` with the file paths and any settings.
3. The connector returns the calculation script. Save it exactly as given. Do not shorten it, retype it, or replace it with other code.
4. The agent runs the script in its sandbox against the files.
5. The script prints the decision cards and writes `vendor_fraud_result.json` and `vendor_fraud_snapshot.json` in the sandbox.
6. The agent reports the flags to the user. Keep the snapshot and pass it on the next run.

### Inside the connector

1. The user asks which payments to verify before you pay, or asks for an identifier check, a shared-account check, or an explanation of one flag.
2. The agent chooses a tool and calls the connector. The call goes to the Saifs AI MCP server over HTTPS at `https://vendor-fraud-check-mcp-microsoft.saifs.ai/mcp`.
3. The server checks the signed-in account.
4. The hosted tool runs the checks on the rows and returns the result. The engine tool returns the same checks as a script, for the agent to run in its sandbox.
5. The agent reads the result and explains it in plain language. The flags come from the engine. The wording comes from the agent.

### Inside the calculation

Both routes run the same checks. A check runs when its inputs are in the call, or when `job` names it. Omit `job` and the tool runs what the inputs allow.

The scoring weights and the day windows are not on this page. They are not in the tool schema. Publishing them would not help a reviewer rerun the check, because the engine does that, and it would describe the rules to anyone reading a public page.

| Step | What the engine does | Method |
| --- | --- | --- |
| 1. Read the export | Accepts vendor, change, payment, and history rows as objects or arrays. Applies `mapping` when a header must be forced to a standard field. `date_order` of `DMY` or `MDY` tells it how to read an ambiguous date. | Stated on the tool |
| 2. Identifier checks | Checks the GSTIN check character, PAN structure, IFSC structure, account-number sanity, and whether the PAN inside a GSTIN matches the vendor PAN. There is no GST portal call. | Check characters and published identifier formats |
| 3. Shared accounts | Builds a vendor-account graph and returns clusters where one account is tied to more than one vendor. Employee accounts are compared as hashes and are not returned. | Record linkage and connected components |
| 4. Change and payment risk | Scores each change and each payment. The tool description names timing against payments, a first payment to new details, a new request email domain when you supply it, robust amount outliers, and CUSUM on how often details change. | Median absolute deviation for unusual amounts, and CUSUM on change rates. Weights are not published. |
| 5. Verification list | Ranks what to verify by amount times risk. `min_level` (default `medium`) is the lowest level placed on the list. | Ranking stated on the tool |
| 6. Explain one item | With `job` set to `explain_flag`, explains the flag id (`F-...`) or change id (`C-...`) you send in `flag_id`. | The check that flagged it |
| 7. Snapshot | Returns account history and verifications for the caller to store as `rules_snapshot` and send on the next run. | Caller-held snapshot |
| 8. Decision cards | Puts the plain-language actions first. `max_listed` (default 200) caps how many rows are listed. | Fixed cards filled with the calculated results |

**Why the answer does not change.** The steps above are fixed checks. There is no language model in the calculation. The hosted tool is marked idempotent. The same inputs return the same flags. The schema does not state a random seed. A new snapshot or a new hash salt is a new input.

**What happens when data is missing.** Nothing is required, so the call still runs. The tool reports what it can score from the arrays you sent. A job that needs a missing array has less to compare. Defaults in the table above are the ones on the schema. They are listed again under [Options and defaults](#options-and-defaults). This page does not add a default the schema does not state.

**Where the working is shown.** Decision cards come first. Each payment and each change has a risk score with reasons. `explain_flag` is the job that returns one flag. This page does not name a JSON field for that reason. See the sample response note in [For developers](#for-developers).

<!-- TODO Saifs AI: confirm from a signed-in call or from the engine script which reasons are returned, and whether a vendor with no history uses a fixed label. Do not publish scoring weights or day windows on this page. -->

## Data handling and security

| Question | Local engine | Hosted tool |
| --- | --- | --- |
| Do vendor rows, bank details, or payments reach Saifs AI? | No. The tool states that no data reaches this server on this path. | Yes. The rows passed in the call are sent for the calculation. |
| Where are they processed? | Your agent host's code sandbox | `vendor-fraud-check-mcp-microsoft.saifs.ai`, over HTTPS |
| What happens to the rows after the call? | They stayed in your sandbox. The script writes `vendor_fraud_result.json` and `vendor_fraud_snapshot.json` there. | Not stated on the tool. See the note below. |
| What about bank account numbers? | Send a hash, or let the local script read the file in your sandbox. Use one `hash_salt` for vendor, payment, and employee data. | The same. Employee accounts are hashed on arrival and are not returned. |
| What about `rules_snapshot`? | The caller keeps `vendor_fraud_snapshot.json`. | The caller is expected to store the snapshot and send it next time. The tool does not say the server keeps it. |
| What can the script do? | It reads the local files and writes the two JSON files above. It needs `numpy`. | Not used on this path. |

<!-- TODO Saifs AI: confirm hosted-tool retention in writing before this sentence is made stronger. Say whether vendor rows, bank details, and payments are discarded after the request, and whether any billing or audit log keeps a hash, the account, or the rows. Also confirm the hosting region, rate limits, and request timeout. Do not copy the audit-hash sentence from the Inventory listing unless this server does the same. The privacy page at https://saifs.ai/privacy_policy/vendor-bank-fraud-checker returned HTTP 200 on 24 November 2026. It does not state how payment-run rows are retained. -->

**What "sandbox" means.** The sandbox is the code-execution environment of your own agent host, such as Copilot Studio or Azure AI Foundry. It belongs to your tenant, not to Saifs AI. Your host's data policies still apply to it.

**Use the local engine only.** If bank details must not leave your environment, turn off the hosted tool. In Copilot Studio, open the agent's **Tools** tab, select the Vendor Bank Fraud Checker server, turn off **Allow all**, and switch off `vendor_fraud_check`. Leave `vendor_fraud_check_get_engine` on. The agent can then use only the local engine. A tool added to the server later stays off until you enable it. This setting is per agent.

**Sign-in.** Users sign in with Microsoft single sign-on. Saifs AI holds only the account details needed for sign-in and billing. No API key is created, shared, or stored.

**No write access.** The connector has no action that creates, changes, or deletes a vendor, a bank account, or a payment. Both tools are marked read-only and not destructive.

**The script is returned in full.** Your security team can read it before allowing it to run. It is Python and needs `numpy`. Do not shorten it or rewrite it.

<!-- TODO Saifs AI: confirm whether the local script makes network calls. The tool says no data reaches this server on that path. This page does not claim the script itself makes no network calls. -->

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
| Customer payment rows stored by the hosted tool | Not confirmed here. The local engine keeps rows in your sandbox. Employee accounts are hashed on arrival and are not returned. |
| Passwords held | None. Sign-in is Microsoft single sign-on. |
| Write access to your systems | None |
| Local engine | The script is returned in full for review before it runs. It needs `numpy`. The tool states that no data reaches the server on that path. |

**Independent audits.** Saifs AI does not yet hold an independently audited certificate such as SOC 2 or ISO 27001. If your review needs a completed security questionnaire, request it through the [contact page](https://saifs.ai/contact-us).

## Responsible use

This connector is for payment-diversion review. A person decides what the flag means. The connector does not.

Say "flagged for review" or "verify before you pay." Do not say "fraudulent," and do not say the payment was stopped. A risk score, a shared-account cluster, or a failed GSTIN check is a signal, not proof. It is not an audit opinion, a regulatory determination, or legal or financial advice.

Bank account verification on this page means checks on the files you send. It does not mean the bank confirmed the account holder. Do not tell a vendor, an employee, or a bank that this tool proved who owns an account.

Send bank account numbers as hashes when you can. Use the same `hash_salt` on vendor rows, payment rows, and employee rows. Employee accounts sent in the clear are hashed on arrival and are not returned. Prefer `employee_account_hashes` when you already have the hashes.

A genuine vendor bank change will sometimes be flagged. That is why the next step is a call-back to a number you already hold, not a number from the email that announced the change. The tool does not place that call, and it does not block the payment.

Reason text is for the people your own access control allows to see the run. Do not paste a flag into a message that accuses a vendor or an employee.

## Token usage and cost

With the hosted tool, the rows are part of the tool call, so token use grows with the file. With the local engine, the agent receives a script and the rows stay in the sandbox, so token use does not grow with the file.

<!-- TODO Saifs AI: measure tokens before publishing figures. Use one named tokenizer on three sizes: a small file, the hosted limit (20,001 vendors, 20,001 changes, 20,001 run payments, or 100,001 history payments), and a larger file. Count both directions. Include the script, the write into the sandbox, and the printed decision cards. Record input and output tokens, a dated reference price per million, the first-run and later-run multipliers, and a monthly example. State where the hosted tool uses fewer tokens. Do not copy the Inventory Intelligence token table. -->

Until those figures are measured, this page does not state a token count or a dollar cost.

**Which route to choose, before the token table exists**

- **Inside the hosted limits, and a single question:** the hosted tool. It is one call.
- **Above 20,001 vendor rows, 20,001 changes, 20,001 run payments, or 100,001 history payments:** the local engine. It is the only route.
- **The bank details must stay in your environment:** the local engine. Turn the hosted tool off.
- **The same large file, many times in one session:** the local engine, if the sandbox keeps the script.

**Hosts that bill per action.** Some hosts charge a fixed amount per tool call and do not bill by token. On those hosts, a smaller token count may not reduce the bill. The local engine may also use one extra action to run the script. Choose it there for file size and for where the data stays, not for a lower token bill.

## Prerequisites

- A Saifs AI account, created by signing in with Microsoft single sign-on
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Access to Copilot Studio, Microsoft 365 Copilot, Azure AI Foundry, Power Automate, Power Apps, or Azure Logic Apps
- A vendor master, a change log, or a payment run you are allowed to process. Hashed bank details are enough for matching.
- For the local engine: code execution enabled on the agent host, and `numpy` in that sandbox

## How to get credentials

No keys or secrets are needed. When you create a connection, you are redirected to sign in with your Microsoft account and approve access. If the connection later expires, sign in again from the connection page.

## Get started with your connector

1. In Copilot Studio, open your agent and go to **Tools**, then **Add a tool**.
2. Search for **Vendor Bank Fraud Checker** and select it.
3. Create a connection and sign in.
4. Add this to the agent's instructions: "For any question about a vendor bank change, a payment run, a GSTIN, an IFSC, or a shared account, call the Vendor Bank Fraud Checker tool. Do not decide the risk yourself."
5. Provide the rows by pasting them, uploading them, or pointing the agent to a file. Include the change log and open payments if you have them. Prefer hashes for bank account numbers.
6. Ask in plain language, for example "Check today's payment run for risky bank changes" or "Are any bank accounts shared by more than one vendor?"

Treat results as a review list. Check a flagged payment by calling a number you already hold before you release it.

## For administrators

Enabling this connector gives the organization one approved way to decide which vendor bank changes to verify before you pay, inside the agents your makers already build.

**Why this is an administrator decision.** Without a calculation tool, makers ask an agent to judge a vendor bank change from a spreadsheet. A language model can describe business email compromise and still give a different view of the same file on each run. A governed connector replaces that with one method for every agent and team. The method flags. It does not pay.

- **You choose who can connect it.** Tenant and environment permissions apply. Only users who can create a connection can call the tools.
- **You keep your system of record.** The connector recommends a review. It does not change vendor data or write back to any system.
- **You can check the flag.** Results can be rerun on the same file. The reasons come back with the score.
- **You control where bank details go.** Turn off the hosted tool per agent to keep those rows in your environment. Ask makers to send hashes.
- **You set data policy as usual.** Place the connector in the Business, Non-business, or Blocked group with Power Platform data loss prevention policies.
- **There are no secrets to rotate.** Expired connections are renewed by signing in again.

**Review checklist**

- Decide whether the hosted tool is allowed, or local engine only
- Assign the connector to the correct data policy group
- Confirm that makers will hash bank accounts where they can, and will use one salt across vendor, payment, and employee files
- Review the privacy policy and terms
- Read the self-assessments listed under Security and compliance, once they are published
- Review the local engine script if your policy requires it
- Confirm the hosted-tool retention note in Data handling and security before a wide rollout

## For developers

No field is required. Send the arrays for the job you want. Set `job` to run one check. Omit `job` to run what the inputs allow.

A flag from this connector is a review item. It is not a block on the payment run. For a file over the hosted limits, or when the rows must stay local, call `vendor_fraud_check_get_engine` instead.

The schema does not fix the column names inside a row. Send the export headers. Use `mapping` when a header must be forced to a standard field.

### Inputs

| Input | Limit on the hosted tool | Needed for |
| --- | --- | --- |
| `vendor_master` | 20,001 rows | Vendor code, name, GSTIN, PAN, bank account or its hash, IFSC, email, penny-drop account-holder name, created date |
| `change_log` | 20,001 rows | Vendor, field, old and new value or hash, changed at, role, changed by, approved by, channel, request email, verified |
| `payments` | 20,001 rows | The payment run: payment reference, vendor, amount, date, account on the line if any |
| `payment_history` | 100,001 rows | Earlier payments: vendor, amount, date, account paid |
| `employee_accounts` | 50,001 rows | Employee bank accounts. Hashed on arrival and not returned. |
| `employee_account_hashes` | 50,001 rows | Hashes made with the same recipe and the same `hash_salt` |
| `confirmed_verifications` | 5,000 objects | Call-back results: flag id, or vendor plus account, with `verified` or `failed`, date, and role |
| `rules_snapshot` | One object | Account history and verifications from the last run |
| `flag_id` | One string | A flag (`F-...`) or a change (`C-...`) to explain |

### Jobs

| Job | What you send | What the tool states you get |
| --- | --- | --- |
| `check_payment_run` | `payments`, and the vendor master and change log when you have them | Risk flags on this run |
| `explain_flag` | `flag_id` | The evidence for that flag or change |
| `shared_accounts` | `vendor_master`, and employee accounts or hashes when you have them | Shared-account clusters and employee matches |
| `validate_identifiers` | `vendor_master` | GSTIN, PAN, IFSC, and account checks |
| `vendor_change_risk` | `change_log`, and `payment_history` when you have it | A risk score on each bank-detail change |
| `verification_list` | The flags the run produced, or the inputs that produce them | What to verify, ranked by amount times risk |

<!-- TODO Saifs AI: confirm the minimum arrays each job actually requires, from a signed-in call. The schema marks no field as required. The job names and the tool description are what this table uses. -->

### Options and defaults

| Option | Default | Purpose |
| --- | --- | --- |
| `job` | Omit, and the tool runs what the inputs allow | One of the six job names |
| `flag_id` | None | The flag or change to explain |
| `as_of` | The latest date in the data | Run date |
| `hash_salt` | Empty string | Salt for hashing account numbers. Use the same value on vendor, payment, and employee data. |
| `min_level` | `medium` | Lowest level placed on the verification list. Also `high` or `low`. |
| `date_order` | Not set on the schema | `DMY` or `MDY` for an ambiguous date |
| `mapping` | None | Header to standard field, when a column must be forced |
| `currency_symbol` | Empty | Symbol on the cards. Display only. No conversion. |
| `max_listed` | 200 | How many rows to list. From 1 to 100,000. |

### Sample request

```json
{
  "job": "check_payment_run",
  "vendor_master": [
    {
      "vendor_code": "V-1001",
      "name": "Northwind Supplies",
      "gstin": "27AAAAA0000A1Z5",
      "pan": "AAAAA0000A",
      "bank_account_hash": "hash-demo-account-1",
      "ifsc": "HDFC0000001",
      "email": "ap@northwind.example",
      "account_holder_name": "NORTHWIND SUPPLIES"
    }
  ],
  "change_log": [
    {
      "vendor": "V-1001",
      "field": "bank_account",
      "old_hash": "hash-demo-account-0",
      "new_hash": "hash-demo-account-1",
      "changed_at": "2026-11-22",
      "changed_by_role": "ap_clerk",
      "request_channel": "email",
      "request_email": "billing@northwind-mail.example",
      "verified": false
    }
  ],
  "payments": [
    {
      "payment_ref": "PAY-9001",
      "vendor": "V-1001",
      "amount": 680000,
      "date": "2026-11-24"
    }
  ],
  "hash_salt": "demo-salt",
  "currency_symbol": "Rs",
  "as_of": "2026-11-24",
  "min_level": "medium"
}
```

`job`, `vendor_master`, `change_log`, `payments`, `hash_salt`, `currency_symbol`, `as_of`, and `min_level` are keys on `vendor_fraud_check`. The names inside each row follow the tool description. The schema does not lock those inner names. `mapping` overrides them. The GSTIN, PAN, IFSC, and hashes above are made up. This sample is the shape of a payment-run check. It is not a result.

### Sample response (shortened)

A signed-in call was not available while this page was written. A call to `https://vendor-fraud-check-mcp-microsoft.saifs.ai/mcp` without a token returned HTTP 401 on 24 November 2026. No response body is shown here, because a real result was not returned.

<!-- TODO Saifs AI: replace this note with a shortened real response from vendor_fraud_check. Include one flag, the risk score, and the field that gives the reason. Do not invent field names. Do not paste a response from another product. -->

The tool states what a result contains. Decision cards come first. Then a risk score with reasons for each payment and each change. Then GSTIN, PAN, IFSC, and account checks. Then shared-account clusters and employee matches. Then a verify-before-paying list ranked by amount times risk, and a snapshot of account history. The local engine prints the decision cards. It writes the full set to `vendor_fraud_result.json` and `vendor_fraud_snapshot.json`.

### Response sections

| What comes back | Use it for |
| --- | --- |
| Decision cards | The plain-language actions to read first. They are flags for review, not a block on payment. |
| Risk score with reasons | One score per payment and per change, and why it was flagged |
| Identifier checks | GSTIN, PAN, IFSC, and account-number results |
| Shared-account clusters | One account across vendors, and employee-hash matches |
| Verify-before-paying list | Who to call first, ranked by amount times risk |
| Account-history snapshot | What to store and send back as `rules_snapshot` |
| `vendor_fraud_result.json` | The full local-engine result, written in the sandbox |
| `vendor_fraud_snapshot.json` | The local-engine snapshot, written in the sandbox |

<!-- TODO Saifs AI: replace the first column with the JSON keys from a live response. -->

## Known issues and limitations

- **Review list only.** It does not change vendor data, block a payment, hold a payment, or update any system.
- **Not proof.** A flag is a signal for a person. It is not an audit opinion, a regulatory determination, or legal or financial advice.
- **Not account ownership.** There is no bank call and no GST portal call. A call-back to a known number does that check.
- **The tools run on request.** They do not watch the vendor master overnight, and they do not send an alert. Use a scheduled flow, and pass `rules_snapshot` back, for a repeated check.
- **Hosted tool size.** 20,001 vendor rows, 20,001 changes, 20,001 run payments, 100,001 history payments, 50,001 employee accounts or hashes, and 5,000 confirmed verifications. Above that, use the local engine.
- **Listed rows.** `max_listed` defaults to 200. Raise it, up to 100,000, when the hosted result must list more rows. The local engine writes the full result to `vendor_fraud_result.json`.
- **Verification level.** `min_level` defaults to `medium`.
- **Hash salt.** `hash_salt` defaults to an empty string. A different salt on the next run will not match the previous hashes.
- **Snapshot.** If `rules_snapshot` is omitted, the next run does not use the account history from the last one. The caller keeps the snapshot.
- **Currency.** The symbol labels the cards. This page does not claim a conversion between currencies.
- **Genuine changes.** A real vendor bank change can be flagged. The response is a call-back, not a block.
- **A clear run is not a promise.** A run with no flags means none were flagged at these settings. It does not mean every payment is safe.
- **Regions.** Not available in US Government (GCC, GCC High, DoD) or China (21Vianet) clouds.

<!-- TODO Saifs AI: rate limits, timeouts, and the service region are not in the tool schema. Add them here when confirmed. -->

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| The tool cannot run, or the call returns unauthorized | The connection is missing or expired. A call with no token returned HTTP 401 on 24 November 2026. | Open the connection and sign in again |
| Employee matches do not line up with a hash file you built | `hash_salt` differs from the salt used to build `employee_account_hashes` | Send the same salt on vendor, payment, and employee data |
| The next run ignores the last account history | `rules_snapshot` was omitted, or it was not the snapshot from the last run | Store the snapshot and send the same one next time |
| A shared-account check has nothing to compare | Only one vendor row was sent, or employee hashes were omitted | Send the vendor master, and the employee hashes if that match should run |
| The agent says a payment is safe without calling the tool | The instructions do not require the tool | Add the instruction in Get started |
| The hosted call refuses a large file | The file is over 20,001 vendors, changes, or run payments, or over 100,001 history payments | Call `vendor_fraud_check_get_engine` and run the script unchanged in the sandbox |
| The local engine does not run | Code execution is off, or the script was edited | Enable code execution, save the script exactly as returned, and install `numpy` in that sandbox |
| Dates land on the wrong day | An ambiguous date was read in the other order | Set `date_order` to `DMY` or `MDY` |

## FAQ

**Do I need a fraud analyst to use this?**
No. A user asks in plain language, and the agent returns a review list. Check characters, record linkage, and CUSUM run inside the tool. The call-back is still a person's job. The tool prepares the list.

**What is the least data I need?**
Nothing is required to call the tool. A vendor master is what the identifier check needs. Add the change log and the payment run when the question is which payments to verify. Vendor master data management stays in your ERP. This tool reads the export.

**Does my data leave my environment?**
Not with the local engine. The tool states that no data reaches the server on that path. With the hosted tool, the rows in the call are sent to Saifs AI for the check. You can turn the hosted tool off.

**Will it change anything in my systems?**
No. It does not edit a vendor, block a payment, or write to an ERP. A person acts on the list.

**I already have an ERP MCP server. Why add this?**
That server gives the agent your ERP's records and functions. This connector gives the agent the bank account verification on those rows, and it works on an export from any source.

**Why do I get the same flags every time?**
The hosted tool is marked idempotent, and the checks are fixed methods. The same inputs return the same flags. The agent's sentences around the flags may change.

**What happens if my data is incomplete?**
The tool still runs on what you sent. A job that needs a missing array has less to compare. History from the last run applies only when you send `rules_snapshot`. The page does not add a default the schema does not state.

**Can I check how a flag was produced?**
Yes. Ask for `explain_flag` with the `flag_id`. The result is the evidence for that flag. It is not a conclusion that the payment was diverted.

**How large a file can it handle?**
The hosted tool stops at the limits in At a glance. Larger files use the local engine. `max_listed` (default 200) caps how many rows the hosted result lists.

**Can it run on a schedule?**
Yes. The tools run on request. Call the connector from a Power Automate flow with a recurrence trigger, and store `rules_snapshot` between runs. The connector does not send the alert itself.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment). Microsoft Premium connector licensing applies separately.

**Does a flag mean the payment is fraudulent?**
No. It means the payment is flagged for review. Was this bank change genuine is the question the list is for. A person calls a known number and decides. The tool does not block the payment.

**Does it confirm that the bank account belongs to the vendor?**
No. Bank account verification here checks your files. It does not contact the bank. Confirming the account is the call-back.

## Support

**Vendor Bank Fraud Checker - Saifs AI** is published by Saifs AI.

- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Privacy: [https://saifs.ai/privacy_policy/vendor-bank-fraud-checker](https://saifs.ai/privacy_policy/vendor-bank-fraud-checker)
- Terms: [https://saifs.ai/eula](https://saifs.ai/eula)
- MCP endpoint: `https://vendor-fraud-check-mcp-microsoft.saifs.ai/mcp`

<!-- OPEN QUESTIONS
1. Hosted-tool retention, any audit or billing hash, hosting region, rate limits, and timeout. Not stated on the live tool schema. The privacy page returned HTTP 200 on 24 November 2026 and does not describe payment-row retention.
2. Whether the local engine script makes network calls. The tool states that no data reaches the server on that path.
3. A shortened real response, including JSON field names, from a signed-in call. The endpoint returned HTTP 401 without a token on 24 November 2026.
4. Engine version field name, if one exists. Do not copy metadata.model_version from Inventory Intelligence.
5. Header aliases the engine accepts without mapping.
6. The minimum arrays each job requires. The schema marks no field as required.
7. The label used when a vendor has no payment history. Do not publish scoring weights or day windows.
8. Product page URL. Both candidate URLs returned HTTP 404 on 24 November 2026.
9. Whether rules_snapshot is stored on the server. The tool tells the caller to pass it back. It does not say the server keeps a copy.
-->
