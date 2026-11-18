# Lead Intelligence

[SAIFS AI](https://saifs.ai) (`saifs.ai`) publishes this as a remote MCP server under the registry name `ai.saifs/lead-intelligence`. It scores an open pipeline: who is likely to convert, how long that may take, what a contact is worth, which touches show up on the deals that closed, and who to call today.

People search for this as lead scoring, pipeline forecasting, conversion prediction, sales prioritization, multi-touch attribution, or Shapley attribution. The questions are “which leads to call” and “pipeline forecast.” The short install page is the [server README](../README.md). This page is what a conversion probability means, why a 0–100 lead score does not say who to call, and how to send a pipeline.

| | |
|---|---|
| **Registry name** | `ai.saifs/lead-intelligence` |
| **Endpoint** | `https://lead-scoring-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## What it is

A lead id and a created date are enough to start. Stage history and a touch log make the score sharper. One call to `lead_pipeline` covers up to 500 leads. A larger export uses `lead_pipeline_get_engine`: a Python script that scores the file on your machine and does not call the network.

The ledger for each lead holds:

- P(convert) and an expected time to close, as a range.
- The chance of reaching Won from the current stage, and how long the lead tends to sit in that stage.
- Value of a contact, and value at risk if the lead goes cold, both as ranges.
- Channel credit across the touches you sent, labelled observational.
- A manager decision: **CALL TODAY**, **PUSH FORWARD**, or **QUALIFY**.

The queue is ordered by the value of a contact, not by a raw 0–100 score and not by which lead arrived last. An open lead is censored: not converted yet, not a loss. Treating every open lead as a failure is the mistake the survival model is there to avoid.

The call is stateless. Send the pipeline again when you recompute. Nothing is stored. Teams that work this queue, instead of newest-first, see conversion move on the order of 10–20% from the change in who gets the hour. That is a range across pipelines, not a promise for one rep.

Once a lead has bought, the book of customers is [Customer Intelligence & Segmentation](../../customer-intelligence/README.md). The price on a live quote is [Quote Price Optimization](../../quote-pricing/README.md). Which paid channel to scale, from spend and sales rather than from touch order, is [Marketing Intelligence & Optimization](../../marketing-intelligence/README.md).

## Why newest-first wastes the morning

A CRM score of 80 does not say whether a call changes the outcome, how long the deal may take, or which touch was present on the wins. The leads about to go cold are often not the newest ones. Survival analysis, a Markov reading of the funnel, and Shapley credit for touches have been in the literature for decades. They usually sit in an enterprise marketing platform. A language model can sort a CSV once. It will not treat an open lead as censored, refit the stage transitions when you send another month, or average a touch’s contribution across orderings without dropping to first-touch or last-touch.

## How to use it

Connect the endpoint in the [install README](../README.md) and sign in when the client asks. Export leads from HubSpot, Salesforce, Zoho, Dynamics, or a CSV. Send ids you can join back to the person on your side.

```json
{
  "leads": [
    {
      "lead_id": "L-1042",
      "created": "2026-09-01",
      "stage": "SQL",
      "deal_size": 400000
    }
  ],
  "stage_history": [
    {"lead_id": "L-1042", "stage": "MQL", "ts": "2026-09-04"},
    {"lead_id": "L-1042", "stage": "SQL", "ts": "2026-09-18"}
  ],
  "touches": [
    {"lead_id": "L-1042", "channel": "webinar", "ts": "2026-09-03"},
    {"lead_id": "L-1042", "channel": "demo", "ts": "2026-09-18"}
  ],
  "capacity": 40
}
```

`stage_history` and `touches` are optional. Without them you still get a score from age, stage, and deal size. With them you get stage timing and channel credit.

A manager’s line in this shape is: about ₹18 lakh of pipeline is stalling this week across 14 leads, and here is the ranked list. Those figures are the shape of a value-at-risk sentence. Your export produces its own range. Each rupee figure is a range. A bare point is not the result.

Too few closed deals, and the reply refuses a fitted curve and says what is missing. It does not invent a sharp P(convert) from a handful of wins.

### What the queue is ranking

The baseline sort is expected value: estimated deal size times P(convert). That is the cheap ranking, and it is not the call list.

The call list uses the value of a contact:

```text
Value_of_Contact = Expected_Pipeline_Value × [P(convert | contacted) − P(convert | not contacted)]
```

A lead that will close whether or not anyone calls sits lower. A lead whose chance moves if someone calls sits higher. Value at risk is the same expected value times the chance the lead goes cold if nobody calls this week. The pipeline figure a manager reports is the sum of those risks, as a range.

**CALL TODAY** is the top of that list, up to `capacity`. **PUSH FORWARD** is a lead whose hazard of converting is falling. **QUALIFY** is a lead the trace will not yet treat as ready for that list. The ledger shows which decision and why.

### Prompts

- “Which leads should SDRs call today?”
- “Forecast this quarter’s pipeline.”
- “Which channels drive conversions?”
- “Here's our HubSpot lead export. Who should the team call today?”
- “Which leads are going cold and how much pipeline value is at risk?”
- “Which channels actually drive conversions — email, demo or webinar?”

## Tools

Conversion probability, time to close, the stage outlook, channel credit, the rupee ranges, and the three decisions all come back from `lead_pipeline`. They are not separate tool names.

| Tool | What you send | What comes back | Do not use it when |
|---|---|---|---|
| `lead_pipeline` | Lead rows (`lead_id`, created date, stage, optional deal size), optional stage history and touches, and a capacity. Up to 500 leads | A per-lead ledger: P(convert), expected time, stage outlook, observational channel credit, value of a contact and value at risk as ranges, and CALL TODAY / PUSH FORWARD / QUALIFY | The file has more than 500 leads, the closed-deal history is too thin for a fitted curve, or the people have already purchased. Buyers are a customer book, not a lead. A thin history is refused with a reason |
| `lead_pipeline_get_engine` | The request for a pipeline over 500 leads | A Python script you run locally. It does not call the network. The rows stay on your machine | The pipeline already fits in one `lead_pipeline` call |

## The methods

### Open leads are censored

Cox’s model (1972) writes the hazard of converting at time t, given features x, as

```text
h(t | x) = h0(t) · e^(βᵀx)
```

P(convert by t) is 1 minus the survival function. Expected days to close come off the same curve. A lead that is still open contributes as “not yet.” Marking that lead lost because it has not closed would bias every probability down. Engagement features sit on x: days since the last touch, how often you have touched them, and depth such as a page, a download, or a demo. A book with too few closes does not get this fit.

### Expected value

```text
Expected_Pipeline_Value = P(convert) × expected deal size
```

When you supply a discount factor, that value is also discounted by the expected time to close. The factor is yours. The server does not compute interest, a loan, or a payment-term charge. Every rupee figure ships as a range.

### The funnel

Stages run Lead, MQL, SQL, Opportunity, Won, Lost, or the stage names in your export once they map onto that ladder. A transition matrix estimated from the history you send gives P(reach Won) from the current stage, and an expected time in the stage from how often leads stay put. Two matrices, one for leads a rep has contacted and one for leads they have not, are the source of the uplift in the value-of-contact formula. A cohort view of how won revenue may arrive over a horizon uses a Bass adoption curve on the cohort, not a sum of every open deal at full size.

### Channel credit

Shapley value (1953) gives each touch its average extra contribution to conversion, across orderings of the touches on that lead:

```text
φᵢ = Σₛ  |S|! · (n − |S| − 1)! / n!  ·  [v(S ∪ {i}) − v(S)]
```

The sum is over subsets that do not already contain touch i. When a lead has many touches, the average is estimated by sampling orderings rather than listing every one. The reply is labelled observational. It is not proof that the webinar caused the deal, and it is not a last-touch or a first-touch shortcut presented as if it were. Budget reallocation from spend and sales is the marketing-mix model, not this credit.

## How to read a result

- **Read the range, then the decision.** CALL TODAY means the value of a contact is high enough to spend the hour. The rupee band is why.
- **PUSH FORWARD** means the hazard is dropping. Waiting is the risk the value-at-risk line is pricing.
- **QUALIFY** means the trace does not yet support a call-list rank. Do not override it with the CRM’s 0–100 score and call it the same analysis.
- **An open lead is not a loss.** The survival curve is what keeps a slow deal in the pipeline.
- **Channel credit is observational.** A touch that is always last will look important under last-touch and ordinary under Shapley. Use the labelled split. Do not brief it as the return on ad spend.
- **A refusal** means there are not enough closed deals to fit the curve or the attribution. Send more history, or accept a score that says it is not fitted.
- **Recompute by sending the pipeline again.** The server does not watch it overnight.

## Data and what this will not do

Lead records are personal data. Send ids, stages, and touches. The server processes the request and does not store it. Each user signs in with OAuth. Your own duties under India’s Digital Personal Data Protection Act, the GDPR, or any other law that applies to the export, including consent and retention, stay with you. Past 500 leads, the script runs beside the file and does not call out.

The server will not turn an open lead into a loss, will not rank the queue by a bare score with no range, will not present sampled touch credit as a causal channel ROI, and will not fit a sharp model on a pipeline with too few closes. It will not price the quote. It will not store the book.

Privacy policy: https://saifs.ai/privacy_policy

## Plans

| Tier | What it covers |
|---|---|
| **Free** | Up to 100 leads, a monthly recompute, and the lead score |
| **Pro** | ₹4,100 / $49 per month — up to 1,000 leads, a nightly recompute when you send the file, stage probabilities, and value at risk |
| **Team** | ₹12,400 / $149 per month — up to 5,000 leads, the ranked call queue, Shapley credit, and CRM export pulls |
| **With customer tiers** | ₹20,000 / $249 per month together with [Customer Intelligence & Segmentation](../../customer-intelligence/README.md), from first touch through the customer book |

The free tier is enough to score a small export and read who would be called, with no sales call. One server call still stops at 500 leads. Above that, use the local script. Current terms are on [saifs.ai](https://saifs.ai).

## Support

- Install and client setup: [server README](../README.md)
- After they buy: [Customer Intelligence & Segmentation](../../customer-intelligence/README.md)
- The price on a quote: [Quote Price Optimization](../../quote-pricing/README.md)
- Spend and channel mix: [Marketing Intelligence & Optimization](../../marketing-intelligence/README.md)
- Website: https://saifs.ai
- Registry: https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
