# Customer Intelligence - Saifs AI

**Customer Intelligence - Saifs AI** helps sales teams decide **who to call today**, which accounts are slipping, and how much money is at risk if they go quiet.

You send one row per purchase. You get A, B, C and D tiers, a call list, and a value at risk figure for each account and for the whole book.

It is built for sales teams that rank customers from a spreadsheet, an accounting export, or a CRM, and need a ranking they can rerun next week.

| If you are | Start with |
| --- | --- |
| A business owner, sales manager, or founder | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [Where the data comes from](#where-the-data-comes-from) |
| An administrator or security reviewer | [Data handling and security](#data-handling-and-security), [Security and compliance](#security-and-compliance), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [Where the data comes from](#where-the-data-comes-from), [For developers](#for-developers) |
| A finance reviewer checking the method | [Inside the calculation](#inside-the-calculation) |

## The problem it solves

Most sales teams still work the book by habit. Every account gets a similar call, or last year's largest invoices get all the attention. The accounts about to stop buying stay quiet until the revenue chart drops. By then the easy conversation is gone.

Dashboards can show a tier label. Managers act on money: what is at risk, and who is worth an hour today.

Customer Intelligence makes that ranking available to any AI agent:

- **Who to call today**: a short call list, ranked by money, with a reason on each name.
- **Who is slipping**: accounts whose purchases have gone quiet, with a churn reading so the team can act before the revenue drops.
- **What the account is worth next**: customer lifetime value over the coming year, and value at risk if nobody makes contact.
- **How the book splits**: customer segmentation into A, B, C and D, from predicted future value when the history can support it.
- **Which names are the same firm**: spelling variants of a legal name are merged, and uncertain pairs are listed for a person to decide.

## Why choose it: the same answer every time

Customer Intelligence is deterministic. The same purchases and the same settings return the same tiers, the same value at risk, and the same call list. The numbers come from established models. They are not written afresh by a language model each time you ask.

That matters because a sales manager and a finance lead act on the figures. They need a ranking they can rerun, compare, and explain. The agent's wording around the numbers may vary. The numbers do not.

| What it does | Established method |
| --- | --- |
| Splits the book into A, B, C and D by concentration of value | Pareto classification, also called ABC analysis |
| Reads how recently, how often, and how much each account buys | RFM scoring |
| Estimates whether the account is still an active buyer | BG/NBD |
| Projects spend per future order, then customer lifetime value | Gamma-Gamma spend model |
| Estimates how a contact changes the chance of moving tier | Markov migration |
| Turns those results into a ranked call list and a book total | Fixed wording filled with the calculated figures |

**It does not guess.** Below 10 customers, or below 5 customers who have bought more than once, the survival model is not fitted, and the response says why. The churn reading is withheld with it. An account with a single purchase is marked low confidence. A large predicted value on that account is a book-wide prior, not that account's own forecast.

**It recommends and does not act.** It never places a call, sends a message, or writes to a CRM, accounting system, or Dataverse.

## Who should use this

| Role | How it helps |
| --- | --- |
| **Sales managers** at distributors and wholesalers | A Monday call list, the accounts going quiet, and the value at risk on each |
| **Founders and owners** of D2C and e-commerce brands | A first ranking of the book from an order export, without a data science team |
| **Sales reps** | A short list for today: save a quiet account, grow an active one, or check a new one |
| **RevOps leads and consultants** | The same customer segmentation on each client book, with a trace behind every tier |
| **Agency account leads** | Which clients are slipping, and which still have room to grow |
| **Travel trade sales leads** | Who among repeat buyers has gone quiet before the season turns |
| **Finance partners of the sales team** | A book-level value at risk figure, with a range, for the weekly review |
| **Agent makers and developers** | A tested ranking they can add to an agent, instead of asking a language model to score a CSV |

**Best fit:** distributors, wholesalers, agencies, D2C brands, e-commerce sellers, and travel trade businesses that sell repeatedly to the same accounts and decide who to call today.

**Not required:** a data science team, a configured scoring module, or a CRM. A purchase export is enough to start. A log of rep calls makes the contact figure sharper.

## Use cases

Each use case below can be built as an agent. The agent types are listed in [Agents you can build](#agents-you-can-build).

**1. Who to call today**
Scenario: a sales manager starts the week with a full book and a few hours of rep time.
Outcome: a call list of SAVE, GROW, and VERIFY actions, each with a reason and a money figure.
Ask: "Who should we call today?"

**2. Accounts that are slipping**
Scenario: a few large buyers have gone quiet, and the team finds out only when the invoice report drops.
Outcome: those accounts flagged, with days since the last purchase and the value at risk if they are neglected. This is the churn reading: the chance the account stops buying.
Ask: "Which of my top customers are going quiet?"

**3. Customer segmentation into A, B, C and D**
Scenario: the team still treats a first invoice the same as a ten-year account.
Outcome: every account placed in A, B, C, or D from predicted future value when history allows, and from past revenue when it does not.
Ask: "Tier my customers A to D."

**4. Money at risk across the book**
Scenario: a founder has to explain, in one number, where attention is owed this month.
Outcome: book-level value at risk and upgrade upside, each with a range.
Ask: "How much revenue is at risk if we neglect these accounts?"

**5. Where a conversation can grow the account**
Scenario: reps spend their hours on accounts that would have ordered anyway.
Outcome: B and C accounts where a contact has the most room to move the account up, ranked by that contact value.
Ask: "Which B accounts are worth a growth call?"

**6. Why this account has this tier**
Scenario: a rep or a finance partner challenges one name on the list.
Outcome: the working for that account: past rank, RFM cell, probability the account is still active, customer lifetime value, and the money figures.
Ask: "Why is Adventure Works on the call list?"

**7. One firm typed several ways**
Scenario: the export says "Acme Pvt Ltd" on some invoices and "ACME PRIVATE LIMITED" on others.
Outcome: those rows merged into one account, with a report of what was merged. Pairs that might be the same firm, and might not, are left separate for a person to decide.
Ask: "Score this Tally sales register, and tell me which names you merged."

**8. A call log, when you have one**
Scenario: the CRM records the date of each rep call or visit.
Outcome: for any tier with at least 10 transitions that followed a contact, the contact value is measured from those outcomes. Thinner tiers keep the stated assumption, and the response says which is which.
Ask: "Use our call log as well as the invoices. What is a contact worth?"

**9. A large book**
Scenario: the export has more than about 500 purchase rows.
Outcome: the same calculation runs in the agent's own sandbox through the local engine, and the purchase rows stay there.
Ask: "Score this 10,000-row order export."

## Agents you can build

Each agent is one agent in your agent host with this connector added as a tool. The last column shows what else to add.

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Daily call-list agent | Answers who to call today, with a reason, a churn reading, and a money figure on each quiet account | A purchase file, or a data source from [Where the data comes from](#where-the-data-comes-from) | 1, 2 |
| Customer segmentation agent | Returns A, B, C and D for the book, with the trace behind each tier | The same data source | 3 |
| Book exposure agent | Reports value at risk and upgrade upside for the whole book, with ranges | The same data source | 4 |
| Growth-call agent | Ranks active B and C accounts by the value of a contact | The same data source | 5 |
| Account explanation agent | Shows the working for one named account | The same data source | 6 |
| Name-cleanup agent | Merges legal-name variants and lists pairs that need a human decision | The purchase file as exported, without pre-cleaning the names | 7 |
| Call-log agent | Measures contact value per tier from call or visit dates | A second file: customer and date of each contact | 8 |
| Large-book agent | Runs the local engine on a full export of thousands of purchases | Code execution on the agent host | 9 |
| CRM book agent | Reads closed orders from the CRM, scores them, and hands the call list back for a person to approve | The CRM's own MCP server | 1, 3 |
| Scheduled call-list agent | Sends the call list each morning without anyone asking | A Power Automate flow with a recurrence trigger, and email or Teams | 1 |

The tools run on request. Scheduling comes from the flow, not from the connector. Set `as_of` to the run date. If you leave it unset, the analysis date stays at the latest purchase in the file, and the same file returns the same call list.

## At a glance

| Item | Detail |
| --- | --- |
| Tools | `customer_tiering` (hosted calculation) and `customer_tiering_get_engine` (local engine) |
| Minimum input | One purchase row with `customer_id`, `date`, and `amount` greater than zero |
| Hosted tool size | Meant for up to about 500 purchase rows per call |
| Larger files | Local engine, runs in your agent's code sandbox, needs only `numpy` |
| Writes to your systems | None |
| Engine version | Returned in every response as `metadata.model_version` |

## Where the data comes from

Customer Intelligence does not connect to your systems. The agent brings the rows to it, from any source that can supply purchases.

The connector has not been certified against each system named below. It needs rows, not a live connection.

| Data source | How the agent gets the data | Best route |
| --- | --- | --- |
| A spreadsheet or CSV from accounting, CRM, or commerce software (for example Tally, QuickBooks, Zoho, Salesforce, HubSpot, Shopify) | The user uploads the file to the agent | Local engine for large files, hosted tool for small ones |
| A CRM or ERP that has its own MCP server (for example Dynamics 365, Business Central, Dataverse, SAP) | The agent reads orders or invoices with that server's tools, then passes the rows on | Hosted tool up to about 500 purchase rows. Above that, export a file and use the local engine. |
| Dataverse tables | The Dataverse MCP server or the Dataverse connector | Same as above |
| SQL, an online workbook, or a SharePoint list | A connector on the same agent or flow | Same as above |
| A scheduled flow | A Power Automate flow reads the source on a schedule and calls this connector | Hosted tool, for a file within about 500 rows |
| A rep call or visit log | Pass `rep_contacts` on the hosted tool, or `contacts_path` on the local engine | Either |

**Works alongside your CRM's MCP server.** That server gives the agent the CRM's records. This connector adds the ranking and the call list. The CRM supplies the purchases. Customer Intelligence calculates who to call today. A person makes the call.

**What to pull from the source**

| Data in your system | Field it fills | Why it matters |
| --- | --- | --- |
| Customer code or customer name | `customer_id` | Groups every purchase for one account. A code is steadier than a name. |
| A display name, when the code is the identifier | `customer_name` | The result names the account the way the team does. Grouping still uses the code. |
| Invoice or order date | `date` | Purchase timing. A per-customer total, with no dates, cannot be scored. |
| Invoice or order amount | `amount` | Value. Zero and negative rows are dropped and counted. |
| Rep calls or visits | `rep_contacts` or a contacts file | Lets contact value be measured for tiers with enough history. Customer and date only. |

**Column names on the local engine are recognised as they are.** You do not rename them first. The printed result includes a `Columns read` line with the headers it used.

The hosted tool does not read file headers. The agent sends `customer_id`, `date`, and `amount` on each row. Optional `customer_name` labels the account when the identifier is a code.

| Field | Headers the local engine recognises (examples) |
| --- | --- |
| `customer_id` | customer_id, Customer, AccountId, Party Name, buyer, client |
| `customer_name` | customer_name, ledger_name, company_name, doctor_name, dealer_name, bill_to |
| `date` | date, Invoice Date, order_date, posting_date, bill_date, created_at |
| `amount` | amount, Invoice Amount, revenue, net_amount, grand_total, order_value |

Headers such as stockist, clinic, gstin, email, and phone are not matched. The local engine then stops with "Need customer/date/amount columns." Rename that column to `customer_id` in the file, or in the rows, and run the same script again.

**File formats for the local engine.** CSV, TSV, JSON, and JSON Lines. Delimiters it will detect: comma, semicolon, tab, and pipe. Dates may be YYYY-MM-DD, DD-MM-YYYY, DD/MM/YYYY, MM/DD/YYYY, or YYYY/MM/DD. Amounts may include a currency symbol, thousands separators, or an accounting negative such as (500). Save an Excel sheet as CSV first, or have the agent convert it in the sandbox.

Send the names as they appear in the source. The engine merges case, spacing, punctuation, and legal-form spellings such as Pvt and Private, or Ltd and Limited. It does not merge on a hunch. "Acme Pvt Ltd" and "Acme & Co" stay apart until you send an override.

## How it works

The connector offers two routes to the same calculation. The agent picks the route by the size of the file.

| | Hosted tool | Local engine |
| --- | --- | --- |
| Tool name | `customer_tiering` | `customer_tiering_get_engine` |
| Use when | Up to about 500 purchase rows | Anything larger, or when the rows must stay in your environment |
| What the agent sends | The purchase rows, and a call log if you have one | A file path and settings only |
| What comes back | The full result, including every account | A ready-to-run Python script |
| Where the calculation runs | Saifs AI endpoint | Your agent's code-execution sandbox |
| Where the purchase rows go | To the Saifs AI endpoint for calculation | Nowhere. They stay in the sandbox. |
| Requires | Nothing extra | An agent host with code execution, and `numpy` |

**Local engine, step by step**

1. The user uploads a purchase file, or points the agent at one.
2. The agent calls `customer_tiering_get_engine` with the file path and any settings.
3. The connector returns the calculation script.
4. The agent runs that script, unchanged, in its sandbox against the file.
5. The script prints the call-list decisions and writes the full ledger to `customer_tiering_result.json` in the working directory. It writes the name map to `customer_id_map.json`.
6. The agent reports the decisions. For one account, it reads that account from the ledger file.

The only lines the agent may change are `PATH`, `AS_OF`, `CURRENCY`, `OUT`, `OVERRIDES`, and `CONTACTS` at the bottom of the script.

### Inside the connector

1. The user asks who to call, who is slipping, or how the book splits.
2. The agent chooses a tool. The connector passes the call to the Saifs AI MCP server over HTTPS at `https://customer-tiering-mcp-microsoft.saifs.ai/mcp`.
3. The server checks the signed-in account.
4. The hosted tool runs the engine on the rows and returns the result. The engine tool returns the same engine as a script, for the agent to run in its sandbox.
5. The agent reads the result and explains it in plain language. The numbers come from the engine. The wording comes from the agent.

### Inside the calculation

Both routes run the same engine. Each response lists which steps ran, under `metadata.layers_run`.

| Step | What the engine does | Method |
| --- | --- | --- |
| 1. Clean names | Folds case, spacing, punctuation, and legal-form spellings, then merges values that match. Similar names that are not the same on those rules are listed, unmerged, under `identity_cleaning.review_candidates`. | Rule-based identity cleaning, version `identity/1.0` |
| 2. Keep usable purchases | Drops rows with no customer, no readable date, or an amount that is not above zero, and counts the drops. | Row checks |
| 3. Past concentration | Sorts accounts by revenue to date. A is the share up to 80 percent, B up to 95 percent, C up to 99 percent, and D is the rest. Also reports how concentrated the book is. | Pareto classification (ABC analysis) |
| 4. Buying pattern | Scores each account 1 to 5 on recency, frequency, and monetary value, and names the cell. | RFM scoring |
| 5. Still buying | Fits how often repeat buyers purchase, and the chance an account has gone quiet. Needs at least 10 customers and 5 repeat buyers, and more than one agreeing fit. Otherwise this step is skipped and `metadata.refusals` says why. | BG/NBD |
| 6. Future spend | Estimates the value of a future order, then discounts expected orders over the horizon. The default horizon is 12 months. The default discount rate is 10 percent a year. | Gamma-Gamma spend model, summed into customer lifetime value |
| 7. Tier movement | Builds monthly moves among A, B, C, D, and Lost from the trailing year of revenue. If a call log is present, splits those moves into ones that followed a contact and ones that did not. A tier is "learned" only with at least 10 touched transitions. Under about 3 months of history the counts are zero and this step is skipped. | Markov migration, with a small smoothing constant on each cell |
| 8. Final tier | Accounts with more than one purchase are tiered by a Pareto cut on predicted customer lifetime value. A single-purchase account keeps its historical tier, and is marked low confidence. | Pareto cut on the predicted value, with a thin-data guard |
| 9. Money | Value at risk is the value of keeping the account, times the chance it lapses if neglected, over a default window of 3 months. Contact value applies a retain effect and an upgrade effect. Without enough call-log evidence those effects are 0.30 and 0.15. Each account's money figure includes a range that is wider when that account has few purchases. | Arithmetic on the fitted values. The per-account range is a width from the purchase count. The book total's range resamples accounts with a fixed seed. |
| 10. Decisions | Writes plain-language actions: CALL NOW, GROW, and CHECK, each with a reason, a worth, and a confidence. | Fixed templates filled with the calculated numbers |
| 11. Call list | Builds a queue of SAVE, GROW, and VERIFY, default length 8. SAVE is a valuable account that looks quiet. GROW is an active account with room to move up. VERIFY is a new account that looks strong and has thin history. | Ranking by the money figures, with low-confidence accounts kept out of SAVE and GROW |

**Why the answer does not change.** Every step is a fixed calculation. The fit uses fixed starting points. The book-level range draws 400 resamples from a fixed seed. The same purchases, the same settings, and the same engine version return the same numbers.

**What happens when data is missing.** The engine uses a stated default and records it under `money_assumptions`: horizon 12 months, annual discount rate 0.10, risk window 3 months, contact effect 0.30, upgrade probability 0.15, currency label USD. There is no currency conversion. When the survival model cannot be fitted, predicted value and value at risk are withheld for the book, past-revenue tiers are still returned, and the reason is in `metadata.refusals`.

**Each account carries its working.** `explanation_trace` states the historical rank, the RFM cell, the BG/NBD inputs, the customer lifetime value, the money figures, and any confidence warning.

A call log changes the contact figure. It does not remove an account you called yesterday. The list is rebuilt from purchases and `as_of` on every run.

## Data handling and security

| Question | Local engine | Hosted tool |
| --- | --- | --- |
| Do purchase rows reach Saifs AI? | No | Yes, the rows passed in the call |
| Where are they processed? | Your agent host's code sandbox | `customer-tiering-mcp-microsoft.saifs.ai`, over HTTPS |
| What happens to the rows after the call? | They never left your sandbox | <!-- TODO Saifs AI: confirm retention and logging. Do not publish a storage claim until this is confirmed. The scoring code reviewed on 16 November 2026 does not write purchase rows to a customer store. --> Not confirmed for this listing. |
| What does Saifs AI record? | Only that the engine script was requested, to the extent the host's own logs record the call | <!-- TODO Saifs AI: confirm whether the audit record is time, account, tool name, and a hash, or something else. --> Not confirmed for this listing. |

**What "sandbox" means.** The sandbox is the code-execution environment of your own agent host. It belongs to your tenant, not to Saifs AI. Your host's data policies still apply to it.

**Use the local engine only.** If purchase rows must not leave your environment, turn off the hosted tool. In Copilot Studio, open the agent's **Tools** tab, select the Customer Intelligence server, turn off **Allow all**, and switch off `customer_tiering`. Leave `customer_tiering_get_engine` on. Any tool added to the server later stays off by default. This setting is per agent.

**Sign-in.** The live server requires OAuth 2.0, authorization code with PKCE. The authorization server published by the endpoint is `https://multiplewords.in`. No API key is sent to the connector.

<!-- TODO Saifs AI: confirm the sign-in screen is a Microsoft account, and list the account fields kept for billing. Live metadata on 16 November 2026 shows issuer https://multiplewords.in and scopes read and write. -->

**No write access to your systems.** The connector has no action that creates, changes, or deletes records in a CRM, accounting system, or Dataverse. The OAuth scope list includes `write`. That is a sign-in scope on the authorization server. It is not a write into your CRM.

<!-- TODO Saifs AI: confirm the write scope exposes no data-changing tool. -->

**The script is open to review.** The local engine script is plain Python with a single dependency, `numpy`, and makes no network calls. Your security team can read it before allowing it to run.

## Security and compliance

This section separates what an independent party has reviewed from what Saifs AI states about itself.

**Reviewed by an independent party**

<!-- TODO Saifs AI: publish this row only after Microsoft connector certification is complete.

| Item | What it covers |
| --- | --- |
| Microsoft connector certification | Microsoft reviewed and tested this connector before listing it as a certified connector. The review covers the connector's function and packaging, not Saifs AI's internal systems. |

-->

Microsoft connector certification is added here after Microsoft completes its review. That review covers the connector, not Saifs AI's internal systems.

**Self-assessed by Saifs AI**

Self-assessments are statements Saifs AI makes about its own practices. They are not independent audits.

<!-- PUBLISH EACH ROW ONLY AFTER IT IS COMPLETED. Until then, keep the row inside this comment. Add the public link when you have it.

| Self-assessment | What it is | Where to read it |
| --- | --- | --- |
| Microsoft Publisher Attestation | Saifs AI's answers to Microsoft's questionnaire on security, data handling and compliance. Microsoft publishes the answers and does not verify them. Renewed every year. | [link to the Microsoft app compliance page for this offer] |
| CSA STAR Level 1 | Saifs AI's completed Cloud Security Alliance questionnaire (CAIQ), published on the public STAR Registry. Updated every year. | [link to the STAR Registry entry] |
| CSA STAR for AI Level 1 | Saifs AI's completed AI questionnaire (AI-CAIQ) on the same registry. | [link to the STAR Registry entry] |

-->

| Design fact | Detail |
| --- | --- |
| Customer purchase rows stored | Not confirmed for the hosted tool. See the data handling table. The local engine keeps rows in your sandbox. |
| Passwords held by this connector | None. Sign-in is OAuth 2.0. The connector does not ask for a password. |
| Write access to your systems | None |
| Local engine | Plain Python, one dependency, no network calls, open to review |

**Independent audits.** Saifs AI does not yet hold an independently audited certificate such as SOC 2 or ISO 27001. If your review needs a completed security questionnaire, request it through the [contact page](https://saifs.ai/contact-us).

<!-- TODO Saifs AI: the page https://saifs.ai/customer-intelligence currently says "Full SOC 2 compliance." Confirm the sentence above before either page is treated as final. -->

## Responsible use

This connector scores purchase history and produces money figures about accounts. A person decides whom to call, what to offer, and whether a relationship continues.

Send a customer code when the source has one. A person's name, email, or phone is accepted as `customer_id` if that is all the export contains, and it will appear in the result. The tool does not need those personal identifiers when a code exists.

The figures are decision support for sales coverage. They are not a credit decision, a price, or a reason on their own to refuse service. Contact value is an assumption of 0.30 unless a call log supplies enough evidence, and even a measured figure is observed history, not a controlled trial. The response says which basis was used.

## Token usage and cost

With the hosted tool, token use grows with every purchase row and every account in the result. With the local engine, the first run pays a fixed script cost that does not grow with the file, because the rows never pass through the model.

| Purchase file | Hosted tool: tokens and cost per run | Local engine: first run | Local engine: each later run | Comparison |
| --- | --- | --- | --- | --- |
| 200 purchase rows, 25 customers | about 22,000 tokens, about $0.07 | about 52,000 tokens, about $0.29 | about 550 tokens, under $0.01 | Hosted tool costs about 4 times less |
| 480 purchase rows, 40 customers | about 37,000 tokens, about $0.14 | about 52,000 tokens, about $0.29 | about 550 tokens, under $0.01 | Hosted tool costs about 2 times less |
| 10,000 purchase rows | Not the intended route. Sending the rows alone is about 230,000 tokens, about $2.30, before any result. | about 52,000 tokens, about $0.29 | about 550 tokens, under $0.01 | Local engine only |

Costs use a reference price of $1.25 per million input tokens and $10 per million output tokens, a common list price for a general-purpose model in November 2026. Rows the model sends to the hosted tool are output tokens, the more expensive kind. The result the model reads back is priced as input.

**Estimate your own file**

- **Hosted tool:** about 23 tokens per purchase row to send, plus about 650 tokens per customer to read the full ledger.
- **Local engine, first run:** about 52,000 tokens. The agent reads a script of about 26,000 tokens, writes that script into the sandbox, and reads about 550 tokens of printed decisions.
- **Local engine, later runs:** about 550 tokens, for as long as the sandbox keeps the script. At 480 rows that is about 67 times fewer tokens than the hosted tool.

**Which route to choose**

- **Up to about 500 purchase rows, asked once:** the hosted tool. At both measured sizes, 200 rows and 480 rows, it costs less than the first local run.
- **More than about 500 purchase rows:** the local engine. It is the route the tools describe, and at 10,000 rows it is the only practical route.
- **The same book scored again in one session:** the local engine.
- **Purchase rows must stay in your environment:** the local engine, including when the hosted tool would use fewer tokens.

**Hosts that bill per action.** Some agent hosts charge a fixed amount for each tool call or response and do not bill by token. On those hosts the two routes cost about the same, and the local engine may use one extra action. There, choose the local engine for file size and data privacy, not for cost.

How these figures were measured: token counts use the `o200k_base` tokenizer on generated sample books, with rows written as compact JSON and three fields per purchase. Result tokens include the full per-customer ledger. The script was counted once as input and once as output. Counts vary by model and by how many optional fields you send. The figures reflect engine `tiering-engine/1.2.2-sandbox` as measured on 16 November 2026.

## Prerequisites

- An account that can complete the connector's OAuth sign-in
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Access to Copilot Studio, Microsoft 365 Copilot, Azure AI Foundry, Power Automate, Power Apps, or Azure Logic Apps
- Purchase rows with a customer, a date, and an amount on each row
- For the local engine: code execution enabled on the agent host

## How to get credentials

No keys or secrets are created for you to copy. When you create a connection, you are redirected to sign in and approve access. If the connection later expires, sign in again from the connection page.

<!-- TODO Saifs AI: confirm this sign-in is a Microsoft account. The live authorization server is https://multiplewords.in. -->

## Get started with your connector

1. In Copilot Studio, open your agent and go to **Tools**, then **Add a tool**.
2. Search for **Customer Intelligence** and select it.
3. Create a connection and sign in.
4. Add this to the agent's instructions: "For any question about who to call today, which accounts are slipping, customer tiers, or value at risk, call the Customer Intelligence tool. Do not rank customers or invent money figures yourself."
5. Provide purchases by pasting rows, uploading a file, or pointing the agent at an export. Include a call log if you have one.
6. Ask in plain language, for example "Who should we call today?" or "Which accounts are slipping, and what is the value at risk?"

Treat the call list as decision support. A person decides who is actually called.

<!-- TODO Saifs AI: confirm makers search for "Customer Intelligence" and not a longer marketplace title. -->

## For administrators

Enabling this connector gives the organisation one approved way to answer who to call today, which accounts are slipping, and what is at risk, inside the agents your makers already build.

**Why this is an administrator decision.** Without a calculation tool, makers ask an agent to rank a spreadsheet. A language model can give a different ranking on each run, and the result cannot be compared week to week. A governed connector replaces that with one method for every agent and team.

- **You choose who can connect it.** Tenant and environment permissions apply. Only users who can create a connection can call the tools.
- **You keep your system of record.** The connector recommends and explains. It does not update the CRM or place a call.
- **You can check the number.** The same file and settings can be rerun. Missing inputs and refused steps are reported.
- **You control where data goes.** Turn off the hosted tool per agent to keep purchase rows in your environment.
- **You set data policy as usual.** Place the connector in the Business, Non-business, or Blocked group with Power Platform data loss prevention policies.
- **There are no secrets to rotate.** Expired connections are renewed by signing in again.

**Review checklist**

- Decide whether the hosted tool is allowed, or local engine only
- Assign the connector to the correct data policy group
- Review the privacy policy and terms, and the open gap on the privacy page noted under Support
- Read the self-assessments under Security and compliance once they are published
- Review the local engine script if your policy requires it
- Confirm the retention statement before you approve hosted-tool use

## For developers

Only `customer_id`, `date`, and `amount` are required on each purchase. `amount` must be finite and greater than zero. Send names as they appear in the source.

### Purchase fields

| Field | Meaning | Needed for |
| --- | --- | --- |
| `customer_id` | Customer code or name, as the source spells it. Required. | Grouping every purchase |
| `customer_name` | Display name when `customer_id` is a code. Optional. | Readable account names |
| `date` | Purchase date. YYYY-MM-DD is preferred. | Timing, recency, and the survival model |
| `amount` | Amount paid, greater than zero. Required. | Value, tiers, and money figures |

### Call log (optional)

Rows of `customer_id` and `date`. No amount. On the hosted tool the field is `rep_contacts`. On the local engine it is `contacts_path` or the `CONTACTS` line. A tier needs at least 10 touched transitions before its contact value is measured. Other tiers keep the defaults.

### Options and defaults

| Option | Default | Purpose |
| --- | --- | --- |
| `as_of` | Latest purchase date | Analysis date. Set it to the run date if you want silence counted up to today. |
| `horizon_months` | 12 | Length of the customer lifetime value projection |
| `annual_discount_rate` | 0.10 | Yearly discount applied inside that projection |
| `currency` | USD | Display label only. No conversion. |
| `currency_symbol` | From the currency code | Overrides the symbol, for example a rupee sign |
| `risk_period_months` | 3 | Window for the neglect reading inside value at risk |
| `contact_effectiveness` | 0.30 | Share of at-risk lapse treated as recovered by one contact, when the tier is not learned from a call log |
| `assumed_upgrade_prob` | 0.15 | Chance a contact moves the account up a tier, when that chance is not learned |
| `min_customers_for_model` | 10 | Below this, BG/NBD is not fitted |
| `min_repeat_buyers_for_model` | 5 | Below this, BG/NBD is not fitted |
| `rep_queue_max_items` | 8 | Maximum length of the SAVE / GROW / VERIFY list |
| `rep_queue_min_save_revenue` | 0 | Ignores SAVE candidates whose lifetime revenue is below this |
| `include_diagnostics` | false | Adds fit parameters when set true |
| `identity_overrides` | None | Your decisions on review candidates, as a map of source value to the account it belongs to |

The local engine accepts `file_path` (required), `contacts_path`, `currency`, and `as_of`. The other options above are the engine defaults. They are not parameters of `customer_tiering_get_engine`. To change them you would be editing the script, which the tool tells the agent not to do.

### Sample request

The response below was scored from 503 made-up purchase rows for 13 accounts, including one legal-name variant and one single-purchase account. Three rows are shown. A three-row request does not produce these figures. The run used engine `tiering-engine/1.2.2-sandbox`, the engine this connector executes. The live endpoint requires sign-in, so this sample was produced by running that engine directly.

<!-- TODO Saifs AI: replace this sample with a response from a signed-in call to the live endpoint once a token is available, and confirm the deployed version string. -->

```json
{
  "transactions": [
    { "customer_id": "Adventure Works", "date": "2026-05-18", "amount": 3220.5 },
    { "customer_id": "Acme Pvt Ltd", "date": "2026-10-03", "amount": 2760 },
    { "customer_id": "ACME PRIVATE LIMITED", "date": "2026-10-18", "amount": 2150 }
  ],
  "currency": "USD",
  "as_of": "2026-11-16"
}
```

### Sample response (shortened)

```json
{
  "status": 1,
  "message": "Customer book scored.",
  "decisions": {
    "summary": "Biggest thing to act on: adventure works looks like it is slipping away. 1 account(s) worth about $88,129 have gone quiet and need a call this week.",
    "n_decisions": 4,
    "decisions": [
      {
        "action": "CALL NOW",
        "headline": "Adventure Works looks like it is slipping away",
        "because": "They have spent $109,497 with you over 34 orders, but nothing for 182 days.",
        "worth": 88128.8,
        "worth_display": "$88,129 of future business at risk",
        "confidence": "high",
        "priority": 1
      },
      {
        "action": "CHECK",
        "headline": "1 account(s) have too little history to judge",
        "because": "These have only bought once, or only recently, so anything said about their future is a guess. Accounts: New Harbor Goods",
        "worth": null,
        "confidence": "high",
        "priority": 4
      }
    ]
  },
  "metadata": {
    "model_version": "tiering-engine/1.2.2-sandbox",
    "status": "scored",
    "as_of_date": "2026-11-16",
    "tier_basis": "predicted_clv",
    "layers_run": {
      "L1_pareto_abc": true,
      "L2_rfm": true,
      "L3_bgnbd": true,
      "L4_gamma_gamma": true,
      "L5_markov_migration": true,
      "L6_assembly": true
    },
    "refusals": [],
    "warnings": [
      "Gamma-Gamma independence assumption strained: corr(frequency, avg value) = 0.33 (|r| > 0.3)."
    ]
  },
  "book_exposure": {
    "total_value_at_risk_display": "$916,606",
    "total_value_at_risk_range_display": "$723,274–$1,109,267",
    "accounts_at_risk": 10,
    "headline": "$916,606 at risk across 10 neglected accounts · $1,301 upgrade upside in 2 B/C accounts."
  },
  "money_assumptions": {
    "contact_effectiveness": 0.3,
    "assumed_upgrade_prob": 0.15,
    "tiers_with_learned_uplift": []
  },
  "identity_cleaning": {
    "n_raw_values": 14,
    "n_customers": 13,
    "n_merged_groups": 1,
    "merged_by_rules": [
      {
        "customer": "Acme Pvt Ltd",
        "merged_from": ["ACME PRIVATE LIMITED", "Acme Pvt Ltd"]
      }
    ]
  },
  "rep_queue": {
    "counts": { "save": 1, "grow": 2, "verify": 0 },
    "queue": [
      {
        "action": "SAVE",
        "customer_id": "Adventure Works",
        "value_at_risk_display": "$88,129",
        "days_silent": 182,
        "confidence": "high",
        "rank": 1
      }
    ]
  },
  "customers": [
    {
      "customer_id": "New Harbor Goods",
      "final_tier": "D",
      "prediction_confidence": "low",
      "historical": { "total_revenue": 640.0, "n_purchase_occasions": 1 },
      "probabilistic": { "p_alive": 1.0, "predicted_clv": 103266.34 },
      "explanation_trace": [
        "Final tier D assigned by Pareto cut on predicted_clv.",
        "Confidence: Single purchase on file — P(alive) and CLV are population-prior estimates, not yet behavior-based. Treat this tier as provisional."
      ]
    }
  ]
}
```

In this sample, New Harbor Goods has one purchase of 640 and a predicted value above 100,000. That value is a population prior. `prediction_confidence` is `low`, and the decisions tell you to check the account rather than rank it. Adventure Works is the account to call: 182 days silent, value at risk 88,129 in the sample currency. No call log was sent, so `tiers_with_learned_uplift` is empty and the contact figure uses the 0.30 assumption. The warning says order size and purchase frequency moved together in this made-up book, so the spend model is under more strain than usual.

### Response sections

| Section | Use it for |
| --- | --- |
| `decisions` | Plain-language actions (CALL NOW, GROW, CHECK) with the reason, the worth, and the confidence |
| `rep_queue` | Today's SAVE, GROW, and VERIFY list, plus `delivery_text` ready to paste into a message |
| `book_exposure` | Book-level value at risk and upgrade upside, with ranges |
| `customers[]` | One account's tier, money, and `explanation_trace` |
| `identity_cleaning` | Names merged by rule, and pairs left for you to decide |
| `money_assumptions` | Which contact figures were assumed, and which were learned from a call log |
| `data_quality_report` | Rows received, used, and dropped, and a warning if every account has a single purchase |
| `metadata` | Engine version, `layers_run`, `refusals`, and `warnings` |
| `alerts`, `uplift_targets` | Slip and growth candidates behind the decisions |

## Known issues and limitations

- **Decision support only.** It does not place calls, send messages, or update any system.
- **Hosted tool size.** Meant for about 500 purchase rows per call. The schema does not hard-stop a slightly larger payload. Use the local engine above that size.
- **Same file, same list.** `as_of` defaults to the latest purchase date. The call list does not move forward by the calendar unless you set `as_of`.
- **A call yesterday is not remembered.** A call log changes the measured contact value. It does not take a recently called account off today's list.
- **Thin history.** Under 10 customers or 5 repeat buyers, the survival model is refused. A single purchase is scored at low confidence, and its predicted value can reflect the rest of the book.
- **Short books.** Under about 3 months of history, tier-migration counts are zero and that step is skipped.
- **One row per purchase.** A file that is already one row per customer, with totals and no dates, cannot support the survival model.
- **Identifier column.** If every account has exactly one purchase, the result warns that the identifier may be an invoice number. Check `data_quality_report.likely_identifier_problem`.
- **`book_summary.n_transactions`.** On this engine the field is 0 when scoring uses prepared statistics. Use `data_quality_report.rows_used` for the row count.
- **Contact value.** Without a call log, or with fewer than 10 touched transitions in a tier, contact value uses 0.30 and upgrade probability uses 0.15. A learned figure is observed history, not a trial.
- **Per-account ranges.** The band on one account widens when that account has few purchases. It is not a resampled statistical interval. The book total's range is the resampled one, with the seed fixed.
- **Currency.** The currency is a display label. There is no conversion between currencies.
- **Repeat purchases.** The model is for accounts that buy, go quiet, and may buy again. It has no contract-renewal mode for a subscription that bills on a fixed date.
- **Prospects.** An account with no purchase row is not scored. There is nothing to send.
- **Credit and pricing.** The figures are not a lending decision and not a price.
- **Regions.** Not available in US Government (GCC, GCC High, DoD) or China (21Vianet) clouds.

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| The tool cannot run, or the endpoint returns unauthorized | The connection is missing or has expired | Open the connection and sign in again |
| A refusal on layer BG/NBD in `metadata.refusals` | Fewer than 10 customers, or fewer than 5 repeat buyers, or the fit did not agree across starts | Send a wider book, or use the past-revenue tiers the response still returns |
| "Need customer/date/amount columns" | The local engine did not recognise the buyer, date, or amount header | Rename that column to `customer_id`, `date`, or `amount` and run the same script |
| "CHECK YOUR CUSTOMER COLUMN" | Nearly every account has a single purchase, which usually means the identifier is an invoice number | Point the customer field at the buyer, not the invoice |
| The agent gives tiers without calling the tool | The agent instructions do not require the tool | Add the instruction in Get started |
| The local engine does not run | Code execution is not enabled, or `numpy` is missing | Enable code execution, or use the hosted tool for a small file |
| Rows missing from `rows_used` | Amount was zero or negative, the date did not parse, or the customer was blank | Check `data_quality_report` and the source rows |
| Markov migration listed under `refusals` | The file covers under about 3 months, so transition counts were zero | Tiers and the call list can still be used. Migration needs a longer history. |
| Two firms you know are one account | The rules left them in `review_candidates` | Re-run with `identity_overrides` on the hosted tool, or `OVERRIDES` in the script |

## FAQ

**Do I need a data scientist or a statistician?**
No. A user asks in plain language. The agent returns a call list. The models run inside the tool.

**What is the least data I need?**
Purchase rows with a customer, a date, and an amount. Ten or more customers, and five who have bought more than once, are what the survival model needs before it will fit.

**Does my data leave my environment?**
Not with the local engine. With the hosted tool, the rows in the call are sent to Saifs AI for the calculation. You can turn the hosted tool off. What the hosted service keeps after the call is not confirmed on this page yet.

**Will it change anything in my systems?**
No. It returns a ranking and a call list. It does not write to a CRM, an accounting system, or Dataverse.

**My CRM already has an MCP server. Why add this?**
That server gives the agent your CRM's records. This connector adds customer segmentation, the churn reading, and the call list, and it works on rows from any source.

**Why do I get the same answer every time?**
The same purchases and settings run the same calculations, including a fit with fixed starting points and a book-level range with a fixed seed. The agent's sentences around the numbers can vary.

**What happens if my data is incomplete?**
The tool still runs. It applies the documented defaults, lists them under `money_assumptions`, and withholds any model it cannot support. The reason is in `metadata.refusals`.

**Can I check how a result was produced?**
Yes. Each account has an `explanation_trace`. `metadata.model_version` and `layers_run` say which engine ran and which steps completed.

**How large a file can it handle?**
The hosted tool is meant for about 500 purchase rows. Larger files use the local engine. A 10,000-row file is a local-engine job.

**Can it run on a schedule?**
Yes, by calling it from a scheduled flow. The tools themselves run only when called. Set `as_of` to the run date if the list should move each day.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment). Separate host licensing may apply.

## Support

**Customer Intelligence - Saifs AI** is published by Saifs AI.

- Website: [https://saifs.ai](https://saifs.ai)
- Learn more: [https://saifs.ai/customer-intelligence](https://saifs.ai/customer-intelligence)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Privacy: [https://saifs.ai/privacy_policy/customer-tiering](https://saifs.ai/privacy_policy/customer-tiering)
- Terms: [https://saifs.ai/eula](https://saifs.ai/eula)
- Sign-in: OAuth 2.0, authorization code with PKCE, issuer `https://multiplewords.in`. No API key.
- Endpoint: `https://customer-tiering-mcp-microsoft.saifs.ai/mcp`

<!-- TODO Saifs AI: the privacy URL above still describes Facebook login, Stripe, and Monedata. Replace or rewrite it before publication so it matches this connector. -->
