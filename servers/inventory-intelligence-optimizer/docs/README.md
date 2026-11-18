# Inventory Intelligence & Optimizer

[SAIFS AI](https://saifs.ai) (`saifs.ai`) publishes this as a remote MCP server under the registry name `ai.saifs/inventory-intelligence-optimizer`. It reads a stock list and, when you have it, sales history, and returns how much to order, when to reorder, what is overstocked, how much cash is tied up, and a buying list grouped by supplier.

People search for this as inventory optimization, reorder point, safety stock, EOQ, stock replenishment, overstock analysis, or pharmacy inventory. The questions are “what should I order,” “what is overstocked,” and “when to reorder.” The short install page is the [server README](../README.md). This page is what an order quantity means, why a fixed reorder level goes stale, and how to send a stock report.

| | |
|---|---|
| **Registry name** | `ai.saifs/inventory-intelligence-optimizer` |
| **Endpoint** | `https://inventory-optimizer-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## What it is

Column names can stay as they are on the export: Item Code, Closing Stock, Open PO, Lead Time, and the sales columns beside them. One call to `inventory_optimizer` covers about 200 products and 2,500 sales rows. A larger book uses `inventory_optimizer_get_engine`, a Python script you run on the machine that holds the file.

What comes back:

- An order quantity per item, with the total-cost curve around it.
- A reorder point and safety stock for the service level you set.
- Items sitting outside those bands, ranked by the cash they tie up or the sales they put at risk.
- A purchase queue grouped by supplier. You can cap it with a budget.
- The trace: which formula, which inputs, which model version.

Every quantity and every rupee figure comes back as a range. A history too short or too gappy to support the formula is refused, with the reason, rather than given a tight number.

The same call has a pharmacy and hospital-store mode. You send each item’s VED class (vital, essential, or desirable) and, for batches, the expiry date. The call returns an ABC-VED category, a higher service level for the critical cells, a first-expiry-first-out issue order, and the value sitting close to expiry. The class comes from your pharmacy or therapeutics committee. The server does not decide how critical a medicine is, and it does not prescribe, substitute, or give clinical advice.

The call is stateless. Send the stock list again when you recompute. Nothing is stored. A reorder alert is a line in that response: on-hand plus open purchase orders is at or under the reorder point.

Teams that replace a static reorder level with this math free on the order of 10–30% of the cash in inventory, and cut stockouts while holding the same service level. That is a range across books, not a promise for one SKU.

A single EOQ from a demand, an order cost, and a holding cost you already know is the [Business Calculator](../../business-calculator/README.md). The demand distribution and the lead-time demand that safety stock should be built on are the [Demand Forecasting](../../demand-forecasting/README.md) server. A messy export should be cleaned first with [Data Cleaning & Validation](../../data-cleaning/README.md).

## Why a fixed reorder level is wrong half the year

Distributors, retailers, manufacturers, pharmacies, and e-commerce stores all decide how much to order, when, and how much buffer to hold. A level set in January is wrong after a season turns, a lead time stretches, or a slow mover stops selling. Too much stock ties up cash and, for a perishable, expires. Too little loses the sale.

The formulas are old. Economic order quantity dates to Harris (1913). The newsvendor critical fractile dates to Edgeworth (1888). They are not what most mid-market stock books run. Those books run a spreadsheet. A language model can evaluate one square root from a prompt. It will not recompute when you send this week’s stock and sales, keep the assumptions versioned on the trace, and refuse when the history cannot support a buffer.

## How to use it

Connect the endpoint in the [install README](../README.md) and sign in when the client asks. Export stock and sales from Tally, Zoho Inventory, QuickBooks, or the ERP. Send the rows.

```json
{
  "items": [
    {
      "item_code": "SKU-4471",
      "closing_stock": 40,
      "open_po": 0,
      "lead_time_days": 14,
      "unit_cost": 220,
      "supplier": "North Supply"
    }
  ],
  "sales": [
    {"item_code": "SKU-4471", "date": "2026-10-06", "qty": 12}
  ],
  "service_level": 0.95,
  "order_budget": 500000
}
```

Holding cost is the input people guess. If you do not send one, the default offered is about 20–30% of unit cost per year, and the trace says that default was used. Change it. The reply includes how the order quantity and the cash figure move when that percent moves, so a fuzzy holding cost is visible rather than buried.

### What you get on a normal book

EOQ is the quantity that balances ordering cost and holding cost:

```text
EOQ = √(2 · D · S / H)
```

D is annual demand, S is the cost of placing one order, and H is the cost of holding one unit for a year. The total-cost curve around that quantity is part of the reply, so you can see what a pack size or a supplier minimum does to the cost.

Safety stock for a service level, when demand varies and lead time is steady, is

```text
SS = z · σ_demand · √(lead time)
```

z is the standard-normal quantile of the service level. When lead time varies as well, the buffer uses both variances:

```text
SS = z · √(L · σ_demand² + d² · σ_lead²)
```

The reorder point is expected demand over the lead time, plus that buffer:

```text
ROP = d · L + SS
```

`flag` the items whose stock sits outside the band, and rank them by cash. A purchasing manager’s morning line looks like: reorder SKU-4471 today, it is under its reorder point; twelve slow movers are overstocked and about ₹8 lakh is sitting in them. The ₹8 lakh is the shape of a cash-impact line. Your book produces its own range.

### Perishable and one-shot buys

A festival buy, a short-dated drug, or any order you cannot carry into the next cycle is a newsvendor problem, not an EOQ. The critical fractile is

```text
CF = Cu / (Cu + Co)
```

Cu is the margin lost when you stock one too few. Co is the cost left when you stock one too many, which is unit cost minus whatever salvage or markdown you can still get. The order quantity is the demand quantile at that fractile. For a normal demand with mean μ and standard deviation σ,

```text
Q* = μ + z · σ    where z = Φ⁻¹(CF)
```

### Pharmacy and hospital store

Send `ved_class` as V, E, or D. The server will not infer it. ABC is annual consumption value: A through about 80% of value, then B, then C. The matrix is the combination.

| Category | Cells | Control |
|---|---|---|
| I | AV, AE, AD, BV, CV | Tight. A high service level. Vital items are buffered so they do not stock out. |
| II | BE, CE, BD | Moderate. |
| III | CD | Minimal. |

Those service levels feed the same safety-stock formula, with a higher z for Category I.

`fefo` issue order, when you send batches, is earliest expiry first: batch id, expiry, quantity on hand, and the quantity to pick. `near_expiry` ranks stock that will expire inside the day threshold you set, by the value that will be written off if it does not move.

This mode is stock control. It does not choose a therapy, suggest a substitute, or decide that a cheap item is clinically unimportant. A cheap vital item is Category I because you marked it vital.

### Prompts

- “What should we reorder this week?”
- “How much cash is stuck in slow stock?”
- “Set safety stock for these SKUs.”
- “Here's our stock report. What should we order this week and how much?”
- “Which items are overstocked and how much cash is stuck in them?”
- “Build a purchase list grouped by supplier within a budget of ₹5,00,000.”
- “Classify this pharmacy list. Here are the VED classes from the committee. Which batches expire in 30 days?”

## Tools

EOQ, safety stock, the newsvendor quantity, overstock, the supplier list, ABC-VED, and FEFO are results inside `inventory_optimizer`. They are not separate tool names.

| Tool | What you send | What comes back | Do not use it when |
|---|---|---|---|
| `inventory_optimizer` | A stock list and, if you have it, sales rows. About 200 products and 2,500 sales rows. Optional service level, holding-cost percent, budget, VED class, and batches | Order quantities, reorder points, safety stock, overstock ranked by cash, and a supplier-grouped buying list. In pharmacy mode, the matrix, the FEFO pick, and near-expiry value. Each figure is a range, with the formula on the trace | The book is larger than that, the sales history is too short or gappy, or you want a clinical decision. Thin history is refused. VED is yours to assign |
| `inventory_optimizer_get_engine` | The request for a larger stock book | A Python script you run locally. The rows stay on your machine | The book fits in one `inventory_optimizer` call |

## How to read a result

- **Read the range, then the midpoint.** A single reorder quantity with no band is not what this server returns.
- **Read the holding-cost note.** If the trace says the 20–30% default was used, the cash figure moves when you replace it. The sensitivity is there so you can.
- **Reorder point includes open purchase orders.** Stock already on order counts. Ordering again because the shelf looks empty double-buys.
- **Overstock is cash, not a scolding.** The rank is the money stuck in slow movers. It is the input to a return, a markdown, or a stop on the next buy.
- **Newsvendor is for one cycle.** Do not put an EOQ on a short-dated or a one-festival item. The critical fractile uses your margin and your salvage, so a wrong salvage makes a wrong buy.
- **Category I is only as good as the VED you sent.** A vital item left unmarked is treated as ordinary. The server will not fill that mark in.
- **A refusal** means the demand history cannot support the buffer. Clean the gaps, or send a longer history from the demand-forecasting server, and call again.

## Data and what this will not do

A stock and sales export is operational data. The server processes the request and does not store it. Each user signs in with OAuth. Your own duties under India’s Digital Personal Data Protection Act, the GDPR, or any other law that applies to the file stay with you. Past about 200 products or 2,500 sales rows, the script runs where the file already is.

The server will not invent a clinical criticality, recommend a substitute medicine, or turn a thin history into a precise reorder point. It will not watch the warehouse overnight on its own. Send the list again. It will not place the purchase order. The buying list is the recommendation, grouped by supplier, inside the budget you set.

Privacy policy: https://saifs.ai/privacy_policy

## Plans

| Tier | What it covers |
|---|---|
| **Free** | 25 SKUs, a weekly recompute |
| **Pro** | ₹4,100 / $49 per month — 500 SKUs, a nightly recompute when you send the new list, reorder alerts in the reply |
| **Business** | ₹12,400 / $149 per month — 5,000 SKUs, perishable and newsvendor quantities, ERP export pulls, cash-impact ranking |
| **Enterprise** | More than one warehouse, a service-level policy you set, and overage by each further 500 SKUs |

The free tier is enough to run a short stock list with no sales call. One server call still stops around 200 products and 2,500 sales rows. Above that, use the local script. Current terms are on [saifs.ai](https://saifs.ai).

## Support

- Install and client setup: [server README](../README.md)
- Lead-time demand and the demand distribution: [Demand Forecasting](../../demand-forecasting/README.md)
- One EOQ from numbers you already have: [Business Calculator](../../business-calculator/README.md)
- Gaps, duplicates, and unit slips in the export: [Data Cleaning & Validation](../../data-cleaning/README.md)
- Website: https://saifs.ai
- Registry: https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
