# Data Validation for AI Agents - Saifs AI

**Data Validation for AI Agents - Saifs AI** helps data, finance, and operations teams decide **whether this data is ready for the agent**. You send the export as it is. It returns clean rows, a change log, a quarantine list, and an agent-ready score before and after cleaning.

It is built for agent makers and operations teams. They load messy files from accounting, CRM, or commerce software. They need one repeatable cleaning step before an agent acts.

**Publisher:** Saifs AI
**Website:** [https://saifs.ai](https://saifs.ai)
**Learn more:** [https://saifs.ai](https://saifs.ai)
**Pricing:** [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
**Contact and help:** [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
**Privacy:** [https://saifs.ai/privacy_policy/data-validation](https://saifs.ai/privacy_policy/data-validation)
**Terms:** [https://saifs.ai/eula](https://saifs.ai/eula)
**Sign-in:** Microsoft single sign-on (OAuth 2.0). No API keys.

<!-- TODO Saifs AI: publish a product page and replace the Learn more link. https://saifs.ai/data-validation returned 404 on 18 November 2026. -->

| If you are | Start with |
| --- | --- |
| A business owner, finance or operations lead | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [Where the data comes from](#where-the-data-comes-from) |
| An administrator or security reviewer | [Data handling and security](#data-handling-and-security), [Security and compliance](#security-and-compliance), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [Where the data comes from](#where-the-data-comes-from), [For developers](#for-developers) |
| A data owner checking a repair | [Inside the calculation](#inside-the-calculation), [For developers](#for-developers) |

## The problem it solves

Most teams still clean an export by hand, or they ask an agent to tidy the sheet in prose. Headers differ by source. Dates and amounts arrive in several shapes.

Quantity times price does not match the amount. The same customer appears twice. The next run can change a different cell, and the agent then acts on that file.

Data cleaning before the agent acts is the point of this connector:

- **What is wrong:** data validation of dates, amounts, phones, check digits, rules, and duplicates.
- **What can be repaired:** a cleaned copy, with each edit in the change log, and uncertain rows held back.
- **Whether this data is ready for the agent:** an agent-ready score before and after cleaning.
- **What changed, and why:** the change log names the row, the field, the old value, the new value, and the reason.
- **How this load differs from the last:** data drift detection when you send the previous cleaning profile.

## Why choose it: the same answer every time

Data validation here is repeatable. The same rows and the same settings produce the same clean copy, the same change log, and the same agent-ready score.

The numbers are calculated. They are not rewritten by a language model on each ask.

That matters because a later agent, a finance lead, and an auditor all act on the file. They need a result they can rerun and compare.

The agent's wording around the result may vary. The cleaned values do not.

Teams also ask whether the export is AI-ready data. The agent-ready score is the data quality reading for this file. The score covers only the rows in that call.

| What it does | Established method |
| --- | --- |
| Maps messy headers to fields | Match score against names and value patterns. A match is accepted at or above 0.85. The match uses Jaro-Winkler. |
| Repairs rows that break a business rule | Fellegi-Holt minimum-change repair. The fewest, least-trusted fields are changed. |
| Fills blanks, and stops when a column is too sparse | Hot-deck, or the EM method. Filling stops above 40% missing. |
| Flags unusual amounts without deleting them | A robust z-score from the median absolute deviation (MAD), default 3.5, plus a Benford test |
| Groups possible duplicates and does not merge them | Fellegi-Sunter scoring. Default match threshold 0.9. |
| Rule discovery for rules the file already implies | Functional dependencies. Default confidence 0.95 and support 20. Not applied unless you ask. |

**It does not guess.** If more than one repair would pass, the row goes to quarantine with the candidates. Outliers stay in the file as flags. Duplicate groups are not merged. Rules found in the data are suggestions unless `apply_discovered_rules` is true.

**It does not change your systems.** You receive a cleaned copy and a change log. The source file is not modified. Nothing is written to an ERP, a CRM, or Dataverse. A person approves the copy before another agent uses it. This is operational data cleaning, not a legal, tax, or audit opinion.

## Who should use this

| Role | How it helps |
| --- | --- |
| **Agent makers and developers** | One data cleaning call before a later tool, so the agent does not invent its own fixes |
| **Data and analytics engineers** | Data preprocessing of an export: types, patterns, data quality issues, and a change log |
| **RevOps and CRM admins** | Checks a CRM export, including deduplication as groups for review, before an agent updates a process |
| **Finance operations teams** | Repairs invoice lines where the amount breaks quantity times price, and shows each edit |
| **Consultants onboarding client files** | Reuses a cleaning profile so the next month's file is cleaned the same way |
| **Business owners and operations leads** | A plain score and a short list of rows that still need a person |
| **Administrators** | One approved data validation method, with a switch that keeps large files in the tenant sandbox |

**Best fit:** teams that regularly load exports from Tally, Zoho, QuickBooks, SAP, Shopify, HubSpot, or Excel into an agent.

**Not required:** a data quality platform, a data science team, or an ETL project. A file of rows is enough to start. Rules, a target schema, and a prior profile make the answer sharper.

## Use cases

Each use case below can be built as an agent. The agent types are listed in [Agents you can build](#agents-you-can-build).

**1. Clean an export before the agent acts**
Scenario: an agent builder has a Tally or Excel export with mixed headers, dates, and amounts.
Outcome: clean rows, a change log, and an agent-ready score, so the next step uses one copy.
Ask: "Clean up this Excel export before you rank my customers."

**2. Decide if a CRM file can be used**
Scenario: a RevOps lead wants a check before any agent writes a follow-up from the file.
Outcome: the agent-ready score before and after, plus the rows still in quarantine.
Ask: "Check this CRM file. Is it ready for the agent?"

**3. Repair amount, quantity, and price**
Scenario: finance operations has invoices where the typed amount does not equal quantity times price.
Outcome: the least-trusted fields change so the rule can pass, and every edit is logged.
Ask: "Fix rows where amount does not equal quantity times price, and show the change log."

**4. Map headers onto your own fields**
Scenario: each source names the customer and the amount differently.
Outcome: a column mapping, and candidates where the score is below the threshold.
Ask: "Map these headers onto our customer fields."

**5. Find duplicates without merging**
Scenario: the same party appears twice, with a small difference in the name.
Outcome: duplicate groups for a person to review. No row is merged or deleted.
Ask: "Find duplicate customers. Do not merge them."

**6. Check digits and odd amounts**
Scenario: a GSTIN, a barcode, or an amount looks one step off.
Outcome: check-digit failures and outlier flags. Rows stay in the file.
Ask: "Run GSTIN validation and show amounts that look unusual."

**7. Fill blanks, with a range**
Scenario: a numeric column has gaps, but most of the column is present.
Outcome: filled cells marked as imputed, with a range, or a refusal when the gap is too large.
Ask: "Fill the missing amounts and show the range. Do not guess the rest."

**8. Compare this load with the last**
Scenario: a consultant cleans a client's file every month and still has last month's profile.
Outcome: the same mapping again, plus drift against the last load.
Ask: "Compare this month's Tally export with last month's cleaning profile."

**9. Clean a large file in the sandbox**
Scenario: the export is above about 20,000 rows or about 2,000,000 cells.
Outcome: the same engine runs in the agent's sandbox. The file stays there.
Ask: "Clean this large export in the sandbox, and do not change the original file."

## Agents you can build

Each agent is one agent in your agent host with this connector added as a tool. The last column shows what else to add.

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Export cleaning agent | Returns a clean copy, a change log, and a score for a file a person uploaded | A file upload, or a data source from [Where the data comes from](#where-the-data-comes-from) | 1, 3 |
| Readiness agent | Answers whether the file can feed a later step, and lists what is still held back | The same data source | 2 |
| Header mapping agent | Maps source headers onto a target schema and shows low-confidence matches | The target field list | 4 |
| Duplicate review agent | Returns duplicate groups and waits for a person before anything is merged | A review step in chat or a task list | 5 |
| Identifier and outlier agent | Flags failed check digits and unusual amounts, including GSTIN validation | The same data source | 6 |
| Missing-value agent | Fills sparse blanks and reports the range, or refuses when the column is too empty | Optional history rows | 7 |
| Load-to-load agent | Reuses the cleaning profile and reports drift against the last load | The saved `cleaning_profile` from the previous run | 8 |
| Large-file agent | Runs the local engine on a full export | Code execution on the agent host | 9 |
| System-of-record agent | Reads rows from the system of record, cleans them, and hands the copy back for approval | That system's own MCP server | 1, 8 |
| Scheduled cleaning agent | Cleans a recurring export without anyone asking in chat | A flow with a recurrence trigger, and email or Teams for the log | 1, 8 |

The tools run on request. Scheduling comes from the flow, not from the connector.

A later Saifs AI tool can take the clean rows. This connector does not call that tool itself.

## At a glance

| Item | Detail |
| --- | --- |
| Tools | `data_cleaning` (hosted) and `data_cleaning_get_engine` (local engine) |
| Minimum input | No column is required. Send the rows you have. |
| Hosted tool size | Up to about 20,000 rows and about 2,000,000 cells. The schema allows 20,001 entries in `rows`, so a header row plus about 20,000 data rows. |
| Larger files | Local engine, in your agent's code sandbox. It needs only `numpy`. |
| Default job | `clean_dataset`, which runs the full data cleaning pipeline |
| Writes to your systems | None. The source file is not modified. |
| Idempotent | Both tools are marked idempotent. The same input and settings are meant to return the same result. |

<!-- TODO Saifs AI: confirm the response field that carries the engine version, if one is returned. Do not document a field name until a live response shows it. -->

## Where the data comes from

Data Validation for AI Agents does not connect to your systems. The agent brings the rows. Data cleaning needs only those rows, plus any rules or profile you choose to send.

| Data source | How the agent gets the data | Best route |
| --- | --- | --- |
| Spreadsheet or CSV export from accounting, CRM, commerce, or point-of-sale software (for example Tally, Zoho, QuickBooks, SAP, Shopify, HubSpot, Excel) | The user uploads the file, or pastes the rows | Local engine for large files, hosted tool for small ones |
| A system that has its own MCP server (for example Dynamics 365, Business Central, Dataverse, SAP, or a CRM) | The agent reads records with that server, then passes the rows here | Hosted tool up to the size limit. Above it, export a file and use the local engine. |
| SQL, an online workbook, or a SharePoint list | A connector on the same agent or flow reads the rows | Same size rule as above |
| A scheduled flow | A flow reads the source on a schedule and calls this connector | Hosted tool, if the extract is within the size limit |
| A previous run of this connector | Pass `cleaning_profile`, and `history` when you have earlier clean rows | Either route |
| Another Saifs AI tool that should run on clean data | Run this connector first. Pass its clean rows into the later tool. | Either route |

The connector has not been certified against each source system. It needs rows, not a direct integration.

**Works alongside your system's MCP server.** That server gives the agent the system's records and functions. This connector adds data validation and data cleaning, with no ETL project to configure. The system supplies the rows. This connector returns a copy and a change log. A person, or the system, applies the copy if you choose to.

**What to send from the source**

| What you have | Field it fills | Why it matters |
| --- | --- | --- |
| The export, in its own headers | `rows` | Any columns are accepted. The tool reports what it could not do. |
| Business rules | `rules` | Data validation and repair use your rules, such as amount equals quantity times price. |
| Your own field names | `target_schema` | Schema mapping onto the names you want |
| The profile from the last run | `cleaning_profile` | Reuses the confirmed mapping, and is required for drift |
| Earlier clean rows | `history` | Lets more unit slips be settled instead of quarantined |
| Corrections a person already made | `confirmed_fixes`, `reviewed_rows` | Applies those corrections and can learn similar ones |
| A column that names the entity, and one that names the source | `entity_key`, `source_field` | Use when several sources report the same thing |

**Send headers as they are.** Do not rename columns before the call. The result includes the column mapping. Examples you can send unchanged include headers such as Party Name and Inv Amt, and values such as mixed dates and amounts written with separators.

<!-- TODO Saifs AI: paste the header synonyms the live server accepts, once a response shows `column_mapping` or the equivalent list. Do not publish a synonym table from the research doc alone. -->

**File formats.** The hosted tool accepts records, or a list of lists with the header row first. The local engine reads CSV, TSV, and JSON from a path in the sandbox. A rules file can be CSV, TSV, JSON, or a text file with one rule per line. Save an Excel sheet as CSV before the local engine, or let the agent convert it in the sandbox. The original file is not modified.

## How it works

The connector offers two routes to the same data cleaning engine. The agent picks the route by the size of the file, and by whether the rows may leave your environment.

**In plain language:** you give it a messy export. It returns a clean copy, a change log of every edit, a quarantine list, and an agent-ready score.

**For technical readers:** `clean_dataset` runs the steps below in order. Any other `job` runs one step. `steps` can override that list.

| | Hosted tool | Local engine |
| --- | --- | --- |
| Tool name | `data_cleaning` | `data_cleaning_get_engine` |
| Use when | Up to about 20,000 rows and about 2,000,000 cells | Anything larger, a file already in the sandbox, or a policy that rows must stay in your environment |
| What the agent sends | The rows, plus rules and settings | File paths and settings only |
| What comes back | The cleaning result | A ready-to-run Python script |
| Where it runs | Saifs AI endpoint | Your agent's code-execution sandbox |
| Where the rows go | To the Saifs AI endpoint for the calculation | Nowhere. They stay in the sandbox. |
| Requires | Nothing extra | Code execution, and `numpy` |

**Local engine, step by step**

1. The user uploads a file, or the file is already in the sandbox.
2. The agent calls `data_cleaning_get_engine` with the path and any settings.
3. The connector returns the script. The agent saves it exactly as given.
4. The agent runs the script in the sandbox against the file. It does not rewrite the script.
5. The script prints the decision cards. It writes `data_cleaning_result.json`, `clean_rows.csv`, `change_log.csv`, and `cleaning_profile.json`.
6. The input file is not modified. The agent reports the decisions and the change log.

### Inside the connector

1. The user asks the agent to clean, validate, or score a file.
2. The agent chooses a tool. The connector passes the call to the Saifs AI MCP server over HTTPS at `https://data-cleaning-mcp-microsoft.saifs.ai/mcp`.
3. The server checks the signed-in account.
4. The hosted tool runs the engine and returns the result. The engine tool returns the same engine as a script.
5. The agent explains the result in plain language. The cleaned values and the change log come from the engine. The wording comes from the agent.

### Inside the calculation

Both routes use the same engine. The hosted tool's `job` list gives the order below.

`clean_dataset` runs all of them. A single job runs one.

| Step | What the engine does | Method |
| --- | --- | --- |
| 1. Map columns | Matches headers to standard fields, or to `target_schema`. Accepts a score at or above `mapping_threshold` (default 0.85). Lower scores come back as candidates. | Jaro-Winkler, plus value-pattern checks |
| 2. Standardise values | Reads dates, amounts, phones, and letter case for `locale` (default IN). `date_order` may be DMY or MDY. | Fixed parsers. `fix_case` defaults to true. |
| 3. Profile | Reports types, missing share, distinct counts, and suspected keys. | Column profiling |
| 4. Infer patterns | Learns the dominant shape of a column and flags values that do not fit. | Pattern shapes on the values |
| 5. Check digits | Checks GSTIN, GTIN, Luhn, ISBN, or IBAN when `check_digit_fields` says so, or when a field is recognised. | Check digits. A failure is a flag. There is no lookup. |
| 6. Validate rules | Marks each rule pass or fail, with the failing rows. | The rules you send. Up to 200 rules. |
| 7. Fix typos and units | Settles a unit slip or a typo only when one candidate passes the rules and the history. If several pass, the row is quarantined. | Deductive checks. `history` settles more of them. |
| 8. Fix errors | Changes the fewest low-trust fields so the rules can pass. Stops at `max_change_fields` (default 3, maximum 4). | Fellegi-Holt minimum-change repair. `field_weights` mark trust. |
| 9. Rule discovery | Searches for functional dependencies. Keeps a rule at `fd_min_confidence` (default 0.95) and `fd_min_support` (default 20). | Functional dependencies. Applied only if `apply_discovered_rules` is true. |
| 10. Fill missing | Fills blanks. Stops on a column whose missing share is above `max_missing_impute` (default 0.4, and 0.4 is the maximum). | Hot-deck or the EM method. `impute_method` defaults to auto. `none` turns filling off. |
| 11. Flag outliers | Flags unusual values. Does not delete them. | MAD robust z-score. `outlier_z` defaults to 3.5. A Benford test is included. |
| 12. Relationship outliers | Flags a row whose combination of values is unusual, even when each value looks normal. | A multi-column check on `column_groups` or `relations` |
| 13. Find duplicates | Returns groups above `duplicate_threshold` (default 0.9). Does not merge. | Fellegi-Sunter scoring. Keys come from `duplicate_keys`, or are chosen for you. |
| 14. Compare loads | Compares this file with the last `cleaning_profile`. | Data drift detection against that profile |
| 15. Quality score | Returns an agent-ready score before and after the run. | A data quality score on the file you sent |
| 16. Change log | Lists the edits. `log_standardisation` defaults to false, so routine standardising is omitted unless you ask. | The log itself. `max_log` defaults to 5,000. |

**Why the answer does not change.** The tools are marked idempotent. The calculation does not use a language model. `seed` defaults to 7 and fixes the one step that draws a choice.

The same input and the same settings return the same numbers. The agent's wording may still vary.

**What happens when data is thin.** No column is required, so a sparse file still runs. The result says what could not be done. Imputation stops above a missing share of 0.4.

Ambiguous repairs are quarantined. Outlier detection flags rows and does not drop them. Missing value imputation is a fill with a range, not a silent overwrite.

<!-- TODO Saifs AI: confirm whether identifier columns are never imputed, and whether a file under 30 rows runs profiling and rules only. The schema does not state either limit. -->

**Where the working is shown.** The change log lists edits. Quarantine lists candidate fixes. The column mapping lists header matches. Decision cards are the plain-language summary. The local engine writes that working to `change_log.csv` and `data_cleaning_result.json`.

The change log is the data observability for the file you sent. This connector does not watch your systems between calls. The score beside it is a data quality result for those rows only.

<!-- TODO Saifs AI: replace the names in the previous paragraph with the JSON keys from a live response. The endpoint returned 401 without a signed-in call, so the keys are not confirmed. -->

## Data handling and security

| Question | Local engine | Hosted tool |
| --- | --- | --- |
| Does business data reach Saifs AI? | No. Paths and settings are sent. The rows stay in the sandbox. | Yes. The rows in the call are sent. |
| Where is it processed? | Your agent host's code sandbox | `data-cleaning-mcp-microsoft.saifs.ai`, over HTTPS |
| What happens to the rows after the call? | They never left your sandbox. The script writes its outputs there. | <!-- TODO Saifs AI: confirm retention. State whether hosted rows are discarded after the response, and whether any audit record stores a hash rather than the rows. Do not claim "nothing stored" until this is confirmed. --> |
| What does Saifs AI record? | Only that the script was requested, until the audit fields are confirmed | <!-- TODO Saifs AI: confirm the audit and billing fields (time, account, tool name, hashes) and that row contents are not stored. --> |

**What "sandbox" means.** The sandbox is the code-execution environment of your own agent host, such as Copilot Studio or Azure AI Foundry. It belongs to your tenant, not to Saifs AI. Your host's data policies still apply.

**Use the local engine only.** If policy says rows must not leave your environment, turn off the hosted tool.

In Copilot Studio, open the agent's **Tools** tab and select the Data Validation for AI Agents server. Turn off **Allow all**, then switch off `data_cleaning`. Leave `data_cleaning_get_engine` on.

The agent can then use only the local engine. A tool added to the server later stays off until you enable it. This setting is per agent.

**Sign-in.** Users sign in with Microsoft single sign-on through OAuth 2.0. Saifs AI holds the account details needed for sign-in and billing. No API key is created or stored.

**No write access.** Neither tool creates, updates, or deletes records in your systems. Both are marked read-only. The local engine does not modify the input file.

Deduplication does not merge rows. Outlier detection does not delete rows.

**Personal data.** Exports may contain names, phones, and tax identifiers. Send only data you are allowed to process. `sensitive_fields` names extra personal fields to hide from the profile. Nothing in the file is looked up or enriched from outside.

**The script is open to review.** The local engine is plain Python with one dependency, `numpy`. Run it only after your security team has read it. Do not ask the agent to shorten or replace it.

<!-- TODO Saifs AI: confirm the hosting region, the request timeout, and any rate limit. -->

## Security and compliance

This section separates what an independent party has reviewed from what Saifs AI states about itself.

**Reviewed by Microsoft**

| Item | What it covers |
| --- | --- |
| Microsoft connector certification | Microsoft reviews this connector before listing it. The review covers the connector's function and packaging. It does not certify Saifs AI's internal systems. |

**Self-assessed by Saifs AI**

Self-assessments are statements Saifs AI makes about its own practices. They are not independent audits.

<!-- PUBLISH EACH ROW ONLY AFTER IT IS COMPLETED, WITH ITS DATE AND PUBLIC LINK. Until then, keep the row inside this comment.

| Self-assessment | What it is | Where to read it |
| --- | --- | --- |
| Microsoft Publisher Attestation | Saifs AI's answers about security, data handling, and compliance. Microsoft publishes the answers and does not verify them. Renewed every year. | Link to the Microsoft app compliance page for this offer |
| CSA STAR Level 1 | Saifs AI's Cloud Security Alliance questionnaire (CAIQ), on the public STAR Registry. Updated every year. | Link to the STAR Registry entry |
| CSA STAR for AI Level 1 | Saifs AI's AI questionnaire (AI-CAIQ) on the same registry. | Link to the STAR Registry entry |

-->

| Design fact | Detail |
| --- | --- |
| Customer rows on the local route | Stay in your sandbox. The hosted route receives the rows in the call. Retention after that call is not stated on this page until it is confirmed. |
| Passwords held | None. Sign-in is Microsoft single sign-on. |
| Write access to your systems | None |
| Lookup or enrichment | None. Values are not fetched from outside the file. |
| Local engine | Plain Python, one dependency (`numpy`), open to review before it runs |

**Independent audits.** Saifs AI does not yet hold an independently audited certificate such as SOC 2 or ISO 27001. If your review needs a completed security questionnaire, request it through the [contact page](https://saifs.ai/contact-us).

## Token usage and cost

With the hosted tool, token use grows with every row, because the rows are part of the call. With the local engine, the rows stay in the sandbox.

The agent receives a script, then reads the printed decisions. The row text does not pass through the model on that route.

<!-- TODO Saifs AI: measure tokens before publishing figures. Use a named tokenizer on three sizes: a small file, the hosted limit (about 20,000 rows or about 2,000,000 cells), and a larger file. Record tokens per row, tokens per result line, and the script's fixed cost. State the file size where the hosted tool uses fewer tokens. Convert at one dated reference price per million input tokens and per million output tokens, and give a monthly example. Add the note for hosts that bill per action. Do not publish a number until it is measured. -->

**Which route to choose, from the size limits that are known today**

- **Within about 20,000 rows and about 2,000,000 cells:** the hosted tool can take the call.
- **Above either limit, or the file is already in the sandbox:** the local engine is the route the tool specifies.
- **Rows must not leave your environment:** the local engine, with `data_cleaning` switched off.
- **The same file will be cleaned again in one session:** the local engine can reuse the script the sandbox already has. The token effect of that reuse is not measured yet.

**Hosts that bill per action.** Some hosts charge a fixed amount per tool call and do not bill by token. On those hosts, pick the route for file size and data handling. A token saving, once measured, would not change that bill. The local engine may use one extra action to run the script.

## Prerequisites

- A Saifs AI account, created by signing in with Microsoft single sign-on
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Access to Copilot Studio, Microsoft 365 Copilot, Azure AI Foundry, Power Automate, Power Apps, or Azure Logic Apps
- An export, a paste of rows, or a file the agent can read. No column is required.
- For the local engine: code execution on the agent host

## How to get credentials

No keys or secrets are needed. When you create a connection, you are redirected to sign in with your Microsoft account and approve access. If the connection expires, sign in again from the connection page.

## Get started with your connector

1. In Copilot Studio, open your agent and go to **Tools**, then **Add a tool**.
2. Search for **Data Validation for AI Agents** and select it.
3. Create a connection and sign in.
4. Add this to the agent's instructions: "For cleaning, validation, header mapping, blanks, duplicates, outliers, or file readiness, call the Data Validation for AI Agents tool. Do not edit values, fill blanks, or decide duplicates yourself."
5. Provide the export by pasting rows, uploading a file, or pointing at a path. Send headers and values as they are.
6. Ask in plain language. For example: "Clean up this Excel export and show the change log."

Treat the result as a proposal. Check quarantined rows before another agent, or a person, uses the clean copy.

## For administrators

Enabling this connector gives the organisation one approved way to decide **whether this data is ready for the agent**. Later agents then act on a checked export.

**Why this is an administrator decision.** Without a shared method, makers ask a language model to clean a spreadsheet. A later run can change a different cell, and there is no change log to compare. A governed connector applies one data validation method, and the change log can be kept with the run.

- **You choose who can connect it.** Tenant and environment permissions apply. Only users who can create a connection can call the tools.
- **You keep the system of record.** The connector returns a copy and a change log. It does not write back.
- **You can check the result.** The same input and settings can be rerun. Uncertain rows are quarantined, not guessed.
- **You control where rows go.** Turn off the hosted tool per agent when rows must stay in your environment.
- **You set data policy as usual.** Place the connector in the Business, Non-business, or Blocked group with your Power Platform data loss prevention policy.
- **There are no secrets to rotate.** An expired connection is renewed by signing in again.

**Review checklist**

- Decide whether the hosted tool is allowed, or the local engine only
- Assign the connector to the correct data policy group
- Review the privacy policy and the terms
- Read any self-assessment that is later published under Security and compliance
- Review the local engine script if your policy requires it
- Confirm hosted-row retention with Saifs AI before you allow the hosted tool for sensitive files

## For developers

No field is required. Send the columns the file has. The response reports what was skipped.

Do not rename headers or pre-convert values before the call.

Set `job` to one step, or leave the default `clean_dataset`. The step names are:

- `map_columns`
- `standardise_values`
- `profile_data`
- `infer_patterns`
- `check_digits`
- `validate_rules`
- `fix_typos_units`
- `fix_errors`
- `discover_rules`
- `fill_missing`
- `flag_outliers`
- `relationship_outliers`
- `find_duplicates`
- `compare_loads`
- `quality_score`
- `change_log`

### Rows and rules

| Field | Meaning | Needed for |
| --- | --- | --- |
| `rows` | Records, or a list of lists with the header row first. Up to 20,001 entries. | Every job that reads the file |
| `headers` | Headers only, for `map_columns` without data. Up to 500. | Schema mapping when you have no rows |
| `rules` | Rule strings. Up to 200. Examples: `amount = qty * price`, `due_date >= invoice_date`, `unique(invoice_no)`. | Data validation and repair |
| `target_schema` | Your fields as `{field: {type, synonyms: [...]}}` | Column mapping onto your names |
| `field_weights` | `{field: weight}`. A higher weight is changed later. | Fellegi-Holt repair |
| `cleaning_profile` | The profile object from an earlier run | Stable mapping, and data drift detection |
| `history` | Earlier clean rows. Up to 20,000. | Settling unit slips instead of quarantining them |
| `confirmed_fixes` | Objects with `row` (0 is the first data row), `value`, and `field` or `header`. Up to 5,000. | Applying a person's correction |
| `reviewed_rows` | Row numbers a person has checked. Up to 5,000. Other cells on those rows count as correct. | Learning, together with `confirmed_fixes` |
| `entity_key` | Column that names the thing several sources describe | Matching across sources |
| `source_field` | Column that names the source. Detected when the name looks like source, src, or provider. | The same multi-source case |

### Options and defaults

| Option | Default | Purpose |
| --- | --- | --- |
| `job` | `clean_dataset` | Which step to run. The default runs the full pipeline. |
| `steps` | None | An explicit step list. It overrides `job`. |
| `locale` | `IN` | Country used for phones and for ambiguous dates |
| `date_order` | Not set | `DMY` or `MDY` when you need to force the order |
| `seed` | 7 | Fixes the draw used in filling. Range 0 to 2,147,483,648. |
| `impute_method` | `auto` | `auto`, `hot_deck`, `em`, or `none` |
| `max_missing_impute` | 0.4 | Missing share above this is not filled. The maximum is 0.4. |
| `outlier_z` | 3.5 | Robust z-score cutoff. Range 1 to 100. |
| `duplicate_keys` | Chosen for you | Fields that identify a record |
| `duplicate_threshold` | 0.9 | Group score cutoff. Range 0.5 to 0.9999. |
| `apply_discovered_rules` | false | When false, discovered rules are suggestions only |
| `fd_min_confidence` | 0.95 | Minimum confidence for a discovered rule. Range 0.5 to 1. |
| `fd_min_support` | 20 | Minimum support. The minimum you can set is 2. |
| `column_groups` | None | Field groups for the multi-column outlier check |
| `relations` | None | Relations to test, for example `amount ~ qty * price` |
| `check_digit_fields` | None | `{field: gstin \| gtin \| luhn \| isbn \| iban}` |
| `mapping_threshold` | 0.85 | Minimum score to accept a header match. Range 0.5 to 1. |
| `fix_case` | true | Apply casing rules during standardising |
| `abs_tolerance` | 0.01 | Absolute tolerance on numeric rules |
| `rel_tolerance` | 0.0005 | Relative tolerance. Maximum 0.5. |
| `max_change_fields` | 3 | Most fields a repair may change. Range 1 to 4. |
| `extra_missing_tokens` | None | Extra strings to treat as blank |
| `sensitive_fields` | None | Personal fields to hide from the profile |
| `return_rows` | true | Include the clean rows in the response |
| `max_rows_returned` | Not set | Cap on rows returned. Minimum 0. |
| `max_log` | 5000 | Cap on change-log entries. Maximum 100,000. |
| `log_standardisation` | false | When false, routine standardising is left out of the log |
| `max_listed` | 200 | Cap on listed groups, flags, and similar lists. Maximum 100,000. |
| `learn_threshold` | 0.6 | Score a learned fix needs before it is applied. Range 0.5 to 0.99. |

`params` on the local engine accepts these same options. Path arguments are `rows_path`, `rules_path`, `history_path`, `cleaning_profile_path`, `target_schema_path`, and `confirmed_fixes_path`.

### Sample request

```json
{
  "rows": [
    {
      "Party Name": "Northwind Supplies",
      "Inv Amt": "1,250.50",
      "Qty": 10,
      "Price": 100,
      "Invoice Date": "12-Jan-2025",
      "GSTIN": "22AAAAA0000A1Z5"
    },
    {
      "Party Name": "Northwind Supplies Pvt",
      "Inv Amt": "n/a",
      "Qty": 2,
      "Price": 100,
      "Invoice Date": "13/01/2025"
    }
  ],
  "rules": ["amount = qty * price"],
  "locale": "IN",
  "job": "clean_dataset"
}
```

### Sample response (shortened)

<!-- TODO Saifs AI: paste a shortened real response from data_cleaning. A call to https://data-cleaning-mcp-microsoft.saifs.ai/mcp on 18 November 2026 returned 401 Unauthorized, so no response body is shown. Do not fill this block from the research doc. -->

The live tool describes what that response contains. It includes decision cards and an agent-ready score before and after. It includes the column mapping, the clean rows, and a cell-by-cell change log.

It includes quarantined rows, missing-value ranges, outliers, and a Benford test. It includes duplicate groups, check-digit failures, and rules found in the data. When a profile was sent, it includes drift against the last load, and it returns an updated `cleaning_profile`.

**It does not guess.** Ambiguous rows are quarantined. The change log is where an edit, and its reason, should be read. Confirm the exact key names from the live response before you bind to them.

### Response sections

| Section | Use it for |
| --- | --- |
| Decision cards | Plain-language actions, as printed by both routes |
| Agent-ready score, before and after | Whether the file is in a better state, and what remains |
| Column mapping | Which headers were matched, and which stayed as candidates |
| Clean rows | The copy to pass on, after a person accepts it |
| Change log | Every edit the engine made. Also written to `change_log.csv` on the local route. |
| Quarantine | Rows not fixed, with the candidate fixes |
| Missing-value ranges | Filled blanks and the range around them |
| Outliers and the Benford test | Values to review. They are not deleted. |
| Duplicate groups | Possible duplicates. They are not merged. |
| Check-digit failures | Identifier typos, including GSTIN validation |
| Rules found in the data | Suggestions, unless `apply_discovered_rules` was true |
| Drift | Difference from the last load. Present when a profile was sent. |
| `cleaning_profile` | Save it and send it on the next run |

## Known issues and limitations

- **A copy, not a write-back.** It does not update the source file, an ERP, a CRM, or Dataverse. A person approves the clean rows before they are used.
- **Not a professional opinion.** Data cleaning and data validation here are operational. They are not legal, tax, or audit advice.
- **Hosted tool size.** About 20,000 rows and about 2,000,000 cells per call. Use the local engine above that.
- **No silent repair.** If several fixes would pass, the row is quarantined. Outliers and duplicates are flags only.
- **Discovered rules.** They are listed, not applied, unless `apply_discovered_rules` is true.
- **Sparse columns.** Missing value imputation stops when more than 40% of a column is missing. You cannot set the cap higher than 0.4.
- **No outside data.** Values are not enriched. A check digit does not prove the identifier exists in a registry.
- **Currency.** Amounts can be read from text that includes separators. There is no conversion between currencies.
- **Locale.** The default locale is IN. Set `date_order` when the file is not in that date order.
- **Excel on the local route.** The script reads CSV, TSV, and JSON. Convert Excel first, or convert it in the sandbox.
- **Several sources.** Set `entity_key` when one column names the shared thing. Otherwise matching across sources is only as good as the keys the tool chooses.
- **Regions.** Not available in US Government (GCC, GCC High, DoD) or China (21Vianet) clouds.

<!-- TODO Saifs AI: confirm that an IN locale parses day-first dates when date_order is omitted, and whether phone numbers are returned in E.164. -->

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| The tool cannot run, or asks you to sign in again | The connection has expired | Open the connection and sign in again |
| The agent edits cells itself and skips the tool | The instructions do not require the tool | Add the instruction in Get started |
| A large file is not accepted on the hosted tool | The file is over about 20,000 rows or about 2,000,000 cells | Call `data_cleaning_get_engine` and run the script |
| The local engine does not run | Code execution is off, or `numpy` is missing | Enable code execution, or use the hosted tool for a small file |
| Rows sit in quarantine | More than one repair passed, or the row was not safe to change | Review the candidates. Send `confirmed_fixes` for the ones you accept. |
| Blanks are left empty | The column is more than 40% missing, or `impute_method` is `none` | Fill from `history`, or accept the gap. The cap cannot be raised above 0.4. |
| Suggested rules did not change any cell | `apply_discovered_rules` defaults to false | Leave it false to review first. Set it true only when those repairs should run. |
| The script result does not match a hosted call | The agent rewrote or shortened the script | Save the script exactly as returned and run that file |
| Drift is missing | No `cleaning_profile` was sent | Send the profile from the previous run |

## FAQ

**Do I need a data engineer or a specialist to use this?**
No. A user asks in plain language. The formulas and the parsers run inside the tool. Rules help, and they are optional.

**What is the least data I need?**
Any rows you have. No column is required. Rules, a target schema, history, and a prior profile each add a specific check.

**Does my data leave my environment?**
Not with the local engine. With the hosted tool, the rows in the call are sent to Saifs AI for the calculation. You can turn that tool off.

**Will it change anything in my systems?**
No. It returns a cleaned copy and a change log. The source file is not modified, and nothing is written back.

**Our system already has an MCP server. Why add this?**
That server gives the agent the system's records. This connector adds data cleaning and data validation: a copy, a change log, and an agent-ready score. It works on rows from any source.

**Why do I get the same answer every time?**
The same input and the same settings return the same numbers. `seed` defaults to 7. The agent's wording around those numbers may vary.

**What happens if my data is incomplete?**
The tool still runs and reports what it could not do. Sparse columns are not filled past a missing share of 0.4. Ambiguous rows are quarantined.

**Can I check how a result was produced?**
Yes. Read the change log, the quarantine candidates, and the column mapping. Save `cleaning_profile` if you want the next run to start from the same mapping.

**How large a file can it handle?**
The hosted tool is for about 20,000 rows and about 2,000,000 cells. Larger files use the local engine.

**Can it run on a schedule?**
Yes, by calling it from a Power Automate flow with a recurrence trigger. The connector itself does not schedule, watch, or alert.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment). Microsoft Premium connector licensing applies separately.

## Support

**Data Validation for AI Agents - Saifs AI** is published by Saifs AI.

- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Learn more: [https://saifs.ai](https://saifs.ai)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Privacy: [https://saifs.ai/privacy_policy/data-validation](https://saifs.ai/privacy_policy/data-validation)
- Terms: [https://saifs.ai/eula](https://saifs.ai/eula)
- MCP endpoint: `https://data-cleaning-mcp-microsoft.saifs.ai/mcp`
