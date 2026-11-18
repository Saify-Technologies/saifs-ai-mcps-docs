# Marketing Intelligence & Optimization

[SAIFS AI](https://saifs.ai) (`saifs.ai`) publishes this as a remote MCP server under the registry name `ai.saifs/marketing-intelligence`. It turns spend, sales, order baskets, A/B counts, and early adoption into ranked decisions: stop, scale, ship, or wait.

People search for this as marketing mix modeling (MMM), marketing attribution, A/B test significance, market-basket analysis, or budget optimization. The questions are “which ads work,” “where to move budget,” and “is this test real.” The short install page is the [server README](../README.md). This page is what a channel contribution means, why last-click is not that number, and how to send the exports.

| | |
|---|---|
| **Registry name** | `ai.saifs/marketing-intelligence` |
| **Endpoint** | `https://marketing-analysis-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## What it is

Each part of the analysis runs only when you send its data. Spend and sales produce a marketing-mix model. Baskets produce affinities and bundles. Variant counts produce a test verdict. Early sales of a new product produce a Bass curve. Acquisitions and revenue produce CAC, LTV, and payback by channel. Missing a block simply leaves that block out. It does not get filled in.

The reply is a ranked list of decisions.

| Card | What it is saying |
|---|---|
| **STOP** | This channel is past saturation, or its incremental contribution does not cover its spend. |
| **SCALE** | This channel still has room before diminishing returns, and the incremental return is positive. |
| **SHIP** | This bundle, or this new-product ramp, is supported by the data you sent. |
| **WAIT** | The A/B test, or the adoption series, is too thin or was read too early. Do not call a winner yet. |

One call to `marketing_intelligence` covers up to 200 rows in each array. A Meta, Google, or Shopify export larger than that uses `marketing_intelligence_get_engine`: a numpy-only Python script you run where the file already is.

The call is stateless. Send the latest weeks when you re-run. Nothing is stored. A mix-model result is labelled directional. It is an observational split of sales across the spend you sent, with a confidence interval. It is not proof that a rupee in that channel caused the sale.

Against a last-click or a gut split, this kind of reallocation is associated with cutting wasted spend on the order of 10–25%. That is a range across accounts, not a promise for one brand.

Who the customers are, and whom to call, is [Customer Intelligence & Segmentation](../../customer-intelligence/README.md). A single CAC or LTV from numbers you already have is the [Business Calculator](../../business-calculator/README.md).

## Why last-click and a peeked test mislead

A marketing team can see spend and a last click. The last click is not the channel that made the sale. Spend carries over into later weeks, and the next rupee in a channel returns less than the first. Those two facts, adstock and saturation, are what make a mix model more than a regression of this week’s spend on this week’s sales. A language model can fit one line from a prompt. It will not re-fit when you send another month, keep the transform version on the trace, and refuse to call an A/B test that was stopped the moment it looked good.

The same gap shows up in the catalogue. Products that share a basket are not a guess about what “goes with” what. And a landing-page test with a small gap and a small sample is often noise. Power is part of the verdict because stopping early is how false wins get shipped.

## How to use it

Connect the endpoint in the [install README](../README.md) and sign in when the client asks. Export spend from the ad platforms, orders from the shop, and test counts from analytics. For baskets, send product ids in each order. Do not send the customer’s name.

```json
{
  "spend": [
    {"week": "2026-10-05", "channel": "meta", "amount": 80000},
    {"week": "2026-10-05", "channel": "email", "amount": 12000}
  ],
  "sales": [
    {"week": "2026-10-05", "amount": 640000}
  ],
  "baskets": [
    ["sku-14", "sku-22"],
    ["sku-14", "sku-22", "sku-9"]
  ],
  "ab_test": {
    "name": "landing-page",
    "a": {"visitors": 1200, "conversions": 48},
    "b": {"visitors": 1180, "conversions": 61}
  }
}
```

A growth lead’s card, when the mix model finds one channel past saturation, reads like this: Meta is past the flat part of its curve; moving about ₹2 lakh toward email is associated with a higher return, on the order of 14% in that fit, and the interval is on the card. The 14% is the shape of a SCALE line. Your weeks produce their own interval, and the line is labelled directional.

A landing-page test that has not reached its sample comes back **WAIT**: the gap is not yet distinguishable from noise, and the card says how much more data the power calculation still wants. “Three more days” is that kind of sentence when your daily traffic makes the shortfall about three days. It is not a reason to keep peeking and stop on the first green day.

### Channel contribution

Sales in week t are split as

```text
Sales_t = β₀ + Σ βᵢ · f(Spendᵢ,t) + seasonality + trend + ε
```

`f` is not the raw spend. Adstock spreads a week’s spend into the following weeks, because an ad is not forgotten on Monday. Saturation bends the curve so the next rupee returns less than the last. The coefficients are fit with a penalty so channels that move together, which they usually do, do not each get a wild share. The trace names the model version. `explain` on a channel is that trace: the transform, the incremental contribution, the ROI, and the interval.

Reallocation respects the constraints you send: a floor on a brand channel, a cap on a prospecting channel, a total budget. The recommended move is the shift along that saturated curve. It is not a causal experiment. If you need a causal read, run the A/B test and send the counts.

### Baskets and bundles

For itemsets in the orders you send, Apriori (Agrawal, 1993) reports

```text
support(X)     = P(X)
confidence(X→Y) = P(Y | X)
lift(X→Y)       = P(Y | X) / P(Y)
```

Lift above 1 is a pair that occurs together more often than the two products’ separate popularities would suggest. Lift near 1 is two popular products that happen to share baskets. Rules below the support threshold are dropped, so a thin file does not fill up with pairs that occurred twice. `recommend` for one product is the other side of the rules that clear that bar. **SHIP** a bundle only when lift and support both clear. Otherwise the card is absent, not a guessed bundle.

### A/B tests

A two-proportion z-test, or a t-test when the outcome is a continuous amount, returns the difference, its confidence interval, the p-value, and the statistical power. Power is the chance the test would have detected an effect of the size you care about. A significant p-value on an under-powered test is still **WAIT**.

Peeking is checked. Looking every day and stopping when the p-value first drops below 0.05 inflates false winners. If the counts you send are consistent with an early stop, the verdict says so and does not ship the variant. Let the test run to the sample the power calculation named, then send the counts once.

### A new product

Bass (1969) fits an adoption curve to early sales:

```text
f(t) / (1 − F(t)) = p + q · F(t)
```

p is innovation, the sales that happen without social proof. q is imitation, the sales that follow people who already bought. The curve is an S. With only a handful of periods the two coefficients are not separated, and the card is **WAIT**. A fitted p and q, with a band, is the ramp you can plan inventory and spend against. It is not a promise of the S.

### Prompts

- “Which channel gives the best ROI?”
- “Is our landing-page test result real?”
- “What products sell together?”
- “Here's our weekly Meta, Google and email spend with sales. Which channel should we scale or cut?”
- “Is our landing-page A/B test win real, or is it under-powered?”
- “Which products should we bundle, based on these order baskets?”

## Tools

Channel ROI, budget moves, bundles, test verdicts, the Bass curve, and CAC / LTV / payback are results inside `marketing_intelligence`. They are not separate tool names.

| Tool | What you send | What comes back | Do not use it when |
|---|---|---|---|
| `marketing_intelligence` | Up to 200 rows in each of spend, sales, baskets, an A/B test, adoption, acquisitions, and customer revenue | Ranked STOP / SCALE / SHIP / WAIT cards, with channel contribution, budget moves, bundles, a test verdict, and unit economics for the blocks you sent. Mix-model numbers carry an interval and a directional label | An array is longer than 200 rows, a basket file is too thin to clear the support threshold, or you want a causal proof from observational spend. Use the local script for the long file. A thin test comes back WAIT |
| `marketing_intelligence_get_engine` | The request for a larger Meta, Google, or Shopify export | A numpy-only Python script you run locally. The rows stay on your machine | Every array already fits in 200 rows |

## How to read a result

- **Read the card, then the interval.** STOP and SCALE are about incremental contribution after adstock and saturation, not about last-click share.
- **Directional means not causal.** Two channels that were always bought together cannot be cleanly split. The interval widens, and the trace says the channels move together. Do not brief a board that the model proved the sale.
- **WAIT is a result.** An under-powered or peeked test does not get a winner. Add the sample and send the counts again.
- **Lift near 1 is not a bundle.** Support has to be large enough that the pair is a real habit of the catalogue.
- **Bass on a short series is WAIT.** p and q need the early ramp, not two data points.
- **CAC, LTV, and payback** appear when you sent acquisitions and revenue. They describe the channel’s unit economics. They do not discount future cash. A discounted lifetime value is a different calculation.
- **Re-run by sending the new weeks.** The server does not keep last month’s attribution.

## Data and what this will not do

Order and spend exports can contain personal data. Send channel totals and product ids. The server processes the request and does not store it, and it does not call an ad platform unless you have already exported the rows. Each user signs in with OAuth. Your own duties under India’s Digital Personal Data Protection Act, the GDPR, or any other law that applies to the export stay with you. Past 200 rows in an array, the script runs beside the file.

The server will not treat a mix model as causal proof, will not ship an under-powered or peeked A/B test, and will not invent a bundle below the support threshold. It will not remember last month’s model. Send the history again.

Privacy policy: https://saifs.ai/privacy_policy

## Plans

| Tier | What it covers |
|---|---|
| **Free** | An A/B verdict and basket rules on up to 1,000 transactions |
| **Pro** | ₹4,100 / $49 per month — one marketing-mix model, the full basket analysis, and A/B tests with power |
| **Business** | ₹16,500 / $199 per month — multi-channel mix models, budget reallocation, adstock and saturation tuning, Bass forecasts, and ad-platform export pulls |

The free tier is enough to check one test and a small catalogue with no sales call. One server call still stops at 200 rows per array. Above that, use the local script. Current terms are on [saifs.ai](https://saifs.ai).

## Support

- Install and client setup: [server README](../README.md)
- Who the customers are, and whom to call: [Customer Intelligence & Segmentation](../../customer-intelligence/README.md)
- A single CAC, LTV, or payback from figures you already have: [Business Calculator](../../business-calculator/README.md)
- Website: https://saifs.ai
- Registry: https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
