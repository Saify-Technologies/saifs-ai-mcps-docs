# Quote Price Optimization - Saifs AI

**Quote Price Optimization - Saifs AI** helps sales teams decide **what price to quote**. You send won and lost quote history, and the deal to price. It returns a price range, the win probability at each price, the expected margin at each price, and the reason for the range.

It is built for sales reps, inside-sales teams, sales managers, pricing analysts, bid and tender managers, and founders who approve discounts.

**Publisher:** Saifs AI
**Website:** [https://saifs.ai](https://saifs.ai)
**Pricing:** [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
**Contact and help:** [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
**Privacy:** [https://saifs.ai/privacy_policy/quote-price-optimization](https://saifs.ai/privacy_policy/quote-price-optimization)
**Terms:** [https://saifs.ai/eula](https://saifs.ai/eula)
**Sign-in:** Microsoft single sign-on (OAuth 2.0). No API keys.

<!-- TODO Saifs AI: publish https://saifs.ai/quote-price-optimization and add it here as Learn more. The page returned 404 on 18 November 2026. -->

| If you are | Start with |
| --- | --- |
| A business owner, sales lead, or founder | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [Where the data comes from](#where-the-data-comes-from) |
| An administrator or security reviewer | [Data handling and security](#data-handling-and-security), [Security and compliance](#security-and-compliance), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [Where the data comes from](#where-the-data-comes-from), [For developers](#for-developers) |
| A pricing or finance reviewer checking the method | [Inside the calculation](#inside-the-calculation) |

## The problem it solves

Most B2B prices are set quote by quote. Reps discount from habit, or from a fixed discount matrix that is wrong for many deals. Some quotes are lost because the price is too high. Many more are won at a price that left margin behind. The pricing calculation that balances margin and the chance of winning usually sits in an enterprise pricing engine. Everyone else uses a matrix.

Quote Price Optimization makes this quote pricing calculation available to any AI agent:

- **What price to quote**: a price range for the deal, not a single number, so the rep still has room to negotiate.
- **Win probability**: how the chance of winning changes as the price changes, learned from your own won and lost quotes.
- **Expected margin**: the margin you earn on average at each price, which is the margin times the win probability.
- **Discount leakage**: discount optimization through a waterfall from list price to the price you collect, by rep, segment, and product.
- **Bid pricing**: a sealed-bid recommendation from public bid results and the number of competitors.
- **Win and loss**: win rate and margin by segment, price band, and rep.

## Why choose it: the same answer every time

Quote Price Optimization is deterministic. The same quote history, the same deal, and the same settings produce the same price range, win probability, and expected margin. Results are calculated from established pricing models. They are not written afresh by a language model each time you ask.

That matters because a rep sends the number, and a manager approves the discount. They need a recommendation they can rerun and compare. The agent's wording around the numbers may vary. The numbers do not.

| What it does | Established method |
| --- | --- |
| Estimates the chance of winning at each price from your own won and lost quotes | Penalized logistic regression |
| Chooses the price range where expected margin is highest | Expected-margin maximization |
| Bid pricing for a sealed tender against a known or unknown set of competitors | Friedman bidding and the Gates bidding model |
| Discount optimization: where margin leaks between list price and the price you collect | Price-waterfall analysis |
| Withholds a fitted curve when closed-quote history is too thin | A stated rule of thumb, with the reason |

**It does not guess.** Under 200 closed quotes, or when a segment is too thin, the tool gives rules of thumb only and says so. It does not present a thin history as a fitted win probability.

**It recommends and does not act.** It never sends a quote, never updates a CRM or CPQ system, and never sets a list price for a catalog. The company sets the price.

## Who should use this

| Role | How it helps |
| --- | --- |
| **Sales reps and inside-sales teams** at distributors and manufacturers | A price range and the win probability at each point, while the quote is being written |
| **Sales managers** | Which discounts are given away with no gain in win rate |
| **Pricing analysts** | A win-probability model per segment, refit as new outcomes arrive, without building the regression |
| **Bid and tender managers** at contractors and logistics firms | A sealed-bid recommendation from public results and the competitor count |
| **Founders who approve discounts** | What a proposed price is likely to win, and what expected margin it gives up |
| **IT services, packaging, and industrial supply teams** | The same quote pricing calculation on exports from the CRM they already use |
| **Agent makers and developers** | A tested quote pricing calculation, so the agent does not invent the price or the win odds |

**Best fit:** distributors, wholesalers, manufacturers, contractors, IT services firms, logistics providers, and packaging companies that negotiate quote by quote and keep won and lost outcomes.

**Not required:** an enterprise pricing engine, a data science team, or a CPQ pricing project. A quote export with outcomes is enough to start.

## Use cases

Each use case below can be built as an agent. The agent types are listed in [Agents you can build](#agents-you-can-build).

**1. What price to quote**
Scenario: a rep has a new deal and a history of won and lost quotes.
Outcome: a price range, the win probability at each price, and the expected margin at each price.
Ask: "What price should we quote for this deal?"

**2. Judge a price already typed**
Scenario: the rep has a number in mind and wants it checked before sending.
Outcome: pass `quoted_price` on the deal. The result places that price on the curve.
Ask: "If we quote this price, what are the win odds and the expected margin?"

**3. Price a batch of deals**
Scenario: a morning queue of open deals, up to 200 on the hosted tool.
Outcome: a price range for each deal from the same history.
Ask: "What price to quote for each of these open deals?"

**4. Discount leakage**
Scenario: some segments discount harder than others, and win rate does not rise with it.
Outcome: discount optimization through a price waterfall, by rep, segment, and product.
Ask: "Where are we leaking discount?"

**5. Win and loss review**
Scenario: a sales head wants win rate and margin by segment, price band, and rep.
Outcome: a win and loss report from quotes that have an outcome.
Ask: "Show win rate and margin by segment and rep."

**6. Sealed tender**
Scenario: a bid manager faces a tender with a known number of competitors, or named bidders.
Outcome: bid pricing for the tender, using Friedman bidding or Gates bidding on public tender history.
Ask: "Bid for this tender with 4 competitors."

**7. Explain the range**
Scenario: a manager asks why the range sits where it does.
Outcome: the segment, deal size, competition, and how win probability changes with price.
Ask: "Why this price range for this deal?"

**8. Thin history**
Scenario: the book has fewer than 200 closed quotes, or a segment has fewer than 30.
Outcome: rules of thumb only, with a clear statement that the curve was not fitted.
Ask: "What price to quote from this short history?"

**9. Large quote exports**
Scenario: more than 20,000 quote rows, or more than 50,000 tender rows.
Outcome: the same calculation runs in the agent's own sandbox through the local engine.
Ask: "Price these deals from the full quote export."

## Agents you can build

Each agent is one agent in your agent host with this connector added as a tool. The last column shows what else to add.

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Quote price agent | Returns a price range, win probability, and expected margin for the deal in front of the rep | A quote export, or a data source from [Where the data comes from](#where-the-data-comes-from) | 1, 2 |
| Deal queue agent | Prices up to 200 open deals from one history | The same data source, with one row per open deal | 3 |
| Discount leakage agent | Shows discount optimization: the waterfall, and where a discount does not buy win rate | Off-invoice fields when you have them: rebate, freight, promo, cash discount | 4 |
| Win and loss agent | Reports win rate and margin by segment, price band, and rep | Quote history with outcomes | 5 |
| Tender bid agent | Recommends bid pricing for a sealed tender from public results and the competitor count | Public tender history, and your company name as it appears in that file | 6 |
| Price explanation agent | States why the range was chosen and how win probability moves with price | The same deal and history as the quote price agent | 7 |
| Large-export agent | Runs the local engine on a full CRM or CPQ export | Code execution on the agent host | 9 |
| CRM quote agent | Reads quotes from the CRM, calculates the range, and hands it back for a person to send | The CRM's own MCP server | 1, 2 |
| Model snapshot agent | Fits once, then prices later deals from `quote_model_snapshot` without resending the full history | A place to store the snapshot file in your tenant | 1, 3 |
| Scheduled review agent | Sends a weekly leakage or win and loss report without anyone asking | A flow with a recurrence trigger, and email or Teams | 4, 5 |

The tools run on request. Scheduling comes from the flow, not from the connector.

## At a glance

| Item | Detail |
| --- | --- |
| Tools | `quote_pricing` (hosted calculation) and `quote_pricing_get_engine` (local engine) |
| Jobs | `price_recommendation`, `explain_price`, `fit_win_model`, `discount_leakage`, `win_loss_report`, `tender_bid`. Omit `job` to run every job the inputs allow. |
| Minimum input | Quote history with outcomes, plus the deal to price. A snapshot from an earlier run can stand in for the history. |
| Hosted tool size | Up to 20,001 quote rows, 200 deals, and 50,000 tender-history rows |
| Larger files | Local engine, for exports over 20,000 quotes or 50,000 tender bids, or for files already in the sandbox. Needs `numpy`. |
| Writes to your systems | None |
| Thin history | Under 200 closed quotes, rules of thumb only |

## Where the data comes from

Quote Price Optimization does not connect to your systems. The agent brings the rows to it. The connector needs only rows. It has not been certified against each system named below.

| Data source | How the agent gets the data | Best route |
| --- | --- | --- |
| Spreadsheet or CSV export from a CRM or CPQ tool (for example Salesforce, HubSpot, Dynamics 365, or Zoho) | The user uploads the file to the agent | Local engine for large files, hosted tool within the size caps |
| A CRM that has its own MCP server | The agent reads quote rows with that server, then passes them here | Hosted tool up to the caps. Above them, export a file and use the local engine. |
| Dataverse tables | The Dataverse MCP server or the Dataverse connector | Same as above |
| SQL database, Excel workbook online, or SharePoint list | A connector on the same agent or flow | Same as above |
| A scheduled flow | A flow reads the source on a schedule and calls this connector | Hosted tool |
| Public tender results | A file of earlier bids: tender, bidder, amount, engineer's estimate, date | Either route, within the tender-history cap |

**Works alongside your CRM's MCP server.** A CRM's MCP server gives the agent the CRM's records and functions. This connector adds the quote pricing calculation. The CRM supplies the rows. Quote Price Optimization calculates the price range. A person sends the quote.

**What to pull from the source**

| Data in your system | Field it fills | Why it matters |
| --- | --- | --- |
| Quote or opportunity id | `quote_ref` | Keeps each quote distinct |
| Close or quote date | `date` | Orders history, and supports recency settings |
| Segment, customer type, product, region, rep | Those field names | The features of the win-probability model |
| Quantity, cost, list price | `qty`, `cost`, `list_price` | Margin and the price basis |
| Price quoted | `quoted_price` or `amount` | What you asked, and whether you won at that price |
| Outcome | `outcome`: won, lost, or open | Only won and lost quotes are used to fit. Open quotes are ignored for fitting. |
| Competitors on the deal | `competitor_count` | Competition as a feature. Not their private prices. |
| Off-invoice items | `rebate`, `freight`, `promo`, `cash_discount` | The discount waterfall |
| The deal to price | `deal` or `deals` | Same fields as history. Optional `quoted_price` to judge a proposed price. |
| Earlier fit | `quote_model_snapshot` | Prices new deals without resending the full history |
| Public tender | `tender` and `tender_history` | Sealed-bid recommendation |

**Headers.** Send CRM or CPQ headers as exported. `quoted_price` and `amount` are both accepted for the price. `engineers_estimate` and `reference` are both accepted on a tender. Pass `mapping` when a header must be forced to a standard field.

<!-- TODO Saifs AI: publish the header alias list the engine matches, from a live response, before adding a recognition table here. -->

**File formats for the local engine.** CSV, TSV, and JSON. Pass a path for quote history, deals, the snapshot, the tender, and tender history, in any combination you have. Save an Excel sheet as CSV first, or ask the agent to convert it in the sandbox.

## How it works

The connector offers two routes to the same calculation. The agent picks the route by the size of the data, and by whether the rows may leave your environment.

| | Hosted tool | Local engine |
| --- | --- | --- |
| Tool name | `quote_pricing` | `quote_pricing_get_engine` |
| Use when | Up to 20,001 quote rows, 200 deals, and 50,000 tender-history rows | Over 20,000 quotes or 50,000 tender bids, or when the files are already in the sandbox |
| What the agent sends | The rows and the settings | File paths and settings only |
| What comes back | Decision cards and the full result | A ready-to-run Python script |
| Where the calculation runs | Saifs AI endpoint | Your agent's code-execution sandbox |
| Where your quote data goes | To the Saifs AI endpoint for calculation | It is not sent to Saifs AI on this path |
| Requires | Nothing extra | An agent host with code execution, and `numpy` |

**Local engine, step by step**

1. The user uploads or points the agent to the quote file, and to deal or tender files if those are separate.
2. The agent calls `quote_pricing_get_engine` with the file paths and any settings.
3. The connector returns the calculation script. Save it as given. Do not shorten or rewrite it.
4. The agent runs the script in its sandbox against the files.
5. The script prints the decision cards and writes `quote_pricing_result.json` and `quote_model_snapshot.json` in the sandbox.
6. The agent reports the decisions to the user.

### Inside the connector

1. The user asks what price to quote, or asks about win probability, discount leakage, or a tender.
2. The agent chooses a tool and calls this connector, which passes the call to the Saifs AI MCP server at `https://quote-pricing-mcp-microsoft.saifs.ai/mcp`.
3. The server checks the signed-in account.
4. The hosted tool runs the engine on the rows and returns the result. The engine tool returns the same engine as a script, for the agent to run in its sandbox.
5. The agent reads the result and explains it in plain language. The numbers come from the engine. The wording comes from the agent.

### Inside the calculation

Both routes run the same engine. Omit `job` and it runs every job the inputs allow. Set `job` to run one of: `fit_win_model`, `price_recommendation`, `explain_price`, `tender_bid`, `discount_leakage`, `win_loss_report`.

| Step | What the engine does | Method |
| --- | --- | --- |
| 1. Read the rows | Accepts quote history, one deal or many, an earlier snapshot, and tender inputs. Applies `mapping` when you send it. | Field matching |
| 2. Keep closed quotes for the fit | Won and lost quotes are used. Open quotes are ignored for fitting. Expired quotes count as lost unless `expired_as` is set to open. | Stated rule |
| 3. Fit win probability | Estimates how the chance of winning changes with price and with deal features such as segment, size, product, and customer type. | Penalized logistic regression |
| 4. Thin data | Under 200 closed quotes, the result is rules of thumb only, and it says so. A segment below `min_segment_quotes` (default 30) is not given its own fitted curve. | Refusal threshold |
| 5. Expected margin | At each price, expected margin is the margin times the win probability. The recommended point is where that product is highest. | Expected-margin maximization |
| 6. Price range | The range is where expected margin stays within `margin_band` of its best. The default band is 0.95. | Band around the maximum |
| 7. Floors | The price stays at or above `min_margin` (default 0, so not below cost unless you allow it). It stays at or below list price unless `allow_above_list` is true. | Constraints you set |
| 8. Explain | States the segment, deal size, competition, and how win probability moves with price. | The fitted curve, read back |
| 9. Tender | From public bid results, recommends a bid. Uses your company name when you pass `our_name`. | Friedman bidding. Gates bidding when competitor identity is unknown. |
| 10. Waterfall | List price, invoice discount, then off-invoice rebate, freight, promo, and cash discount, down to the price collected. | Price-waterfall analysis |
| 11. Win and loss | Win rate and margin by segment, price band, and rep. | Counts and averages on closed quotes |
| 12. Snapshot | Returns an updated `quote_model_snapshot` so a later call can price without the full history. | The fitted model |
| 13. Decision cards | Plain-language actions, returned first. | Fixed wording filled with the calculated numbers |

**Why the answer does not change.** The fit is a fixed calculation. `seed` defaults to 7. The same rows and the same settings, including that seed, return the same numbers. The agent's wording around the numbers may vary.

**What happens when data is missing.** There is no invented win probability. Under 200 closed quotes the tool gives rules of thumb and says so. `min_quotes` defaults to 200 and cannot be set below 50. `min_segment_quotes` defaults to 30 and cannot be set below 10. Open quotes are not used to fit. If you omit lost quotes, the curve only sees wins, and the tool cannot learn the prices that failed.

<!-- TODO Saifs AI: confirm how `forget` (default 0.9) and `recency_half_life_days` change the fit, and add that sentence here. Do not describe them until the engine output shows the rule. -->

**Where the working is shown.** Decision cards come first. The price range carries win probability and expected margin at each price. The explanation states why the range was chosen. The snapshot is the model that produced them.

## Data handling and security

| Question | Local engine | Hosted tool |
| --- | --- | --- |
| Does quote data reach Saifs AI? | No. The tool states that no data reaches the server on this path. | Yes. The rows passed in the call are sent for calculation. |
| Where is it processed? | Your agent host's code sandbox | `quote-pricing-mcp-microsoft.saifs.ai`, over HTTPS |
| What happens to the data after the call? | It is not sent to Saifs AI | <!-- TODO Saifs AI: confirm retention. Do not claim the rows are discarded, or that only a hash is logged, until that is confirmed. --> |
| What does Saifs AI record? | That the script was requested, at minimum | <!-- TODO Saifs AI: confirm the audit and billing record for the hosted tool. --> |

**What "sandbox" means.** The sandbox is the code-execution environment of your own agent host, such as Copilot Studio or Azure AI Foundry. It belongs to your tenant, not to Saifs AI. Your host's data policies still apply to it.

**Use the local engine only.** If your policy requires that quote data never leaves your environment, turn off the hosted tool. In Copilot Studio, open the agent's **Tools** tab, select the Quote Price Optimization server, turn off **Allow all**, and switch off `quote_pricing`. Leave `quote_pricing_get_engine` on. The agent can then use only the local engine. Any tool added to the server later stays off by default. This setting is per agent.

**Sign-in.** Users sign in with Microsoft single sign-on through OAuth 2.0. No API key is created, shared, or stored. Saifs AI holds the account details needed for sign-in.

<!-- TODO Saifs AI: confirm that the account record is limited to sign-in and billing, matching the inventory listing, before adding "and billing" as a fact. -->

**No write access.** The connector has no action that creates, changes, or deletes a quote, opportunity, or price in any CRM, CPQ system, or Dataverse.

**The script is open to review.** The local engine script is Python and uses `numpy`. Your security team can read it before allowing it to run. Run it as returned. Do not rewrite it.

## Security and compliance

This section separates what an independent party has reviewed from what Saifs AI states about itself.

**Reviewed by Microsoft**

| Item | What it covers |
| --- | --- |
| Microsoft connector certification | Microsoft reviews and tests this connector before listing it. The review covers the connector's function and packaging, not Saifs AI's internal systems. |

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
| Customer quote rows stored | Not confirmed for the hosted tool. The local engine does not send quote rows to Saifs AI. |
| Passwords held | None. Sign-in is Microsoft single sign-on. |
| Write access to your systems | None |
| Protected attributes | Not used. Pass `protected_fields` for any extra field that must be left out. |
| Competitor prices | Private competitor prices are not used. Tender history is public bid results. |
| Local engine | Python script using `numpy`, returned in full so it can be read before it runs |

**Independent audits.** Saifs AI does not yet hold an independently audited certificate such as SOC 2 or ISO 27001. If your review needs a completed security questionnaire, request it through the [contact page](https://saifs.ai/contact-us).

## Responsible use

This connector recommends a price. It does not set one. A person at your company decides what to send.

- It does not use protected attributes. Name any extra field to exclude in `protected_fields`.
- It does not use competitors' private prices. Do not paste prices that rivals exchanged with you. Tender history must be public results.
- It does not price loans, interest, or payment-term financing.
- It does not set list prices across a catalog, and it does not forecast demand.
- Customer and rep fields can be personal data. Send the fields the model needs. Use a code instead of a person's name where your policy requires it.

## Token usage and cost

With the hosted tool, token use grows with every row the model sends. With the local engine, the rows stay in the sandbox. The model reads a script, not the file, so token use does not grow with the export.

<!-- TODO Saifs AI: measure tokens before filling the table below. Use a named tokenizer on three sizes (a small file, the hosted cap, and a file above the cap). Record tokens per quote row, tokens per result, and the fixed cost of the script. State the size where the hosted tool uses fewer tokens. Add a cost example at a dated reference price per million input tokens and per million output tokens. Do not publish estimated numbers. -->

| Quote file | Hosted tool | Local engine, first run | Local engine, later runs |
| --- | --- | --- | --- |
| Small file | Not yet measured | Not yet measured | Not yet measured |
| About 20,000 quotes | Not yet measured | Not yet measured | Not yet measured |
| Above 20,000 quotes | Not a hosted-tool path | Not yet measured | Not yet measured |

Until those figures are measured, choose the route by the limits, not by a token estimate.

- **Within 20,001 quote rows, 200 deals, and 50,000 tender rows:** the hosted tool is one call.
- **Over 20,000 quotes or 50,000 tender bids:** the local engine. It is the path the server provides.
- **Quote data must stay in your environment:** the local engine. Turn the hosted tool off.
- **The same file, many times in one session:** the local engine, after the script is in the sandbox.

**Hosts that bill per action.** Some agent hosts charge a fixed amount for each tool call and do not bill by token. On those hosts a token saving does not change the bill. The local engine may use one extra action to run the script. Choose it for file size and data handling.

## Prerequisites

- A Saifs AI account, created by signing in with Microsoft single sign-on
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Access to Copilot Studio, Microsoft 365 Copilot, Azure AI Foundry, Power Automate, Power Apps, or Azure Logic Apps
- Quote history with outcomes, and the deal to price. Cost and list price make the margin real.
- For the local engine: code execution enabled on the agent host

## How to get credentials

No keys or secrets are needed. When you create a connection, you are redirected to sign in with your Microsoft account and approve access. If the connection later expires, sign in again from the connection page.

## Get started with your connector

1. In Copilot Studio, open your agent and go to **Tools**, then **Add a tool**.
2. Search for **Quote Price Optimization** and select it.
3. Create a connection and sign in.
4. Add this to the agent's instructions: "For any question about what price to quote, win probability, expected margin, discount leakage, or a tender bid, call the Quote Price Optimization tool. Do not invent the price or the win odds yourself."
5. Provide quote history by pasting it, uploading it, or pointing the agent to a file. Include lost quotes, not only wins. Include the deal to price.
6. Ask in plain language, for example "What price should we quote for this deal?" or "Where are we leaking discount?"

Treat results as decision support. A person checks the range against commercial policy before the quote is sent.

## For administrators

Enabling this connector gives the organization one approved way to answer what price to quote, inside the agents your makers already build.

**Why this is an administrator decision.** Without a calculation tool, makers ask an agent to pick a price from a spreadsheet. A language model can give a different figure on each run, and the result cannot be rerun or compared. A governed connector replaces that with one method for every agent and team.

- **You choose who can connect it.** Tenant and environment permissions apply. Only users who can create a connection can call the tools.
- **You keep your system of record.** The connector recommends and explains. It does not send quotes or write a price back to any system.
- **You can check the number.** The same file and settings can be rerun. Thin history is reported as rules of thumb, not as a fitted win probability.
- **You control where data goes.** Turn off the hosted tool per agent to keep quote data in your environment.
- **You set data policy as usual.** Place the connector in the Business, Non-business, or Blocked group with Power Platform data loss prevention policies.
- **There are no secrets to rotate.** Expired connections are renewed by signing in again.

**Review checklist**

- Decide whether the hosted tool is allowed, or local engine only
- Confirm quote history is data your tenant is allowed to process
- Assign the connector to the correct data policy group
- Review the privacy policy and terms
- Read any self-assessment listed under Security and compliance once it is published
- Review the local engine script if your policy requires it

## For developers

No single field is marked required in the schema. A useful call sends `quote_history` and a `deal`, or a `quote_model_snapshot` and a `deal`. The response says what it could not fit.

### Quote history fields

| Field | Meaning | Needed for |
| --- | --- | --- |
| `quote_ref` | Quote or opportunity id | Distinct quotes |
| `date` | Quote or close date | Order and recency |
| `segment` | Segment. Default segment field name is `segment`. | A curve per segment |
| `customer_type`, `product`, `region`, `rep` | Deal features | The win-probability fit |
| `qty`, `cost`, `list_price` | Quantity, unit or deal cost, list price | Margin |
| `quoted_price` or `amount` | The price that was quoted | What was asked, and the outcome at that price |
| `discount` | Discount given, if you store it | The waterfall |
| `outcome` | `won`, `lost`, or `open` | Fitting uses won and lost only |
| `competitor_count` | How many competitors were on the deal | A feature. Not their prices. |
| `rebate`, `freight`, `promo`, `cash_discount` | Off-invoice amounts | Discount optimization and the leakage report |

`quote_history` accepts up to 20,001 rows on the hosted tool.

### Deal fields

`deal` is one deal. `deals` is a list of up to 200 deals. Use the same fields as history. Pass `quoted_price` when you want a proposed price judged.

### Tender fields

| Field | Meaning | Needed for |
| --- | --- | --- |
| `tender.engineers_estimate` or `reference` | The reference estimate | The bid ratio |
| `tender.our_cost` | Your cost for the tender | Margin on the bid |
| `tender.competitors` or `competitor_count` | Named bidders, or how many | Friedman bidding or Gates bidding |
| `tender.date` | Tender date | Which history applies |
| `tender_history` | Public results: tender id, bidder, bid amount, engineer's estimate, date. One row per bidder per tender. Up to 50,000 rows. | The bid distribution |
| `our_name` | Your company's name as it appears in `tender_history` | Separating your past bids from others |

### Options and defaults

| Option | Default | Purpose |
| --- | --- | --- |
| `job` | Omit to run every job the inputs allow | One of `fit_win_model`, `price_recommendation`, `explain_price`, `tender_bid`, `discount_leakage`, `win_loss_report` |
| `price_basis` | `auto` | `markup` (over cost), `discount` (from list), or `auto` |
| `segment_field` | `segment` | Which column is the segment |
| `min_quotes` | 200 (minimum 50) | Closed quotes required before a fitted model |
| `min_segment_quotes` | 30 (minimum 10) | Closed quotes required before a segment is fitted on its own |
| `margin_band` | 0.95 (range 0.5 to 0.999) | How close to the best expected margin the price range must stay |
| `min_margin` | 0 (range -0.5 to 10) | Floor on margin. Default does not go below cost. |
| `allow_above_list` | false | When false, the price does not go above list |
| `expired_as` | `lost` | Count expired quotes as lost, or as open |
| `date_order` | None | `DMY` or `MDY` when dates are ambiguous |
| `recency_half_life_days` | None | Optional. Must be greater than 0 when set. |
| `forget` | 0.9 (range 0 to 1) | Weight setting for older history. Confirm the exact rule in the engine output. |
| `currency_symbol` | Empty | Display label only, such as `$` or `Rs`. No conversion. |
| `as_of` | None | Analysis date |
| `seed` | 7 | Leave it unchanged when you need the same numbers again |
| `protected_fields` | None | Field names the model must not use |
| `mapping` | None | Header to standard field |
| `min_win_odds` | 0.05 (range 0 to 0.5) | Lowest win probability to keep in the listed prices |
| `max_listed` | 200 (range 1 to 100,000) | Cap on listed rows |

### Sample request

```json
{
  "quote_history": [
    {
      "quote_ref": "Q-1001",
      "date": "2026-01-15",
      "segment": "Industrial",
      "customer_type": "repeat",
      "product": "Widget A",
      "qty": 100,
      "cost": 80,
      "list_price": 140,
      "quoted_price": 120,
      "outcome": "won",
      "competitor_count": 2
    },
    {
      "quote_ref": "Q-1002",
      "date": "2026-02-02",
      "segment": "Industrial",
      "customer_type": "new",
      "product": "Widget A",
      "qty": 80,
      "cost": 80,
      "list_price": 140,
      "quoted_price": 130,
      "outcome": "lost",
      "competitor_count": 3
    }
  ],
  "deal": {
    "segment": "Industrial",
    "customer_type": "repeat",
    "product": "Widget A",
    "qty": 90,
    "cost": 80,
    "list_price": 140,
    "competitor_count": 2
  },
  "currency_symbol": "$",
  "as_of": "2026-11-18"
}
```

This sample is below 200 closed quotes, so a live call returns rules of thumb, not a fitted curve. That is the behavior to expect from a short history.

### Sample response (shortened)

<!-- TODO Saifs AI: replace this note with a shortened response from a signed-in call to quote_pricing. On 18 November 2026 the endpoint returned 401 without an account, so no response body is invented here. -->

The server describes the return in this order:

| Section | Use it for |
| --- | --- |
| Decision cards | The first thing to read. Plain-language actions. |
| Price range | The range to quote, with win probability and expected margin at each price |
| Explanation | Why the range was chosen |
| Tender bid | Friedman bidding or Gates bidding, when tender inputs were sent |
| Discount waterfall and leakage | List price down to the price collected, and where margin leaks |
| Win and loss report | Win rate and margin by segment, price band, and rep |
| Updated snapshot | `quote_model_snapshot` to reuse on a later call |

**It does not guess.** With the two-row sample above, expect rules of thumb and a statement that the history is too short. Do not treat that result as a fitted win probability.

### Local engine paths

| Path argument | File |
| --- | --- |
| `quote_history_path` | Quote history CSV, TSV, or JSON |
| `deals_path` | Deals file |
| `quote_model_snapshot_path` | Snapshot from an earlier run |
| `tender_path` | The tender to bid |
| `tender_history_path` | Public bid results |
| `params` | Any other input the hosted tool accepts, including `deal` and `job` |

## Known issues and limitations

- **Decision support only.** It does not send a quote or update any system. The company sets the price.
- **Hosted tool size.** 20,001 quote rows, 200 deals, and 50,000 tender-history rows. Over 20,000 quotes or 50,000 tender bids, use the local engine.
- **Thin history.** Under 200 closed quotes, the result is rules of thumb only. A segment under the default of 30 closed quotes is not fitted on its own.
- **Open quotes.** They are ignored when the model is fit. A book of open quotes only cannot train a win probability.
- **Lost quotes.** If lost quotes are missing, the curve cannot learn the prices that failed.
- **Not list pricing.** It does not set list prices across a catalog, and it does not forecast demand. Bid pricing uses public tender results only.
- **Not lending.** It does not price loans, interest, or payment-term financing.
- **Protected attributes.** They are not used. Private competitor prices are not used.
- **Currency.** `currency_symbol` is a display label. There is no conversion between currencies.
- **Above list and below cost.** Off unless you set `allow_above_list` or a negative `min_margin`.
- **Regions.** Not available in US Government (GCC, GCC High, DoD) or China (21Vianet) clouds.

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| The tool cannot run, or the server returns 401 | The connection is missing or has expired | Open the connection and sign in again |
| Rules of thumb, and no fitted curve | Fewer than 200 closed quotes, or the segment is under `min_segment_quotes` | Add won and lost outcomes, or accept the rule of thumb and the stated reason |
| Open deals do not change the fit | Open quotes are ignored for fitting | Fit on closed quotes. Pass open deals in `deal` or `deals` to be priced. |
| The agent gives a price without calling the tool | Agent instructions do not require the tool | Add the instruction in Get started |
| The local engine does not run | Code execution is off, or `numpy` is missing | Enable code execution, or use the hosted tool within the caps |
| A header is read as the wrong field | The export name is not the standard field | Pass `mapping` for that header |

## FAQ

**Do I need a pricing specialist to use this?**
No. A user asks what price to quote. The agent returns a price range. The models run inside the tool.

**What is the least data I need?**
Won and lost quotes, plus the deal to price. Cost, list price, segment, and competitor count make the expected margin and the win probability more specific. Under 200 closed quotes you get rules of thumb.

**Does my data leave my environment?**
Not with the local engine. With the hosted tool, the rows in the call are sent to Saifs AI for calculation. You can turn the hosted tool off.

**Will it change anything in my systems?**
No. It does not send quotes or write prices back. A person sets the price.

**I already have a CRM MCP server. Why add this?**
That server gives the agent your CRM's records and functions. This connector gives the agent the quote pricing calculation, and it works on rows from any source.

**Why do I get the same answer every time?**
The same rows and the same settings return the same numbers. `seed` defaults to 7. The agent's wording around the numbers may vary.

**What happens if my data is incomplete?**
The tool still runs. It says when it is using rules of thumb. It does not fill a missing win probability with a fitted-looking number.

**Can I check how a result was produced?**
Yes. Read the decision cards, the price range with win probability and expected margin, and the explanation. Reuse `quote_model_snapshot` when you want the same model on the next deal.

**How large a file can it handle?**
The hosted tool accepts 20,001 quote rows, 200 deals, and 50,000 tender-history rows. Above 20,000 quotes or 50,000 tender bids, use the local engine.

**Can it run on a schedule?**
Yes. Call it from a flow with a recurrence trigger. The connector itself does not schedule.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment). Microsoft Premium connector licensing applies separately.

## Support

**Quote Price Optimization - Saifs AI** is published by Saifs AI.

- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Privacy: [https://saifs.ai/privacy_policy/quote-price-optimization](https://saifs.ai/privacy_policy/quote-price-optimization)
- Terms: [https://saifs.ai/eula](https://saifs.ai/eula)
- Website: [https://saifs.ai](https://saifs.ai)
- MCP endpoint: `https://quote-pricing-mcp-microsoft.saifs.ai/mcp`
- Sign-in: Microsoft single sign-on (OAuth 2.0). No API keys.

<!-- TODO Saifs AI: add Learn more at https://saifs.ai/quote-price-optimization once that page is published. -->
