# Customer Intelligence & Segmentation

[SAIFS AI](https://saifs.ai) (`saifs.ai`) publishes this as a remote MCP server under the registry name `ai.saifs/customer-intelligence`. It ranks a customer book from purchase history, flags who is slipping or ready to grow, and turns that into rupee ranges a sales manager can act on.

People search for this as customer segmentation, churn prediction, RFM analysis, customer lifetime value (CLV), ABC analysis, account prioritization, or customer retention. The questions are “who should we call today,” “who are the top customers,” and “which accounts are slipping.” The short install page is the [server README](../README.md). This page is what a tier means, why a label is not a decision, and how to send a book.

| | |
|---|---|
| **Registry name** | `ai.saifs/customer-intelligence` |
| **Endpoint** | `https://customer-tiering-mcp-latest.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## What it is

The input is the export every CRM, Tally file, and spreadsheet already has: `customer_id`, `date`, `amount`. One call returns three kinds of result.

| Kind | What it is for |
|---|---|
| A tier, A / B / C / D | Sorting the book |
| A probability, including P(alive) | How much confidence to put on that customer |
| A rupee range | What is at risk, what a contact is worth, and what an upgrade is worth |

The tier is for sorting. The queue is ranked by the rupee value of a contact, not by the letter. An A account that will buy anyway sits low. A B account that a call can still save or lift sits high. Each rupee figure is a range with the model’s confidence, never a single bare number: “about ₹4.2 lakh at risk (₹3.1–5.4 lakh, 90% interval).”

The call list the rep opens is a short SAVE / GROW / VERIFY list. Each row says why it is there today: newly at risk, a follow-up that is due, a no-answer to retry, or never contacted.

Two tools do this. `customer_tiering` scores a book of up to 500 transaction rows on the server. `customer_tiering_get_engine` is for a larger file: it returns a Python script you run locally, so those rows never come to the server. Name variants of the same customer are cleaned before scoring.

The call is stateless. Transactions, contact dates, and customer identifiers are computed in the request and discarded. The signed-in user is what billing and access use. If a response includes a model snapshot, you keep it and send it back next time so the book is not refit from scratch. The server does not keep the snapshot.

Revenue at risk is identified as a range. Teams that work the list see on the order of 2–8% of revenue retained from the top of the book.

A person or company with no purchases yet is a lead, not a dormant customer. That book belongs on [Lead Intelligence](../../lead-intelligence/README.md).

## Why a dashboard tier is late

Sales teams treat customers alike when nobody tells them who matters in a number they can act on. A dashboard that only says “A” or “C” does not say how many rupees are at stake. The accounts about to move between tiers are the moment a rep can still change the outcome. They stay invisible until the revenue chart has already moved.

A language model can sort a CSV once. It does not reliably fit a survival model, recompute as of a new date, keep a migration watch, and attach an auditable trace to both the tier and the rupee figure. The trace is the point: scores, the model version, the percentile, and the date the history runs through.

## How to use it

Connect the endpoint in the [install README](../README.md) and sign in when the client asks. Send purchase rows. Add contact state when you have it. Set `as_of` to the day the queue is for, and `capacity` to how many names the team can actually work.

```json
{
  "book": [
    {"customer_id": "c_882", "date": "2026-05-20", "amount": 48000},
    {"customer_id": "c_882", "date": "2026-04-02", "amount": 51000}
  ],
  "contact_state": [
    {"customer_id": "c_882", "last_contact": "2026-07-26", "outcome": "no_answer"},
    {"customer_id": "c_119", "last_contact": "2026-07-11"}
  ],
  "as_of": "2026-07-27",
  "capacity": 40
}
```

`contact_state` is optional, and `outcome` is optional inside it. Pass each customer’s last engagement date from the CRM. If you omit it, the queue falls back to presumed-contact modelling and the result says so, with lower confidence.

Ask for at least 24 months of history. If customers buy quarterly or more slowly, send 36 months. The window is counted in purchase cycles, not in calendar pages. One year is plenty for a weekly buyer and too short for a customer who orders twice a year.

| Rows | What to call |
|---|---|
| Up to 500 transaction rows | `customer_tiering`. The ledger, the rupee ranges, and the call list come back in that response. |
| More than 500 rows | `customer_tiering_get_engine`. Save the script it returns and run it on the file on your machine. |

Same inputs and the same `as_of` produce the same output. A later `as_of` changes the output even when the rows have not, because recency is measured against that date.

### What the queue does with contact state

For each customer the base rank is the value of a contact. A recent contact inside the cooldown for that outcome drops off the list, unless lapse risk has jumped. A promise to buy waits until the follow-up date and then returns to the top. An order placed recomputes the tier and leaves the queue. “Not interested” waits 30–60 days. “No answer” waits 1–2 days and can rotate channel or time. What remains is ranked by the adjusted value of contact, up to `capacity`.

Three modes are labelled on the result:

| `queue_mode` | When |
|---|---|
| `outcome_informed` | You sent contact state. Cooldowns use the outcomes. |
| `presumed` | You sent no contact state. The list is modelled from rank and elapsed days, and it is labelled as modelled. |
| `inferred` | A new order in the history, after a prior recommendation you passed back, is treated as evidence the contact happened. |

A small rotating share of eligible accounts, about 5–10%, is held out of the queue each cycle, matched on tier and value. The product says so. Membership rotates, so an account is not left off the list forever. The comparison is how often surfaced accounts converted against that matched control. That is the touched-versus-untouched check, and it does not need a CRM connection.

### Prompts

- “Who should my reps call today?”
- “Which of my top customers are going quiet?”
- “Tier my customers A to D.”
- “Here’s our sales export. Who are our A accounts, and who should we call this week?”
- “Which customers are going quiet and how much revenue is at risk?”
- “Why is customer c_882 a C account?”

## Tools

The published server exposes two tools. The ledger inside `customer_tiering` is where tier, explanation, migration, value at risk, contact value, and book exposure live. They are not separate tool names.

| Tool | What you send | What comes back | Do not use it when |
|---|---|---|---|
| `customer_tiering` | `book` rows (`customer_id`, `date`, `amount`), optional `contact_state`, `as_of`, `capacity` | Decision cards and a per-customer ledger: A/B/C/D, P(alive), rupee ranges, a SAVE / GROW / VERIFY list, and the trace for a tier | The file has more than 500 transaction rows, or the rows are people who have never purchased. Leads go to Lead Intelligence |
| `customer_tiering_get_engine` | The request for a book larger than 500 rows | A Python script you run locally on the file | The book fits in 500 rows. Use `customer_tiering` so you get the cards back in the chat |

## How a customer is scored

Six layers. The headline tier is predicted future value, not last year’s revenue. The older layers stay on the trace so a rep can see why.

### Pareto, as a baseline

Customers sorted by revenue. A is the cumulative share through 80%, B through 95%, C through 99%, D is the rest. The book’s Gini coefficient is reported as a concentration check. This layer is a sanity check beside the forecast. It is not the tier the queue uses.

### RFM

Recency, frequency, and monetary value, each in quintiles. Recent is a high recency score. The cell is the early warning on the trace: recency that fell from 5 to 2 in 60 days is visible even when the letter has not moved yet. Named cells include champions and at-risk accounts with high frequency and monetary value but collapsed recency.

### Is this customer still active?

BG/NBD (Fader, Hardie, and Lee, 2005; the Pareto/NBD line of Schmittlein, Morrison, and Colombo, 1987). While a customer is “alive” they purchase as a Poisson process. After a purchase there is a chance the relationship has gone silent. Those rates vary across the book. From frequency, recency, and the age of the relationship the fit returns P(alive) and the expected number of purchases over the next horizon.

The fit is numerical. Thin books, a flat likelihood, and overflow are reasons to refuse or to widen the range, not reasons to invent a confident tier. Below the history thresholds in the next section the tool says why and does not assign a tier.

### What they will spend

The Gamma-Gamma spend model (Fader and Hardie, 2013) blends the customer’s own average order with the book’s prior. Customer lifetime value is expected purchases times expected order value, discounted over the horizon. The tier ranks that predicted value: who is likely to be the cream, not only who was. The fit also reports the correlation between frequency and average order value. The spend model wants that correlation near zero. If it is not, the trace says so.

### Migration

States are A, B, C, D, and lost. A transition matrix is estimated from the book over time (Pfeifer and Carraway, 2000). Two matrices, one for accounts a rep has touched and one for accounts they have not, are the source of contact uplift. Applying the difference to the customer’s state, and valuing it with the tier’s lifetime value, is the rupee value of making the contact.

### The trace

The final tier is the predicted-value rank, annotated with the Pareto baseline, the RFM cell, P(alive), and migration risk. An explanation carries the trace, the model version, and the date the history was trained through.

## The rupee figures

These are arithmetic on the layers above. Each one is shipped as a range.

| Figure | What it answers |
|---|---|
| **Value at risk** | Expected lifetime value times the chance the relationship lapses if it is neglected. “Worth about ₹6 lakh a year, with a 35% lapse risk this quarter, so about ₹2.1 lakh at risk” is the shape of the sentence, and the reply still carries the interval. |
| **Value of contact** | Lifetime value times the difference between the chance of keeping the account if contacted and if not. This number ranks the daily queue. |
| **Upgrade upside** | The gap between lifetime value at the next tier and at the current tier, times the chance a contact produces that move. |
| **Book exposure** | Value at risk summed across neglected accounts, and upgrade upside summed across contactable B and C accounts. This is the figure a manager reports upward. |

## How much history is enough

The window is measured in that customer’s own purchase cycles.

```text
minimum window  >= 4 × that customer's median gap between purchases
preferred       >= 2 full seasonal cycles
```

| How they buy | Purchases in one year | A one-year file |
|---|---|---|
| Weekly | About 50 | Enough |
| Monthly | About 12 | Enough to tier. Thin for seasonality |
| Quarterly | About 4 | Marginal. Alive and lapsed are hard to tell apart |
| Twice a year, or yearly | 1 or 2 | Not enough to fit this model |

Two failures follow from cutting history at twelve months. A customer who orders every eight months looks lapsed. A nine-year relationship is modelled as if it started a year ago, and P(alive) depends on recency against the age of the relationship, so that truncation bends the probability.

What the tool does instead of scoring anyway:

| Purchase cycles in the window | Result |
|---|---|
| 4 or more | Full score |
| 2 or 3 | Scored, with a wider interval, flagged `LIMITED_HISTORY` |
| Fewer than 2 | No tier. The reason is `INSUFFICIENT_HISTORY`, with what to add |

Seasonal claims are reported only when two or more seasonal cycles are in the file. One December does not separate a seasonal peak from growth.

## When a customer leaves the daily list

P(alive) never hits zero. It fades. Dormant and lapsed are policy thresholds on top of the model, and both are shown with the evidence.

| Status | Rule |
|---|---|
| **Dormant** | P(alive) below 0.20 |
| **Lapsed** | P(alive) below 0.05, and the silence is longer than three times that customer’s own median gap between purchases |

Both conditions are required for lapsed. A slow buyer can have a low P(alive) and still be on their normal rhythm. The gap multiple is what stops that false call.

A lapsed customer stays in the book, in a lapsed band below D, and leaves the daily queue. They appear on a periodic win-back list, not on tomorrow’s call list. The history is kept because a later order is a reactivation, not a new stranger.

Someone with zero purchases is not lapsed. They are a lead. Do not send those rows here. A customer with exactly one purchase is scored weakly: status `PROVISIONAL`, and a wider interval.

The status line includes P(alive), days since the last purchase, the median gap, and how many of those gaps have elapsed. “Lapsed” without that line is not what the tool returns.

An order that has not appeared yet is not a failed contact. The recommendation can be in flight. Pending length follows the customer’s own median gap between purchases, plus a margin for how irregular they are. Thin history uses the segment’s median gap, then the book’s. While an account is pending it is not put back on the list and it is not judged. Each day `as_of` moves, P(alive) for a silent account decays, and value at risk rises, so the queue still changes on a quiet book.

## How to read a result

- **The letter is the sort. The range is the decision.** Quote the interval. A point estimate with no band is not what this server is for.
- **Value of contact outranks the letter** on the call list. An A with almost no uplift is not the first call.
- **`LIMITED_HISTORY` and `INSUFFICIENT_HISTORY`** are the product refusing to look more sure than the file. Do not paste a tier on top of them.
- **`PROVISIONAL`** means one purchase. Treat the range as wide.
- **`queue_mode`** tells you whether cooldowns were measured, presumed, or inferred.
- **The holdout** is disclosed. A name missing from a 40-person list can be in the rotating control group, not forgotten.
- **Pareto A is last year’s revenue.** The tier on the ledger is predicted value. When they disagree, the trace shows both.

## Data and what this will not do

Transaction history is personal data. Send customer ids you can join back on your side. The server processes the request and does not store the book. Each user signs in with OAuth. Your own duties under India’s Digital Personal Data Protection Act, the GDPR, or any other law that applies to the export, including consent, retention, and deletion, stay with you. A file over 500 rows is scored by the script on your machine.

This model is for repeat purchases that are not on a fixed contract schedule. A book that is only fixed-term subscriptions is a different problem, and a thin or mismatched file is labelled rather than forced into a tier. The server will not store contact state for you, will not call your CRM, and will not turn a lead with no purchases into a lapsed customer.

Privacy policy: https://saifs.ai/privacy_policy

## Plans

| Tier | What it covers |
|---|---|
| **Free** | Up to 250 customers, a monthly recompute, tier labels |
| **Pro** | ₹4,100 / $49 per month — up to 2,500 customers, a nightly recompute, migration alerts, explanations, value-at-risk ranges |
| **Team** | ₹12,400 / $149 per month — up to 10,000 customers, the uplift-ranked queue, book-exposure reports, CRM export pulls |
| **Consultant** | ₹24,900 / $299 per month — 10 books, white-label reports |

The free tier is the way to see A/B/C/D on a small export with no sales call. Value at risk and the ranked list are the step from Pro to Team. Current terms are on [saifs.ai](https://saifs.ai).

## Support

- Install and client setup: [server README](../README.md)
- People who have not purchased yet: [Lead Intelligence](../../lead-intelligence/README.md)
- Website: https://saifs.ai
- Registry: https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
