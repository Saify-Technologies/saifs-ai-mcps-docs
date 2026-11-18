# Quote Price Optimization (Win Probability)

[SAIFS AI](https://saifs.ai) (`saifs.ai`) publishes this as a remote MCP server under the registry name `ai.saifs/quote-pricing`. It reads your own won and lost quotes and recommends a price range for the next deal, with the chance of winning and the expected margin at each price.

People search for this as price optimization, quote pricing, win probability, CPQ pricing, bid pricing, or discount optimization. The question is “what price to quote.” The short install page is the [server README](../README.md). This page is what the range means, why a discount matrix leaves money on the table, and how to send a history.

| | |
|---|---|
| **Registry name** | `ai.saifs/quote-pricing` |
| **Endpoint** | `https://quote-pricing-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## What it is

A distributor, a manufacturer, a contractor, an IT-services firm, or a logistics company prices most deals one quote at a time. This server fits a win-probability curve to the outcomes you send, then picks the markup where expected margin is highest and returns the band around that peak where a rep still has room to negotiate.

Six tools cover the loop.

| Tool | What you ask |
|---|---|
| `fit_win_model` | Fit the curve to closed quotes, and give me a snapshot to store. |
| `price_recommendation` | For this deal, what range, what P(win), and what expected margin at each price? |
| `explain_price` | Why this range, and how the odds move if I go lower? |
| `tender_bid` | For a sealed bid against a stated number of competitors, what multiple of estimated cost? |
| `discount_leakage` | Where did margin disappear between list and pocket price, and did the win rate move? |
| `win_loss_report` | Win rate and margin by segment, price band, and team. |

The call is stateless. You keep `quote_model_snapshot` and send it with the next deal. Open quotes are not used to fit the curve. Only won and lost outcomes are.

The company sets the price. The range is decision support. On quoted business, working inside that range rather than a flat discount matrix is associated with about 1–4% more margin. That is a range across books, not a promise on one deal.

Who to call is [Customer Intelligence & Segmentation](../../customer-intelligence/README.md). If you are choosing a supplier, or filing a tender you must follow, that is [Procurement & Supplier Selection](../../procurement-and-supplier-selection/README.md). This server is the price on your own quote.

## Why gut discount wins the wrong deals

A price that is too high loses the deal. A price that still wins often leaves margin behind. Friedman (1956) put the objective as margin times the probability of winning, estimated from past bids, not as the highest price that feels safe and not as the lowest price that feels likely. Enterprise configure-price-quote engines do this for large companies. Most B2B teams still use a discount matrix.

A language model can sketch one curve from a prompt. It will not refit when you send this quarter’s wins and losses, refuse a segment that does not have enough closed quotes, and keep payment-term interest out of the pocket price.

## How to use it

Connect the endpoint in the [install README](../README.md) and sign in when the client asks. Export closed quotes from Salesforce, HubSpot, Dynamics, Zoho, or the sheet the team already keeps. Send commercial features of the deal only.

```json
{
  "quote_history": [
    {
      "quote_ref": "Q-4410",
      "date": "2026-09-12",
      "segment": "distributor-west",
      "customer_type": "repeat",
      "product": "valve-a",
      "qty": 200,
      "cost": 100,
      "list_price": 150,
      "quoted_price": 120,
      "outcome": "won",
      "competitor_count": 2
    }
  ],
  "deal": {
    "segment": "distributor-west",
    "customer_type": "repeat",
    "product": "valve-a",
    "qty": 180,
    "cost": 100,
    "list_price": 150
  }
}
```

Do not send a person’s religion, caste, gender, ethnicity, or any other protected attribute. Do not send prices that competitors exchanged with each other. The history is your quotes and your outcomes. `competitor_count` is how many rivals were in the deal, not their price list.

Under 200 closed quotes in the whole history, the reply is a rule of thumb and it says so. A segment with fewer than 30 closed quotes does not get its own curve. If the rest of the book is large enough, that segment borrows a pooled curve and the reply says it is pooled. Open quotes are left out of the fit either way.

### The curve and the range

Markup `m` is the quote over cost, or the discount from list, and `x` stands for the deal features you sent: segment, size, product, region, new or repeat. Win probability is a logistic curve,

```text
P(win | m, x) = 1 / (1 + exp(−(α + γ·x − β·m)))
```

A higher markup lowers the chance of winning. Expected margin at that markup is

```text
E(m) = m · cost · P(win | m)
```

The peak of E is the price that balances margin and the chance of getting it. The range returned to the rep is the set of prices where E stays within 95% of that peak, so a negotiation can move inside the band without giving up most of the expected margin. A single rupee with no band is not the result.

A distributor recommendation in that shape reads: quote about ₹118–124 a unit. At ₹120, the win odds are about half and expected margin is at its highest in the fit. Below about ₹112 the extra win chance adds little expected margin. Those rupees are an illustration of the sentence. Your history produces its own band.

`explain_price` names the segment, the size, the competitor count, and how P(win) moves if the price steps down. `fit_win_model` is what you run when the history has grown. Store the snapshot it returns and pass it to `price_recommendation` so the next deal does not refit from scratch. The server does not keep the snapshot.

### A sealed bid

`tender_bid` is Friedman’s model when you are bidding a tender you prepare yourself, including a GeM or CPPP bid the caller will submit. Against competitors whose bid-ratio distributions come from your own past tenders,

```text
P(win) = Π (1 − Fᵢ(bid))
```

Gates’s variant is the one used when you know how many competitors there are and not which firms they are. The reply is a multiple of your estimated cost, as a range. Four competitors often land near 1.06–1.09 times estimated cost in textbook illustrations. Your history sets the multiple. The server does not file the tender. If the tender’s own rule fixes the price or the evaluation, that rule wins. Procurement’s tender mode is the reminder on the buying side. This tool does not replace it.

### The waterfall

`discount_leakage` walks list price down to pocket price: invoice discounts, then off-invoice items such as freight and rebates. Payment terms that are really a financing charge are excluded. There is no interest calculation. Pocket price is what remains. The report splits the leak by rep, segment, and product line, and sets it next to the win rate. A sales head’s version of that report is three segments giving away about 4–6% with no gain in win rate. That is the shape of a leak worth stopping. A leak that did buy win rate shows up as a different line. `win_loss_report` is the same cut without the waterfall: win rate and margin by segment, price band, and team.

### Prompts

- “What price should we quote for this deal?”
- “Bid for this tender with 4 competitors.”
- “Where are we leaking discount?”
- “At ₹120 a unit, what are the win odds and the expected margin, and how far can I move before the expected margin falls off?”
- “Which segments discounted 4% or more without winning any more often?”

## Tools

| Tool | What you send | What comes back | Do not use it when |
|---|---|---|---|
| `discount_leakage` | Closed quotes with list, invoice discounts, and off-invoice items | The waterfall to pocket price, and where margin leaked by rep, segment, and product line | The “discount” is a financing charge for longer payment terms. Interest is out of this tool |
| `explain_price` | A quote id from a recommendation you just received | The segment, size, and competitor count behind the range, and how P(win) changes with price | You have not priced the deal yet |
| `fit_win_model` | Closed quotes (`won` or `lost` only), with cost, list, quoted price, and deal features | A `quote_model_snapshot` per segment, or a pooled model, or a stated rule of thumb | There are fewer than 200 closed quotes, or you included open quotes in the fit. Open quotes are dropped. A thin book is labelled as a rule of thumb |
| `price_recommendation` | The deal and a snapshot | A price range, P(win) and expected margin at each point inside it | The snapshot is from a segment that was only a rule of thumb and you want it briefed as a fitted curve. The label stays on the reply |
| `tender_bid` | Your estimated cost, the competitor count, and your own tender history | A bid-ratio range from Friedman or Gates | You are sending rivals’ prices that those rivals shared with each other, or the tender already fixes the price. Also do not use it to file the bid |
| `win_loss_report` | Closed quotes | Win rate and margin by segment, price band, and team | You want the price for one live deal. That is `price_recommendation` |

## How to read a result

- **The band is the recommendation.** The price where expected margin peaks sits inside a range that keeps 95% of that peak. Quote inside the band. A point below the band buys little extra chance of winning.
- **P(win) is from your outcomes.** It is not a promise that this buyer will accept. It moves when you send more wins and losses.
- **A rule of thumb is labelled.** Under 200 closed quotes, or a segment under 30 that could not borrow a pooled curve, do not tell the rep the number came from a fitted model.
- **A pooled segment says so.** Borrowed strength is not the same as 30 of that segment’s own deals.
- **Leakage without a win-rate gain** is the discount to take back. Leakage that raised the win rate is a trade you can see, not an automatic cut.
- **The company sets the price.** Nothing in the reply posts a quote or submits a tender.

## Data and what this will not do

Quote history can identify a customer. Send references and commercial fields. The server processes the request and does not store it. Each user signs in with OAuth. Your own duties under India’s Digital Personal Data Protection Act, the GDPR, or any other law that applies to the export stay with you.

The server will not price on a protected attribute, will not take prices that competitors exchanged among themselves, and will not compute interest on payment terms. It will not fit a curve on open quotes or on a book that is too small, and it will not hide that fact. It will not submit a GeM, CPPP, or any other bid.

Privacy policy: https://saifs.ai/privacy_policy

## Plans

| Tier | What it covers |
|---|---|
| **Free** | 300 quotes of history, and 20 recommendations a month |
| **Pro** | ₹4,100 / $49 per month — recommendations without that monthly cap, across 5 segments |
| **Business** | ₹16,600 / $199 per month — tenders, the discount waterfall, unlimited segments, and more than one currency |

The free tier is enough to fit a small closed-quote file and price a handful of deals, with no sales call. A file under 200 closed outcomes still comes back as a rule of thumb. Current terms are on [saifs.ai](https://saifs.ai).

## Support

- Install and client setup: [server README](../README.md)
- Who to call before you quote: [Customer Intelligence & Segmentation](../../customer-intelligence/README.md)
- A tender’s own award rule, and choosing a supplier: [Procurement & Supplier Selection](../../procurement-and-supplier-selection/README.md)
- Website: https://saifs.ai
- Registry: https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
