<!-- DRAFT for review. Do not replace intro.md until this file is accepted.
This folder has no mcptools JSON. Tool names, the 200-row split, and defaults below are from the connected Marketing Intelligence tool description. intro.md and manifest.json still say 500 rows. This draft follows 200. Confirm which limit the Microsoft endpoint enforces before you publish.

FIELD BLOCK
Title (22 characters): Marketing Intelligence
Description (276 characters):
Turns spend, sales, orders, tests, and acquisition rows into ranked stop, scale, ship, or wait actions. The same file and settings return the same actions. Channel results are a guide from history, not proof that ads caused sales. It does not change your ad or store accounts.

Operation summary (23 characters): Stop scale ship or wait
Operation description:
Ranks what to stop, scale, ship, or wait on from spend, sales, orders, tests, or acquisition rows. The same file and settings return the same actions.
-->

# Marketing Intelligence - Saifs AI

**Marketing Intelligence - Saifs AI** helps sales and marketing leaders **decide what to stop, scale, ship, or wait on**. You send the rows the question needs. It returns ranked actions with a money figure and a plain confidence. The same file and the same settings return the same actions.

It is built for teams that already have ad spend, store sales, orders, a test, or an acquisition export, and want a briefing they can rerun.

| If you are | Start with |
| --- | --- |
| A marketing, sales, or growth lead | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [Where the data comes from](#where-the-data-comes-from) |
| An administrator or security reviewer | [Data handling and security](#data-handling-and-security), [Security and compliance](#security-and-compliance), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [Where the data comes from](#where-the-data-comes-from), [For developers](#for-developers) |
| A finance or audit reviewer checking the method | [Inside the calculation](#inside-the-calculation) |

## The problem it solves

Most teams still move budget from last-click, a one-week spike, or a guess. A channel that has stopped paying for itself keeps its spend. A test with too little traffic gets shipped. A bundle is picked because it sounds right, not because people buy the two products together.

Marketing Intelligence makes that work available to any AI agent:

- **What to do with a channel**: stop, scale, or shift, from spend by date and channel plus sales by date.
- **What to bundle**: products that show up in the same orders.
- **Whether a test is ready**: ship, or keep it running, from trial and conversion counts.
- **How a launch may ramp**: an early scenario from units by period. Not a promise of market size.
- **Whether acquisition pays back**: what a customer is worth against what you paid, when you send acquisitions and revenue.

A layer runs only when its rows are in the call. Spend alone is not a channel read.

## Why choose it: the same file, the same actions

The reason to use this connector is that the actions are fitted to your rows. Ask again with the same file and the same settings and you get the same stop, scale, ship, or wait list. The agent does not invent a return or a winner.

**Channel results are a guide, not proof.** They are a reading of spend and sales history. They do not prove that ads caused the sales. If a channel is unclear, say so. Do not dress it up as a win.

**A test that is too small does not get a winner.** The action is to keep it running. If the team has looked more than once, pass that count as `peeks`.

**It does not change your accounts.** It does not edit Ads Manager, a store admin, or Dataverse. People still move the budget.

## Who should use this

| Role | How it helps |
| --- | --- |
| **Marketing and sales executives** | A ranked list for the week, with money and a plain confidence |
| **Growth leads** | See which channel still returns, and which looks tired |
| **Merchandisers** | Pairs that customers actually buy together |
| **Performance marketers** | A test verdict that waits when the test is not ready |
| **Agencies and RevOps** | The same method on CSV exports, without a separate analytics suite |
| **Agent makers** | Call a tool instead of summing spend divided by sales in the agent |

**Best fit:** D2C, retail, marketplace, subscription, and agency teams that have spend plus sales, or orders, or a test with counts.

**Not required:** a data-science team or an API key. A Saifs AI account, OAuth 2.0 sign-in, and the rows for the question you are asking are required.

Customer A/B/C/D tiers from purchases alone are a different connector. This one does not build that queue.

## Use cases

**1. Where the next amount of spend should go**
Scenario: weekly spend by channel and weekly sales are in two exports.
Outcome: ranked channel actions. Unclear channels are not called winners.
Ask: "Which channel should we cut or scale from this spend and sales file?"

**2. Hold total spend flat and change the mix**
Scenario: the team will not add budget. They can move it.
Outcome: a shift inside `max_reallocation_fraction` (default 0.30). The default budget is last-period total spend.
Ask: "If we keep total spend the same, where should we move it?"

**3. Products to put together**
Scenario: order lines have an order id and a product.
Outcome: pairs people buy together. A file of single-item orders cannot produce pairs.
Ask: "Which products should we bundle from these orders?"

**4. Is this test ready to ship?**
Scenario: control and variant each have a trial count and a conversion count.
Outcome: ship, or keep the test running. An under-powered test does not get a winner.
Ask: "Is this test real, or should we keep it running? We looked three times."

**5. How a new product may ramp**
Scenario: at least four periods of units for one product.
Outcome: an early ramp scenario. Before the peak, treat the total as a scenario, not a market-size promise.
Ask: "How might this launch ramp from these weekly units?"

**6. Does paid acquisition pay back?**
Scenario: new customers by channel, and later revenue by customer.
Outcome: what a customer is worth against what you paid. Default `gross_margin_rate` is 1, which is revenue, not margin. Set it below 1 when you want margin.
Ask: "Are we paying too much to acquire customers on paid social?"

## Agents you can build

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Weekly media agent | Ranks stop, scale, and shift from spend and sales | Spend by date and channel, and sales by date. Add a price or discount column when you run promos | 1, 2 |
| Bundle agent | Lists products bought together | Order id plus product, one row per line | 3 |
| Test-read agent | Says ship or keep running | Trial and conversion counts, and how many times the team looked | 4 |
| Launch agent | Sketches a ramp from early units | Product, period, and units | 5 |
| Payback agent | Compares value and acquisition cost | Acquisitions and customer revenue. Set the margin rate if you have it | 6 |
| Large-file agent | Runs the same stack on a file over 200 rows | A code sandbox with numpy. The script stays in the sandbox. Rows are not pasted into the chat | Any |
| Scheduled briefing agent | Runs a known file on a cadence | A flow with a recurrence trigger. The connector itself has no schedule | 1 |

The tools run on request. A person still changes budgets and tests in their own systems.

## At a glance

| Item | Detail |
| --- | --- |
| Tools | `marketing_intelligence` (200 rows or fewer in every array). `marketing_intelligence_get_engine` (any array over 200 rows) |
| Minimum input | The rows for the layer you want. Nothing is required on the schema, and a layer with no rows does not run |
| Currency | Display only. Default `USD`. No conversion between currencies |
| Writes to your systems | None |
| Same file, next run | Same actions, if the settings match |
| Pricing | [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment) |

The folder's current intro and manifest say 500 rows. The connected tool description says 200. This page uses 200.

## Where the data comes from

You export the rows and the agent sends them, or points the local script at the file. This connector has not been certified against each system named below.

| Question | Export | Fields |
| --- | --- | --- |
| What to bundle | Orders, one row per line | `basket_id` (or order id) and `item` (or product or sku). Or one row with `items` |
| Which channel is paying for itself | Ad spend by day or week, plus store sales by day or week | Spend: `date`, `channel`, `spend`. Sales: `date`, `revenue` |
| Did a promo drive the week | A price or discount series on the same dates | `controls`: `date` plus a number such as discount depth |
| Is the test ready | The test tool's exposure and conversion counts | `control_trials`, `control_conversions`, `variant_trials`, `variant_conversions`, optional `peeks` |
| How a launch may ramp | Units by period for the new product | `product`, `period`, `units` |
| Does acquisition pay back | New customers, and later revenue | Acquisitions: `date`, `channel`, `customers`. Revenue: `customer_id`, `date`, `revenue` |

Last-click channel reports are not a substitute for spend plus sales. A one-row summary ("we spent X and made Y") is not a mix model.

## How it works

Two routes run the same stack. The hosted tool accepts the rows in the call. The engine tool returns a script. The rows stay in the sandbox on that second route, and they are not sent to this server.

### Inside the connector

1. The user asks what to stop, scale, ship, or wait on, and provides the rows or a file.
2. At 200 rows or fewer in every array, the agent calls `marketing_intelligence` at `https://marketing-analysis-mcp-microsoft.saifs.ai/mcp`.
3. Over 200 rows in any array, the agent calls `marketing_intelligence_get_engine`, saves the script, and runs it. Only the path and option lines at the bottom may change.
4. The agent leads with the ranked actions. Coefficients and test tables stay in the background unless someone asks how the number was produced.

To keep large files out of the chat, turn the hosted tool off for that agent and leave `marketing_intelligence_get_engine` on: open the agent's **Tools** tab, select this server, turn off **Allow all**, turn off `marketing_intelligence`, and leave `marketing_intelligence_get_engine` on. Do this on each agent.

### Inside the calculation

| Layer | What it uses | When it runs | What to tell the buyer |
| --- | --- | --- | --- |
| Baskets | Association rules | Order lines are present. Needs about 30 baskets, and baskets with more than one item | "Customers who buy X often also buy Y." Not proof that one product causes the other |
| Channel contribution | A mix model with carryover and saturation | Spend and sales, at least 30 periods at the chosen grain. Default grain is weekly | A guide for where to move money. Not proof that ads caused sales. Without a price or promo control, a sale week can look like an ads win |
| Budget shift | The fitted channel read, inside a move limit | You ask to move money. Default move is 30 percent of a channel. Default budget is last-period spend | A suggested mix. A person still changes the budget |
| Tests | A two-proportion read, power, and a peeking guard. An always-valid check is on by default | Trial and conversion counts | No winner when the test is too small. Extra looks make the bar harder |
| Launch ramp | A Bass-style fit | Product, period, and units, at least four periods | An early scenario, not a proven market size |
| Acquisition payback | Cohort value against acquisition cost | Acquisitions, customer revenue, or both. Spend when you want cost | Default margin rate is 1, so the value is revenue until you set a margin |

**What happens when an input is missing.** That layer does not run. The tool does not invent a return, a pair, or a winner. Other layers you did supply can still return.

**Defaults you can leave alone**

| Setting | Default |
| --- | --- |
| Currency | `USD` |
| Basket support / confidence / lift | 0.01 / 0.20 / 1.05 |
| Rules returned | 200 |
| Channel grain | Weekly. Seasonal cycle 52. Minimum periods 30 |
| Test bar | 0.05. Minimum power 0.80. Always-valid check on |
| Biggest one-step channel move | 0.30 |
| Margin rate | 1 (revenue, not margin) |
| Value window | 24 months. Annual discount 0.10 |
| Minimum customers before a channel value | 30 |
| Healthy value-to-cost ratio | 3 |
| Trend grain | Quarter |

## Data handling and security

| Question | Hosted `marketing_intelligence` | Local `marketing_intelligence_get_engine` |
| --- | --- | --- |
| Do the rows reach Saifs AI? | Yes. They are in the tool call | No. The tool description says marketing data does not reach this server on this path. The script runs in your sandbox |
| Where is the work done? | `marketing-analysis-mcp-microsoft.saifs.ai`, over HTTPS | In the sandbox. Numpy only |
| Where is the result stored in your tenant? | In the agent thread, unless you save it | `tmp/mi_run/marketing_intelligence_result.json` by default |
| How long does Saifs AI keep a hosted upload? | <!-- TODO Saifs AI: confirm retention and the audit log. --> | Not uploaded on this path |
| Write access to ad or store accounts | None | None |

**Sign-in.** OAuth 2.0 through Saifs AI. No API key is created for the maker to paste into the agent.

The hosted route is for 200 rows or fewer in every array. Above that, use the local script so the file is not pasted into the chat.

## Token usage and cost

Prices below use the rates published on 16 November 2026: **$1.25 per million tokens the model reads**, and **$10 per million tokens the model sends**. Rows the model sends to the tool are the expensive direction. Counts use the `o200k_base` tokenizer on spaced JSON. The decision objects are a representative shape, not a captured live result. A longer live result raises the read cost only. The local script is not in this folder, so its first-run token cost is not stated. Do not copy another connector's script cost onto this one.

One spend row is about 28 tokens to send. One sales row is about 21. One basket line is about 17.

| File | Hosted tokens and cost | Local first run | Local later run | Comparison |
| --- | --- | --- | --- | --- |
| 90 spend rows, 30 sales rows, 40 basket lines | About 3,800 sent and 400 read. About $0.04 | Script not measured | A short printed action list is about 20 tokens, under $0.01 | Hosted is a few cents. Use it at this size |
| 200 spend, 200 sales, and 200 basket lines | About 12,800 sent and 400 read. About $0.13 | Script not measured | Under $0.01 for the printed actions | Still a small hosted bill. This is the hosted cap in the connected tool description |
| Over 200 rows in any array | Not the hosted tool | Script not measured | Under $0.01 for the printed actions. The rows stay in the sandbox | Use the local engine. Do not paste the file into the chat |

Hosts that charge a fixed amount per tool call are not changed by these token figures. Choose the local engine for file size and for keeping the rows in the sandbox.

## Security and compliance

**Reviewed by Microsoft**

| Item | What it covers |
| --- | --- |
| Microsoft connector certification | Microsoft reviewed and tested this connector before listing it as a certified connector. The review covers the connector's function and packaging, not Saifs AI's internal systems. |

**Self-assessed by Saifs AI**

<!-- PUBLISH EACH ROW ONLY AFTER IT IS COMPLETED.

| Self-assessment | What it is | Where to read it |
| --- | --- | --- |
| Microsoft Publisher Attestation | Saifs AI's answers to Microsoft's questionnaire. Microsoft publishes the answers and does not verify them. | [link] |
| CSA STAR Level 1 | Saifs AI's completed CAIQ on the public STAR Registry. | [link] |
| CSA STAR for AI Level 1 | Saifs AI's completed AI-CAIQ on the same registry. | [link] |

-->

| Design fact | Detail |
| --- | --- |
| Files written to ad or store accounts | None |
| Hosted retention | <!-- TODO Saifs AI: confirm. --> |
| Passwords held | None. Sign-in is OAuth 2.0 |
| Channel and test results | A guide from the file. Not a lab proof. A small test does not get a winner |

**Independent audits.** Saifs AI does not yet hold SOC 2 or ISO 27001. For a security questionnaire, use the [contact page](https://saifs.ai/contact-us).

## Prerequisites

- A Saifs AI account with access to Marketing Intelligence
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- OAuth 2.0 sign-in when you create the connection
- The rows for the question you are asking
- For a file over 200 rows in any array: a code sandbox with numpy
- Access to Copilot Studio, Microsoft 365 Copilot, Azure AI Foundry, Power Automate, Power Apps, or Azure Logic Apps

## How to get credentials

No API key is created for you to copy. Create a connection and sign in with OAuth 2.0. If the connection expires, sign in again from the connection page.

## Get started with your connector

1. In Copilot Studio, open your agent and go to **Tools**, then **Add a tool**.
2. Search for **Marketing Intelligence** and select it.
3. Create a connection and complete OAuth 2.0 sign-in.
4. Add this to the agent's instructions: "For what to stop, scale, ship, or wait on, call the Marketing Intelligence tool. If every array has 200 rows or fewer, call marketing_intelligence. If any array is larger, call marketing_intelligence_get_engine and run the script as given. Change only the path and option lines. Lead with the actions and the money. Do not invent a return or a test winner. If a channel or a test is unclear, say wait."
5. Ask with a small file first: "Which channel should we scale from this spend and sales export?"

## For administrators

Enabling this connector gives the organisation one approved way to turn a marketing export into a rerunnable action list.

- **You choose who can connect it.** Tenant and environment permissions apply.
- **You keep the ad and store accounts as the system of record.** This connector reads a file the user supplies. It does not change those accounts.
- **You can rerun the file.** The same rows and settings return the same actions.
- **You do not get a causal proof.** Channel results are a guide. Small tests stay on wait.
- **You choose the route for large files.** The local script keeps the rows in the sandbox. The hosted tool sends them in the call.
- **You set data policy as usual.** Place the connector in the Business, Non-business, or Blocked group.
- **There is no API key to rotate.** Expired connections are renewed by signing in again.

**Review checklist**

- Confirm whether the hosted cap is 200 rows or the 500 rows written in the older intro
- Decide who may upload spend and sales files
- Assign the connector to the correct data policy group
- Review the privacy policy and terms
- Read the self-assessments once they are published

## For developers

Two tools. No field is required. A layer runs only when its array is present.

| Tool | When | Notes |
| --- | --- | --- |
| `marketing_intelligence` | 200 rows or fewer in every array | Returns the full result, including ranked decisions |
| `marketing_intelligence_get_engine` | Any array over 200 | Returns a script. Run it as given. Numpy only. Result file defaults to `tmp/mi_run/marketing_intelligence_result.json` |

Paths on the engine tool, all optional: `data_path`, `transactions_path`, `spend_path`, `sales_path`, `controls_path`, `ab_tests_path`, `adoption_path`, `acquisitions_path`, `customer_revenue_path`, plus `currency`, `currency_symbol`, `as_of`, and `gross_margin_rate`.

### Sample request

```json
{
  "spend": [{ "date": "2026-01-05", "channel": "Search", "spend": 1200 }],
  "sales": [{ "date": "2026-01-05", "revenue": 8000 }],
  "currency": "USD"
}
```

A channel read needs at least 30 periods. One week is a valid shape and is not enough history.

### Sample response (shortened)

<!-- TODO Saifs AI: replace this note with a shortened response from a real call. Do not publish invented actions or money. -->

A live shortened response is not included in this draft. Capture one small `marketing_intelligence` call before you publish the page. Lead with `decisions`.

## Known issues and limitations

- **200 versus 500.** This draft uses the connected tool's 200-row cap. The folder intro and manifest say 500. Confirm before publish.
- **Not causal.** Channel results are a guide from history.
- **Promos.** Without a control series, a discount week can be credited to whichever channel spent that week.
- **Baskets.** Single-item orders produce no pairs. Thin data should raise `min_support` rather than invent pairs.
- **Tests.** No winner when power is too low. Pass `peeks` when the team has looked more than once.
- **Launch totals.** Early estimates are scenarios.
- **Margin.** The default rate of 1 scores revenue, not margin.
- **Currency.** A label only. No conversion.
- **No write-back.** Budgets and tests stay in your own tools.
- **Script.** Do not rewrite it. Change only the path and option lines. The script is not in this folder, so its token cost is unmeasured.
- **Customer tiers.** Purchase-only A/B/C/D queues are out of scope.

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| The tool asks you to sign in again | The connection expired | Sign in again with OAuth 2.0 |
| No channel ranking | Spend without sales, or fewer than 30 periods | Send both series, on a shared calendar, with enough periods |
| A sale week looks like an ads win | No price or promo control | Add a control row for those dates |
| No bundles | Orders have one item, or too few baskets | Use multi-item orders. Do not invent pairs |
| A small test was treated as a win | The agent skipped the tool, or peeks were omitted | Call the tool and pass `peeks` |
| A large file fails or truncates | It was sent to the hosted tool | Use `marketing_intelligence_get_engine` |
| The local script fails | The script was edited above the path lines, or numpy is missing | Run the script as given. Install numpy |

## FAQ

**Do I need an analyst?**
No. Ask in plain language and attach the export. A person still changes the budget.

**What is the least I need to send?**
The rows for one layer. Spend alone is not enough for a channel read. Pair it with sales, or send orders, or send test counts.

**Does my data leave my environment?**
On the hosted tool, yes. On the local engine, the tool description says the rows stay in the sandbox and do not reach this server.

**Will it change anything in my ad or store accounts?**
No.

**We already have last-click. Why add this?**
Last-click is not a spend-and-sales read. This connector fits the actions to the file you send, and it returns the same actions when you rerun that file.

**Why might two runs differ?**
They should match when the file and the settings match. A different grain, margin rate, or peek count is a different run.

**What happens if a column is missing?**
That layer does not run. The tool does not invent the missing figure.

**Can I check how an action was produced?**
Yes. Ask for the supporting layer. The page should still lead with the action. A live trace is not pasted into this draft.

**How large a file can I send?**
200 rows in every array on the hosted tool, using the connected tool description. Larger files use the local script.

**Can it run on a schedule?**
Yes, from a scheduled flow. The connector runs on request.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment). A hosted book at the 200-row example above is about $0.13 in model tokens at the 16 November 2026 rates. Microsoft Premium connector licensing applies separately.

## Support

**Marketing Intelligence - Saifs AI** is published by Saifs AI.

- Website: [https://saifs.ai](https://saifs.ai)
- Learn more: [https://saifs.ai/marketing-intelligence](https://saifs.ai/marketing-intelligence)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Privacy: [https://saifs.ai/privacy_page](https://saifs.ai/privacy_page)
- Terms: [https://saifs.ai/terms](https://saifs.ai/terms)
- Sign-in: OAuth 2.0. No API key for the maker to paste.
- MCP endpoint: `https://marketing-analysis-mcp-microsoft.saifs.ai/mcp`

<!-- OPEN QUESTIONS
1. Hosted cap is 200 in the connected tool description and 500 in intro.md and manifest.json. Confirm the Microsoft endpoint.
2. Confirm hosted retention and the audit log.
3. Capture a shortened live response.
4. The local script is not in this folder, so its token cost is unmeasured.
5. Confirm excluded government and China clouds.
6. Publisher Attestation and STAR rows stay commented until completed and linked.
7. Privacy URL in this folder is https://saifs.ai/privacy_page and terms are https://saifs.ai/terms. Other Saifs connectors use /privacy_policy/... and /eula. This draft keeps this folder's URLs.
8. Pricing uses the same Marketplace URL as the other Saifs AI connector pages.
-->
