<!-- DRAFT for review. Do not replace intro.md until this file is accepted.
FIELD BLOCK (Microsoft validated fields). Confirm before submission.

Title (21 characters): Collections Optimizer
Description (422 characters):
Helps collections teams decide which overdue accounts to work, on which channel, and when. Send account ids and, when you have them, balance, days past due, and stop flags. Returns a work queue ranked by contact impact, accounts likely to pay without a call, accounts that must not be contacted, and roll-rate forecasts. It recommends a plan. It does not place calls or send messages. The same inputs return the same plan.

Operation summary (33 characters): Collections work queue and timing
Operation description:
Ranks overdue accounts for contact, channel, and time window, and keeps do-not-contact accounts off the queue. It does not place calls.
-->

# Collections Optimizer - Saifs AI

**Collections Optimizer - Saifs AI** helps collections and recovery teams decide **which overdue accounts to work, on which channel, and when**. You send today's overdue accounts, and payment history when you have it. It returns a work queue ranked by how much a contact changes the chance of payment, accounts likely to pay without a call, accounts that must not be contacted, a channel and time window, roll-rate and cure forecasts, and an assignment of accounts to agents.

It is built for teams that still work a book oldest-first or biggest-balance-first, and who need a daily plan they can rerun inside contact limits.

| If you are | Start with |
| --- | --- |
| A collections lead or recovery head | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [Where the data comes from](#where-the-data-comes-from) |
| An administrator, compliance, or security reviewer | [Responsible use](#responsible-use), [Data handling and security](#data-handling-and-security), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [For developers](#for-developers) |
| A compliance reviewer checking the method | [Inside the calculation](#inside-the-calculation) |

## The problem it solves

Most collections teams still work overdue accounts oldest-first or biggest-balance-first. That spends the day on accounts that would have paid anyway, or that cannot be collected, and it can miss the accounts a well-timed contact can still move. Contact outside permitted hours, or on an account that asked to stop, creates a separate harm.

Collections Optimizer makes that daily plan available to any AI agent:

- **Who to work today**: a queue ranked by how much a contact changes the chance of payment, not by balance or age.
- **Who to leave alone**: accounts likely to pay without a call, so agent time is not spent on them.
- **Who must not be contacted**: cease-contact, dispute, bankruptcy, deceased, legal hold, and hardship stay off the queue.
- **How to contact**: channel, a time window in the debtor's local time, and a tone tier, inside hour and frequency caps.
- **How the book will move**: roll-rate and cure forecasts from monthly snapshots.
- **Who should work it**: assignment by language, skill, capacity, and past success.
- **Whether a new strategy is better**: a stable champion-challenger split, when you set a test share.

## Why choose it: the same plan every time

Collections Optimizer is deterministic. The same overdue book, history, contact log, and settings produce the same work queue, the same do-not-contact list, the same channel and window, and the same roll-rate forecast. Results are calculated from fixed collections models. They are not written afresh by a language model each time you ask.

That matters because collections leads and compliance officers act on who is contacted and when. They need a queue they can rerun. The agent's wording around the numbers may vary. The numbers do not.

| What it does | Established method |
| --- | --- |
| Ranks accounts by how much a contact changes the chance of payment | Contact-impact ranking, not balance or age |
| Flags accounts likely to pay with no call | Self-cure threshold (default 0.80) |
| Chooses channel, window, and tone inside hour and frequency caps | Constrained contact policy. A looser policy is refused |
| Keeps stop-flagged accounts off the queue | Hard exclusion. Hardship is routed to the hardship team |
| Assigns accounts to agents | Assignment by language, skill, capacity, and past success |
| Forecasts how balances move between delinquency buckets | Roll-rate and cure forecast from monthly snapshots |
| Holds out a stable share so a strategy can be compared | Champion-challenger split. Default share is 0, meaning no test |

**It does not guess.** Thin history is reported as a gap rather than invented precision. If a stop flag is omitted, the tool cannot exclude that account, so include flags whenever they exist.

**It recommends and does not act.** It never places a call, never sends WhatsApp, SMS, email, or letter, and never writes to a loan system, dialer, or Dataverse. The creditor remains responsible for every contact and for legal review. The plan is not a compliance guarantee.

## Who should use this

| Role | How it helps |
| --- | --- |
| **Collections agents** at banks, NBFCs, MFIs, BNPL, telecom, and utilities | A morning queue: which accounts to work, which to skip, and the channel, window, and tone |
| **Collections team leads** | Assign accounts by language, skill, and past success, and compare a challenger strategy with the current one |
| **Recovery heads** | Roll-rate and cure forecasts, and a queue aimed at accounts a contact can still move |
| **Collections compliance officers** | Hour limits, frequency caps, and do-not-contact rules applied as hard constraints, with a plan that can be rerun |
| **B2B trade-credit and receivables desks** | Rank overdue invoices from an export instead of chasing the oldest balance first |
| **SMB finance leads** | A daily plan from an overdue export, without a collections-science team |
| **Agent makers and developers** | Add a tested collections plan to an agent, so nobody has to rank the book by hand |

**Best fit:** banks, NBFCs, MFIs, BNPL, telecom, utilities, and B2B trade-credit desks that hold a receivables book and decide each day whom to contact.

**Not required:** an enterprise collections platform. An overdue export with an account id is enough to start. Balance, days past due, stop flags, and payment history make the plan sharper.

This is decision support for the creditor's own collections operation. It is not a tool for raising contact volume, pressing people past the caps, or working accounts that asked to stop.

## Use cases

Each use case below can be built as an agent. The agent types are listed in [Agents you can build](#agents-you-can-build).

**1. This morning's work queue**
Scenario: a team lead uploads today's overdue accounts, and payment history if it exists.
Outcome: a ranked queue with channel, window, and tone, so the day is not spent oldest-first or biggest-first.
Ask: "Which overdue accounts should we work today, and on which channel?"

**2. Skip accounts that will pay anyway**
Scenario: some accounts cure without a call, and contacting them wastes time.
Outcome: those accounts are left off the call list when the chance they pay anyway is at least 0.80, unless you set another threshold.
Ask: "Which accounts are likely to pay without a call?"

**3. Honour do-not-contact and hardship**
Scenario: the book includes cease-contact, disputes, bankruptcy, deceased accounts, legal holds, or hardship.
Outcome: those accounts are not queued. Hardship is identified for the hardship team.
Ask: "Which accounts must not be contacted?"

**4. Stay inside calling hours and frequency caps**
Scenario: the book is in India, the United States, or a mix, and contact rules differ.
Outcome: India (`IN`) defaults to calls from 08:00 to 19:00 local and 3 calls in 7 days. The United States (`US`) defaults to 08:00 to 21:00, at most 7 calls in 7 days, and no call for 7 days after a conversation. Any other jurisdiction uses the stricter combination. A policy can only make these tighter.
Ask: "Plan today's contacts inside the calling caps."

**5. Roll rates for the next months**
Scenario: finance wants to see how the book may move.
Outcome: with monthly snapshots, roll-rate and cure forecasts for `forecast_periods` months (default 3, maximum 24).
Ask: "What are the roll rates for the next three months?"

**6. Assign the book to agents**
Scenario: several collectors are on shift, with languages, skills, and a cap on calls.
Outcome: accounts assigned within today's capacity. Language is a hard match.
Ask: "Assign today's queue across these agents."

**7. Compare a strategy without reshuffling**
Scenario: the team wants a stable test against the current approach.
Outcome: set `challenger_share` above 0 and keep `test_salt` the same every day, so an account stays in one arm. Default share is 0, which means no test. `no_contact` keeps a no-call group so the comparison stays measurable.
Ask: "Hold out a stable share so we can compare strategies."

**8. Explain one account**
Scenario: a reviewer asks why a named account was queued, skipped, or blocked.
Outcome: pass up to 50 account ids in `explain_accounts`. The result says where each account went and why.
Ask: "Explain why these account ids are on or off the queue."

**9. A large book**
Scenario: more than about 5,000 accounts, 20,000 history or snapshot rows, or 200 agents.
Outcome: the same calculation runs in the agent's sandbox, and debtor data stays there.
Ask: "Build the plan for this large overdue file in the sandbox."

## Agents you can build

Each agent is one agent in your agent host with this connector added as a tool. The last column shows what else to add.

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Morning queue agent | Answers which accounts to work, on which channel, and when | Today's overdue file, with stop flags | 1, 4 |
| Self-cure agent | Flags accounts likely to pay without a call | Payment history, when you have it | 2 |
| Do-not-contact agent | Keeps cease-contact, dispute, bankruptcy, deceased, legal hold, and hardship off the queue | Stop flags on every account that has them | 3 |
| Roll-rate agent | Forecasts how the book moves | Monthly snapshots | 5 |
| Agent assignment agent | Matches accounts to collectors by language, skill, and capacity | An agent roster for today | 6 |
| Strategy test agent | Holds a stable share in a challenger arm | The same `test_salt` every day, and observed `test_results` when you have them | 7 |
| Large-book agent | Runs the local engine on a full export | Code execution on the agent host | 9 |
| Loan-system agent | Reads overdue rows from the loan or billing system, calculates the plan, and hands it back for a person to contact | That system's own MCP server. Pass account ids, not names or phone numbers | 1, 3 |
| Scheduled morning agent | Sends the day's queue without anyone asking | A flow with a recurrence trigger, and email or Teams | 1 |

The tools run on request. Scheduling comes from the flow, not from the connector. The contact itself still happens in the creditor's dialer or messaging tools.

## At a glance

| Item | Detail |
| --- | --- |
| Tools | `collections_optimizer` (hosted calculation) and `collections_optimizer_get_engine` (local engine) |
| Minimum input | `account_id` on each row |
| Hosted tool size | About 5,000 accounts, about 20,000 history, contact-log, snapshot, or test-result rows, and about 200 agents |
| Returned queue | `max_queue` defaults to 200 and cannot exceed 5,000 |
| Larger files | Local engine, runs in your agent's code sandbox, needs only `numpy` |
| Writes to your systems | None. It does not place calls or send messages |
| Pricing | [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment) |

<!-- TODO Saifs AI: confirm this marketplace URL is the pricing page for this offer. -->

## Where the data comes from

Collections Optimizer does not connect to your systems. The agent brings the rows. The connector needs only rows. It has not been certified against each system named below.

| Data source | How the agent gets the data | Best route |
| --- | --- | --- |
| Overdue export from a loan, billing, or receivables system (for example a bank core extract, an NBFC or MFI workbook, a telecom or utility arrears file, QuickBooks or Tally receivables) | The user uploads the file | Local engine for large books, hosted tool for smaller ones |
| A loan or billing system that has its own MCP server | The agent reads account ids, balances, days past due, and stop flags, then passes the rows | Same size rule |
| Dataverse tables | The Dataverse MCP server or the Dataverse connector | Same size rule |
| A scheduled flow | A flow reads the book each morning and calls this connector | Hosted tool |
| Contact history and monthly snapshots | Separate files the agent passes as `history`, `contact_log`, and `snapshots` | Either route, within the row caps |

**Works alongside your loan system's MCP server.** That server gives the agent the book's records. This connector adds the daily plan. A person places every contact in the dialer or messaging tool. This connector does not.

**What to pull from the source**

| Data in your system | Field it fills | Why it matters |
| --- | --- | --- |
| Account identifier | `account_id` | The only required field. Do not send a name or a phone number |
| Amount overdue | `balance` | Values the account. It is not the sort order |
| How late it is | `dpd` or `due_date`, or `bucket` | Days past due, used in the plan |
| Promises | `broken_promises`, `kept_promises` | Recent promise behaviour |
| Recent contact | `calls_last_7_days`, `messages_last_7_days`, `last_conversation_date` | Caps and the wait after a conversation |
| Channel permission | `phone_valid`, `sms_ok`, `whatsapp_opt_in`, `email_valid` | Which channels are allowed. WhatsApp only with consent on file |
| Stop flags | `cease_contact`, `dispute`, `bankruptcy`, `deceased`, `legal_hold`, `hardship` | These accounts are never queued |
| Time zone | `utc_offset` | Debtor local time for the calling window |

Do not send names, phones, emails, addresses, PAN, Aadhaar, or SSN. Do not send gender, age, marital status, religion, or caste.

**Column names are recognised as they are.** `loan_id`, `customer_id`, `contract_id`, and `invoice_id` are read as `account_id`. `outstanding`, `amount_due`, and `overdue_amount` are read as balance. `days_past_due` and `days_overdue` are read as `dpd`.

**File formats for the local engine.** CSV, TSV, and JSON. Point `accounts_path` at today's book. Optional paths cover history, the contact log, snapshots, agents, and test results. The script writes `collections_plan.json`.

## How it works

The connector offers two routes to the same calculation. The agent picks the route by the size of the data.

| | Hosted tool | Local engine |
| --- | --- | --- |
| Tool name | `collections_optimizer` | `collections_optimizer_get_engine` |
| Use when | About 5,000 accounts, 20,000 related rows, and 200 agents, or fewer | Anything larger |
| What the agent sends | The rows and settings | File paths and settings only |
| What comes back | The full plan | A ready-to-run Python script |
| Where the calculation runs | Saifs AI endpoint | Your agent's code-execution sandbox |
| Where debtor data goes | To the Saifs AI endpoint for calculation | Nowhere. It stays in the sandbox |
| Requires | Nothing extra | An agent host with code execution, and `numpy` |

**Local engine, step by step**

1. The user uploads or points the agent to the overdue file.
2. The agent calls `collections_optimizer_get_engine` with the file paths and settings.
3. The connector returns the calculation script.
4. The agent runs the script unchanged in its sandbox.
5. The script prints the decision cards, the queue, and the do-not-contact list, and writes `collections_plan.json`.
6. The agent reports the plan. It does not place contacts.

### Inside the connector

1. The user asks which overdue accounts to work, on which channel, and when.
2. The agent chooses a tool and calls the connector, which passes the call to the Saifs AI MCP server over HTTPS at `https://collections-optimizer-mcp-microsoft.saifs.ai/mcp`.
3. The server checks the signed-in account.
4. The hosted tool runs the engine on the rows and returns the plan. The engine tool returns the same engine as a script.
5. The agent explains the plan in plain language. The numbers come from the engine. The wording comes from the agent.

### Inside the calculation

Both routes run the same engine.

| Step | What the engine does | Method |
| --- | --- | --- |
| 1. Read the book | Requires only `account_id`. Uses balance, days past due, promises, recent contact, and channel flags when present | Supplied fields, with recognised headers |
| 2. Exclude accounts that must not be contacted | Drops cease-contact, dispute, bankruptcy, deceased, legal hold, and hardship from the queue. Hardship is identified for the hardship team | Hard exclusion |
| 3. Rank the rest | Orders accounts by how much a contact changes the chance of payment, not by balance or age | Contact-impact ranking |
| 4. Self-cure | Skips a call when the chance they pay anyway is at least `self_cure_threshold` (default 0.80) | Self-cure threshold |
| 5. Channel, window, and tone | Picks call, WhatsApp, SMS, email, or letter only where that channel is allowed. WhatsApp requires `whatsapp_opt_in`. Tone is 1 friendly reminder, 2 firm reminder, or 3 formal notice | Constrained policy |
| 6. Caps | Applies the jurisdiction window and frequency cap. `IN` is 08:00–19:00 and 3 calls in 7 days. `US` is 08:00–21:00, at most 7 calls in 7 days, and 7 days after a conversation. Anything else uses the stricter combination. Digital messages default to 3 in 7 days. A policy can only tighten these. A looser value is refused | Jurisdiction defaults plus stricter-only `policy` |
| 7. Assignment | Matches accounts to agents by language (a hard match), skills, capacity, and past success. `minutes_per_call` defaults to 8 | Assignment within capacity |
| 8. Forecast | From monthly snapshots, forecasts roll rates for `forecast_periods` months (default 3, maximum 24) | Roll-rate and cure forecast |
| 9. Strategy test | If `challenger_share` is above 0, holds that share in one arm. The same `test_salt` (default `cc1`) keeps an account in the same arm from day to day. Default share is 0 | Champion-challenger split |
| 10. Queue cap | Returns at most `max_queue` accounts (default 200, maximum 5,000) | Fixed cap |

**Why the answer does not change.** The same inputs and settings return the same plan. A champion-challenger split stays stable when the same test salt is used. The agent's wording may vary.

**What happens when data is missing.** The engine still returns a plan and reports thin history. It does not block the call for a missing balance. It cannot apply a stop flag you did not send. `jurisdiction` defaults to `IN`. `as_of` defaults to the latest date in the data. Currency is not a conversion engine. `l2` (default 1) is a regularisation setting. Leave it unless you were asked to change it.

**Where the working is shown.** Pass account ids in `explain_accounts` (up to 50) to see where each account went and why.

## Data handling and security

| Question | Local engine | Hosted tool |
| --- | --- | --- |
| Does debtor data reach Saifs AI? | No | Yes, the rows passed in the call |
| Where is it processed? | Your agent host's code sandbox | `collections-optimizer-mcp-microsoft.saifs.ai`, over HTTPS |
| What happens to the data after the call? | It never left your sandbox | <!-- TODO Saifs AI: confirm retention for this server. --> |
| What does Saifs AI record? | Only that the engine script was requested, if your facts sheet confirms that | <!-- TODO Saifs AI: confirm the audit and billing log. --> |

**What "sandbox" means.** The sandbox is the code-execution environment of your own agent host. It belongs to your tenant, not to Saifs AI. Your host's data policies continue to apply.

**Use the local engine only.** If debtor data must never leave your environment, turn off the hosted tool. In Copilot Studio, open the agent's **Tools** tab, select the Collections Optimizer server, turn off **Allow all**, and switch off `collections_optimizer`. Leave `collections_optimizer_get_engine` on. This setting is per agent.

**Sign-in.** Users sign in with Microsoft single sign-on (OAuth 2.0). No API key is created, shared, or stored.

**No write access.** The connector does not place calls, send messages, or write to a loan system, dialer, or Dataverse.

Overdue accounts and contact history can be sensitive personal and financial data. Use only data you are authorised to process.

## Responsible use

A person makes the final contact decision. This connector does not.

- Do not contact outside the hours and caps the tool returns.
- Do not contact cease-contact, dispute, bankruptcy, deceased, legal hold, or hardship accounts. Hardship goes to the hardship team.
- Do not send protected or demographic attributes. No gender, age, marital status, religion, or caste.
- Send account ids only. No names, phones, emails, addresses, or government ids.
- WhatsApp is used only when consent is on file.
- The hour and frequency caps are hard limits. They are not a guarantee that conduct is lawful. Legal review stays with the creditor.
- This is not financial or legal advice. It is not a tool for increasing pressure past the caps.

## Security and compliance

This section separates what an independent party has reviewed from what Saifs AI states about itself.

**Reviewed by Microsoft**

| Item | What it covers |
| --- | --- |
| Microsoft connector certification | Microsoft reviewed and tested this connector before listing it as a certified connector. The review covers the connector's function and packaging, not Saifs AI's internal systems. |

**Self-assessed by Saifs AI**

Self-assessments are statements Saifs AI makes about its own practices. They are published so that you can check them, and they are not independent audits.

<!-- PUBLISH EACH ROW ONLY AFTER IT IS COMPLETED. Until then, keep the row inside this comment.

| Self-assessment | What it is | Where to read it |
| --- | --- | --- |
| Microsoft Publisher Attestation | Saifs AI's answers to Microsoft's questionnaire. Microsoft publishes the answers and does not verify them. Renewed every year. | [link] |
| CSA STAR Level 1 | Saifs AI's completed CAIQ on the public STAR Registry. Updated every year. | [link] |
| CSA STAR for AI Level 1 | Saifs AI's completed AI-CAIQ on the same registry. | [link] |

-->

| Design fact | Detail |
| --- | --- |
| Customer data stored | <!-- TODO Saifs AI: confirm hosted-tool retention. The local engine keeps rows in your sandbox. --> |
| Passwords held | None. Sign-in is Microsoft single sign-on. |
| Write access to your systems | None. No calls and no messages are sent |
| Contact limits | Hard caps. A policy can only make them tighter |
| Local engine | Plain Python, one dependency, no network calls, open to review |

**Independent audits.** Saifs AI does not yet hold an independently audited certificate such as SOC 2 or ISO 27001. If your review needs a completed security questionnaire, request it through the [contact page](https://saifs.ai/contact-us).

## Token usage and cost

With the hosted tool, token use grows with every row, because the model sends those rows out and reads a result back. With the local engine, the rows stay in the sandbox, so that row cost does not grow with the file.

| Book | Hosted tool: tokens and cost per run | Local engine: first run | Local engine: each later run | Comparison |
| --- | --- | --- | --- | --- |
| 25 accounts, 100 history rows | about 10,000 tokens, about $0.10 | Script not measured | about 140 tokens, under $0.01 | Hosted tool. One small call |
| 5,000 accounts, 20,000 history rows | about 1,885,000 tokens, about $18.76 | Script not measured | about 140 tokens, under $0.01 | Hosted call is expensive because every row is sent |
| 20,000 accounts, 80,000 history rows | Not the hosted route. Sending the rows alone is about 7,500,000 tokens | Script not measured | about 140 tokens, under $0.01 | Local engine only |

The returned queue is capped at 200 accounts by default, so the result the model reads grows more slowly than the book it sends. The do-not-contact list in this sample still grows with the book.

**Estimate your own file**

- **Hosted tool, to send:** about 171 tokens per account and 51 per history row.
- **Hosted tool, to read:** about 1,100 tokens on the 25-account sample, and about 9,700 tokens at 5,000 accounts with this result shape.
- **Local engine, later run:** about 140 tokens for a short decision print. It does not include the raw rows.

**Which route to choose**

- **A few dozen accounts:** the hosted tool. On this sample that is about $0.10.
- **A full book near 5,000 accounts:** the hosted call is about $18.76, because the rows are output tokens. Use the local engine so those rows are not sent.
- **More than about 5,000 accounts, 20,000 history rows, or 200 agents:** the local engine. It is the only route.
- **Debtor data must stay in your environment:** the local engine, with the hosted tool switched off.

**Hosts that bill per action.** Some agent hosts charge a fixed amount for each tool call and do not bill by token. On those hosts the measured token saving does not change the bill. Choose the local engine for file size and for keeping debtor data in your environment.

How these figures were measured: token counts use the `o200k_base` tokenizer on generated sample data, with rows written as JSON, on 16 November 2026. The same check on a 10-field inventory row is 78 tokens and a sales row is 26, in line with the Inventory page. Costs use $1.25 per million input tokens and $10 per million output tokens. Rows the model sends are output tokens. The result it reads is input tokens. Result records are representative objects from the documented fields, not a captured engine file, so a longer live result costs more to read. The local-engine script is not in this folder, so the first local run has no dollar figure. Do not copy the Inventory script cost onto this connector.

## Prerequisites

- A Saifs AI account, created by signing in with Microsoft single sign-on
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Access to Copilot Studio, Microsoft 365 Copilot, Azure AI Foundry, Power Automate, Power Apps, or Azure Logic Apps
- An overdue export with an account id per row, and authority to process it
- For the local engine: code execution enabled on the agent host

## How to get credentials

No keys or secrets are needed. When you create a connection, you are redirected to sign in with your Microsoft account and approve access. If the connection later expires, sign in again from the connection page.

## Get started with your connector

1. In Copilot Studio, open your agent and go to **Tools**, then **Add a tool**.
2. Search for **Collections Optimizer** and select it.
3. Create a connection and sign in.
4. Add this to the agent's instructions: "For any question about which overdue accounts to work, call the Collections Optimizer tool. Do not rank accounts yourself. Do not place calls or send messages. Honour every do-not-contact flag and every cap the tool returns."
5. Provide today's accounts with account ids only. Include stop flags whenever they exist.
6. Ask in plain language, for example "Which overdue accounts should we work today, and on which channel?"

Treat results as decision support. The creditor places every contact.

## For administrators

Enabling this connector gives the organisation one approved way to decide which overdue accounts to work, on which channel, and when.

**Why this is an administrator decision.** Without a calculation tool, makers ask an agent who to call from a spreadsheet. A language model can give a different list on each run, and it can recommend contact outside permitted hours or on an account that asked to stop. A governed connector applies one method and hard caps.

- **You choose who can connect it.** Tenant and environment permissions apply.
- **You keep your system of record.** The connector recommends a plan. It does not place calls or write back to any system.
- **You can check the queue.** The same file and settings can be rerun. `explain_accounts` shows why an account was queued or blocked.
- **You keep identity out of the request.** Account ids only. Demographic fields do not belong in the file.
- **You do not get a compliance guarantee.** Caps are hard limits. Legal review stays with the creditor.
- **You control where data goes.** Turn off the hosted tool per agent to keep the book in your environment.
- **You set data policy as usual.** Place the connector in the Business, Non-business, or Blocked group.
- **There are no secrets to rotate.** Expired connections are renewed by signing in again.

**Review checklist**

- Confirm stop flags will be included whenever they exist
- Decide whether the hosted tool is allowed, or local engine only
- Set jurisdiction and any stricter policy with compliance
- Assign the connector to the correct data policy group
- Review the privacy policy and terms
- Read the self-assessments under Security and compliance, once they are published
- Review the local engine script if your policy requires it

## For developers

Only `account_id` is required on each accounts row.

### Account fields

| Field | Meaning | Needed for |
| --- | --- | --- |
| `account_id` | Account id. Required | Everything |
| `balance` | Amount overdue | A sharper plan. Not the sort key |
| `dpd` or `due_date` | Days past due, or a due date the engine converts | Timing and bucket |
| `bucket` | `current`, `1-30`, `31-60`, `61-90`, `91-180`, `180+` | A bucket when days past due are absent |
| `broken_promises`, `kept_promises` | Promise history | The contact-impact read |
| `calls_last_7_days`, `messages_last_7_days`, `last_conversation_date` | Recent contact | Caps and the post-conversation wait |
| `utc_offset` | Hours from UTC, for example 5.5 | Local calling window |
| `phone_valid`, `sms_ok`, `whatsapp_opt_in`, `email_valid` | Channel permission | Which channel can be chosen |
| `language`, `product`, `segment` | Matching and labels | Agent assignment |
| `cease_contact`, `dispute`, `bankruptcy`, `deceased`, `legal_hold`, `hardship` | Stop flags | Exclusion from the queue |

### Other inputs (optional)

| Input | Meaning |
| --- | --- |
| `history` | Past accounts, plus `paid`, `contacted`, `channel`, and `recovered` |
| `contact_log` | `account_id`, `timestamp`, `channel`, `reached` |
| `snapshots` | One row per account per month: `period`, `dpd` or `bucket`, `balance`, `status` |
| `agents` | `agent_id`, `capacity`, `languages`, `skills`, `success_rate` |
| `test_results` | Observed arms: `arm`, `paid`, `recovered` |

### Options and defaults

| Option | Default | Purpose |
| --- | --- | --- |
| `jurisdiction` | `IN` | `IN`, `US`, or any other value for the stricter combination |
| `self_cure_threshold` | 0.80 | Skip a call at or above this chance of paying anyway |
| `max_digital_7d` | 3 | WhatsApp, SMS, and email cap in 7 days. Policy can only lower it |
| `minutes_per_call` | 8 | Used to size the queue from agent minutes |
| `max_queue` | 200 | Most accounts returned. Maximum 5,000 |
| `forecast_periods` | 3 | Months ahead. Maximum 24 |
| `challenger_share` | 0 | 0 means no test. Maximum 0.50 |
| `challenger_policy` | `balance_first` | Also `oldest_first`, `propensity_first`, `no_contact` |
| `test_salt` | `cc1` | Keep it stable so an account stays in one arm |
| `l2` | 1 | Regularisation. Leave it unless asked |
| `explain_accounts` | None | Up to 50 account ids to explain |

`policy` may set a later `call_start`, an earlier `call_end`, a lower call cap, fewer channels, or a lower `max_tone`. A looser value is refused.

### Sample request

```json
{
  "accounts": [
    {
      "account_id": "A-1001",
      "balance": 12500,
      "dpd": 34,
      "calls_last_7_days": 1,
      "phone_valid": true,
      "whatsapp_opt_in": false,
      "cease_contact": false,
      "hardship": false,
      "utc_offset": 5.5
    }
  ],
  "jurisdiction": "IN",
  "self_cure_threshold": 0.8,
  "max_queue": 50
}
```

### Sample response (shortened)

<!-- TODO Saifs AI: replace this note with a shortened response from a real call. Do not publish invented payment probabilities. -->

A live shortened response is not included in this draft. Capture one call before you publish the page. Keep the sample on account ids.

### Response sections

| Section | Use it for |
| --- | --- |
| Work queue | Whom to contact, by which channel, in which window, and in which tone |
| Self-cure list | Accounts left off the call list because they are likely to pay anyway |
| Do-not-contact list | Stop-flagged accounts, including hardship routed to the hardship team |
| Roll-rate and cure forecast | How the book may move, when snapshots were sent |
| Agent assignment | Who should work which accounts today |
| Champion-challenger | The stable split, when `challenger_share` is above 0 |
| Explanations | Where a named account went, and why |

## Known issues and limitations

- **Decision support only.** It does not place calls, send messages, or write to any system.
- **Not a compliance guarantee.** Caps are hard limits. They do not certify that conduct is lawful.
- **Do not loosen the defaults.** A policy can only start calls later, end them earlier, lower the caps, remove channels, or lower the tone. A looser value is refused.
- **Jurisdiction.** `IN` and `US` have the windows above. Any other value uses the stricter combination.
- **Stop flags must be supplied to be honoured.** If they are omitted, those accounts can be queued.
- **WhatsApp needs consent.** It is used only when `whatsapp_opt_in` is true.
- **Account ids only.** No names, phones, emails, addresses, government ids, or demographic attributes.
- **Thin history.** Short payment or contact history weakens the plan. Gaps are reported.
- **Hosted tool size.** About 5,000 accounts, 20,000 related rows, and 200 agents.
- **Queue cap.** Default 200 accounts in the returned list.
- **Regions.** Not available in US Government (GCC, GCC High, DoD) or China (21Vianet) clouds.

<!-- TODO Saifs AI: confirm the excluded-cloud sentence for this offer. -->

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| The tool cannot run, or asks you to sign in again | The connection has expired | Open the connection and sign in again |
| A policy value is refused | It is looser than the jurisdiction default | Tighten the window, the cap, the channels, or the tone |
| An account that asked to stop is on the queue | The stop flag was not sent | Include cease-contact and the other flags whenever they exist |
| WhatsApp was not chosen | `whatsapp_opt_in` is not true | Leave WhatsApp off unless consent is on file |
| The agent ranks the book without calling the tool | Instructions do not require the tool | Add the instruction in Get started |
| The local engine does not run | Code execution is not enabled | Enable it, or use the hosted tool for a small book |
| Not permitted, or a subscribe link | The account cannot call the tool | Paste the full Marketplace link exactly as returned |

## FAQ

**Do I need a specialist to use this?**
No. A user asks in plain language. The creditor still places every contact and keeps legal review.

**What is the least data I need?**
An account id on each row. Balance, days past due or a due date, and stop flags make the plan sharper.

**Does my data leave my environment?**
Not with the local engine. With the hosted tool, the rows in the call are sent for calculation. You can turn the hosted tool off.

**Will it change anything in my systems?**
No. It does not place calls, send messages, or write to a loan system, dialer, or Dataverse.

**Our loan system already has an MCP server. Why add this?**
That server gives the agent the book's records. This connector adds the daily plan and the contact caps.

**Why do I get the same answer every time?**
The same book and settings return the same plan. Keep `test_salt` unchanged if you are running a strategy test. The agent's wording may vary.

**What happens if my data is incomplete?**
The tool still runs and reports gaps. It cannot exclude an account whose stop flag you did not send.

**Can I check how a result was produced?**
Yes. Pass up to 50 account ids in `explain_accounts`.

**How large a file can it handle?**
About 5,000 accounts on the hosted tool. Larger books use the local engine.

**Can it run on a schedule?**
Yes, from a scheduled flow. The connector itself runs on request. It does not dial.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment). Microsoft Premium connector licensing applies separately.

## Support

**Collections Optimizer - Saifs AI** is published by Saifs AI.

- Website: [https://saifs.ai](https://saifs.ai)
- Learn more: [https://saifs.ai/collections-optimizer](https://saifs.ai/collections-optimizer)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Privacy: [https://saifs.ai/privacy_policy/collections-optimizer](https://saifs.ai/privacy_policy/collections-optimizer)
- Terms: [https://saifs.ai/eula](https://saifs.ai/eula)
- Sign-in: Microsoft single sign-on (OAuth 2.0). No API keys.
- MCP endpoint: `https://collections-optimizer-mcp-microsoft.saifs.ai/mcp`

<!-- OPEN QUESTIONS
1. Confirm the marketplace offer URL.
2. Confirm hosted-tool retention and the audit log.
3. Capture a shortened live response on account ids only.
4. Hosted send and read tokens are measured on generated JSON. The local-engine script is still not tokenized. A live result file would replace the representative result objects.
5. Confirm excluded clouds.
6. Confirm whether naming RBI and FDCPA in the schema should stay off the public page. This draft states the hour and frequency numbers and says they are not a compliance guarantee.
7. Publisher Attestation and STAR rows stay commented until completed and linked.
-->
