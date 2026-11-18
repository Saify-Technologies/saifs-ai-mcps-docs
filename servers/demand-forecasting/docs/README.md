# Demand Forecasting

[SAIFS AI](https://saifs.ai) (`saifs.ai`) publishes this as a remote MCP server under the registry name `ai.saifs/demand-forecasting`. It forecasts how much you will sell, per product and optionally per region or channel, and puts a range on every number.

People search for this as demand forecasting, sales forecasting, time-series forecasting, Holt-Winters, intermittent demand, or Croston’s method. The questions are “how much will we sell” and “next month’s demand.” The short install page is the [server README](../README.md). This page is what a forecast contains, why last-year-plus-ten-percent breaks the next decision, and how to send a history.

| | |
|---|---|
| **Registry name** | `ai.saifs/demand-forecasting` |
| **Endpoint** | `https://demand-forecaster-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## What it is

You send sales history. A date and a quantity are enough. SKU, region, channel, and lead time are optional, and the file can keep the column names from Excel, Tally, or a CSV. The forecast comes back by period — daily, weekly, or monthly — for the horizon you ask.

Every number has a prediction interval. The listing shape is a P10–P90 band. You can ask for a 95% range. A point with no band is not a result this server returns.

The server picks the model by backtest, not by a fixed label:

| History looks like | Model |
|---|---|
| A level, little trend, no season | Simple exponential smoothing (Brown, 1956) |
| A level and a trend | Holt |
| A level, a trend, and a repeating season | Holt-Winters (1957/60) |
| Lumpy or intermittent: spare parts, slow B2B, many exact zeros | Croston (1972), or the Syntetos-Boylan adjustment (SBA) when that backtest is better |

Selection uses time-series cross-validation. The holdout is a later slice of the same series. Rows are not shuffled. The reply names the model and its backtest score. MAPE and MASE are the accuracy figures when the history you sent already contains the actuals those forecasts can be scored against.

Against a naive baseline such as last year plus a flat percent, forecast error on this kind of fit is on the order of 10–30% lower. That is a range across series, not a promise for one SKU.

One call to `demand_forecaster` covers about 200 products and 2,500 rows. A larger file uses `demand_forecaster_get_engine`: a Python script you run on the machine that holds the file, so those rows never come to the server.

The call is stateless. Send the history each time, including the latest weeks when you re-fit. Nothing is stored. Lead-time demand from this forecast is the input the [Inventory Intelligence & Optimizer](../../inventory-intelligence-optimizer/README.md) uses for safety stock and order quantities. A moving average on a single column, with no interval and no model choice, is the [Business Calculator](../../business-calculator/README.md).

## Why last year plus 10% is the wrong input

Next month’s sales, per SKU or per region, is the number inventory, staffing, and cash are planned from. A flat uplift on last year ignores a trend that has turned, a season that is early, and a spare part that sells twice a year in clumps. Holt-Winters and Croston’s method have been published, and benchmarked, for decades. They usually sit with a data-science team. A language model can sketch one forecast from a prompt. It will not re-fit when you send a new month, keep a backtest score next to the model, and refuse when the series is too short to identify.

## How to use it

Connect the endpoint in the [install README](../README.md) and sign in when the client asks. Export sales from Tally, a POS, an ERP, or a spreadsheet. Send the rows. Name the horizon.

```json
{
  "sales": [
    {"date": "2026-01-06", "sku": "SP-220", "qty": 0, "region": "West"},
    {"date": "2026-01-13", "sku": "SP-220", "qty": 4, "region": "West"}
  ],
  "horizon": 13,
  "frequency": "weekly",
  "lead_time_periods": 4
}
```

Column names in the file can stay as they are. Date and quantity are the required pair. Add SKU to split products, and region or channel when the forecast should split that way. `lead_time_periods` is what turns the forecast into demand over the replenishment lead time, which is the figure the inventory server orders from.

Ask for two full seasonal cycles before you trust a seasonal read. A Diwali peak, a summer, or a financial-year pattern needs to have happened twice in the file. One cycle cannot separate “this season is always big” from “this year grew.” If the history is shorter than the model needs, the call refuses and says why. It does not fill the gap with a guess.

Re-fit by sending the history again with the new actuals. The server does not keep last night’s fit. A shift in level that the old season no longer explains comes back as a wider interval and a note that the recent points have left the previous pattern. That widening is the regime-change signal. Confidence gets wider on purpose. It does not stay tight through a break.

### What you get

- A forecast per period, per series, with a prediction interval.
- The model that won the backtest, and that backtest score (MAPE or MASE against the held-out tail).
- A decomposition you can read: level, trend, and season, when the chosen model has those parts.
- For an intermittent series, a Croston or SBA forecast of size and gap, not a Holt-Winters line pulled toward zero.
- Lead-time demand when you sent a lead time, ready to pass into inventory planning.

### Prompts

- “Forecast next month’s sales per product.”
- “How much will we sell during Diwali?”
- “Forecast spare-parts demand.”
- “Here’s our Tally sales export. How much of each item will we sell over the next 13 weeks?”
- “Which products are slow movers, and what should we expect from them next month?”
- “Forecast monthly demand for spare part SP-220 with a 95% range.”

## Tools

The published server exposes two tools. Seasonality, intermittent demand, the backtest, the explanation, and the lead-time total are part of `demand_forecaster`. They are not separate tool names.

| Tool | What you send | What comes back | Do not use it when |
|---|---|---|---|
| `demand_forecaster` | Sales rows (date, quantity, and any of SKU, region, channel, lead time) and a horizon. Up to about 200 products and 2,500 rows | Forecasts with intervals, the chosen model, its backtest score, and lead-time demand | The file is larger than that, or the series is too short. A short series is refused with a reason. A larger file uses the local script |
| `demand_forecaster_get_engine` | The request for a sales file above that size | A Python script you run locally. The forecasts match the server. The rows stay on your machine | The file fits in one `demand_forecaster` call |

## The methods

### Holt-Winters

Triple exponential smoothing. The multiplicative form, which fits demand that scales with the season, updates three states. α, β, and γ are chosen by minimising in-sample error, then judged on the backtest, not on that in-sample fit alone.

```text
level    ℓ_t = α (y_t / s_{t−m}) + (1 − α) (ℓ_{t−1} + b_{t−1})
trend    b_t = β (ℓ_t − ℓ_{t−1}) + (1 − β) b_{t−1}
season   s_t = γ (y_t / ℓ_t) + (1 − γ) s_{t−m}
forecast ŷ_{t+h} = (ℓ_t + h · b_t) · s_{t−m+h}
```

m is the season length. h is how many periods ahead. Holt drops the seasonal term. Simple exponential smoothing keeps only the level. The reply says which of the three was selected.

### Croston and SBA

Intermittent demand is mostly zeros, then a clump. Holt-Winters treats the zeros as a low season and drifts. Croston smooths two series separately: the size of a demand when it happens, and the number of periods between demands. The forecast is size divided by that interval.

SBA is the same split with a correction for the upward bias Croston’s ratio is known for. The backtest picks Croston or SBA. Use this path for spare parts, slow movers, and B2B lines that do not sell every week. The retail buyer’s seasonal SKU stays on Holt-Winters.

### What the backtest is protecting

A random split would train on February and test on the previous January. Time-series cross-validation always tests on a later window. The score you see is that later window. MAPE is the average absolute percent error. MASE compares the error with a naive forecast, so a series full of zeros does not make a percent error look infinite. Both travel with the model version that produced them.

Tracked accuracy over months is the same call on a longer file, or you keeping the previous forecast beside the new actuals. The server does not store that log.

## How to read a result

- **Read the band first.** P10 to P90, or the 95% range you asked for, is the planning number. The midpoint is the centre of that band.
- **Read the model name.** Holt-Winters means a season was identified. Croston or SBA means the series is intermittent. If the name is simple smoothing, there was not enough structure for a trend or a season.
- **Read the backtest score next to it.** A wide interval and a weak MASE mean the history does not support a tight plan. Order and staff for the band, not for the midpoint.
- **A refusal is a result.** Too little history, or only one seasonal cycle, comes back with the reason and without a forecast. Do not replace it with last year plus 10%.
- **A wider interval after a quiet regime** means recent demand has left the old pattern. The band widened because the fit is less sure.
- **Lead-time demand** is the sum that matters for a purchase order. Pass it to Inventory Intelligence with the lead time you sent. It is not a substitute for the week-by-week cash forecast.

## Data and what this will not do

Sales history at this grain is operational data. The server processes the request and does not store it. Each user signs in with OAuth. Your own duties under India’s Digital Personal Data Protection Act, the GDPR, or any other law that applies to the export stay with you. A file past 200 products or 2,500 rows is forecast on your machine by the script.

The server will not return a bare point with no interval, will not invent a seasonal peak from a single cycle, and will not keep a nightly watch for you. Connectors that pull an ERP on a schedule are not part of this call. Send the export. It will not turn a forecast into a purchase order. That handoff is the inventory server, using the lead-time demand from this one.

Privacy policy: https://saifs.ai/privacy_policy

## Plans

| Tier | What it covers |
|---|---|
| **Free** | 25 series, a monthly forecast |
| **Pro** | ₹4,100 / $49 per month — 500 series, a nightly re-fit when you send the new history, accuracy tracking, prediction intervals |
| **Business** | ₹12,400 / $149 per month — 5,000 series, Croston’s method and SBA, regime-change notes, data-export pulls |

The free tier is enough to forecast a short list of products with no sales call. One server call still stops around 200 products and 2,500 rows; above that, use the local script. Current terms are on [saifs.ai](https://saifs.ai).

## Support

- Install and client setup: [server README](../README.md)
- Order quantities and safety stock from lead-time demand: [Inventory Intelligence & Optimizer](../../inventory-intelligence-optimizer/README.md)
- A single moving average or a fitted line, with no model choice: [Business Calculator](../../business-calculator/README.md)
- Website: https://saifs.ai
- Registry: https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
