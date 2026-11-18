# Business Calculator: GST, Margin & Everyday Formulas

[SAIFS AI](https://saifs.ai) (`saifs.ai`) publishes this as a remote MCP server under the registry name `ai.saifs/business-calculator`. It is a deterministic calculator for the arithmetic that shows up on an invoice, a quote, or a board report: GST, margin and markup, percentages, break-even, EOQ, DSO, TDS, and more than 100 related formulas.

People search for this as a business calculator, GST calculator, profit margin calculator, markup calculator, discount calculator, break-even calculator, invoice calculator, EOQ calculator, percentage calculator, or KPI calculator. The short install page is the [server README](../README.md). This page is what the server is, why a model’s usual answer is not enough, and how to use each tool.

| | |
|---|---|
| **Registry name** | `ai.saifs/business-calculator` |
| **Endpoint** | `https://business-calculator-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## What it is

Thirty-two task-named tools, plus two helpers, cover the formulas. Each tool groups a few related operations (`gst_calculator` does add-GST, split-inclusive, CGST/SGST versus IGST, reverse charge, and dated rate lookup). The compact set of 32 tools is the default, so an agent’s context is not flooded with one tool per formula. Hosts that can search tools dynamically can use a fuller profile with one tool per formula.

Every answer returns the same fields:

| Field | What it is |
|---|---|
| `result` | The number, or the structured breakdown (for example base, CGST, SGST, round-off). |
| `formula` | The expression that produced it, so the number can be checked. |
| `inputs_used` | The values the formula actually used. |
| `rounding` | The rounding rule applied (half-up, nearest ₹1, nearest 50 paise, or banker’s). |
| `rule_version` | Which GST, TDS, or other dated table was used, when a rule table applies. |
| `effective_date` | The date that table applies from. An older date returns the older rate. |
| `explanation` | One plain sentence. |
| `upsell_hint` | Present only when the question needs history or optimisation that this calculator does not do. |

The same inputs always give the same answer. Money is decimal arithmetic, not floating point. The call is stateless: the inputs are processed and not stored. There is no call out to fetch a rate. Foreign exchange is whatever rate you pass in.

## Why a language model is not enough

Agents and staff do this arithmetic all day: 18% of a price, profit on cost, a GST split, an invoice total, break-even, EOQ, DSO, attrition. A model usually gets them right. “Usually” is the wrong standard once the number is printed on an invoice, a quote, or a report to the board.

The slips that show up in practice:

- GST added on top of a price that already includes GST.
- Margin treated as markup, or the reverse.
- A change in percent confused with a change in percentage points.
- A rounding rule nobody stated, so two runs differ by a rupee.
- Lakh and crore grouped like millions, or mixed up with them.
- A ratio that goes wrong halfway through a long chain.

A second run of the same question can also come back with a different formula. This server’s answer carries the formula, the rounding rule, and, where a statutory rate is involved, the rule version and effective date.

## How to use it

Connect the endpoint in the [install README](../README.md) and sign in when the client asks. Then name the job in ordinary language. The client picks the tool. If you want the exact operation names and input fields first, call `list_formulas`. If you want the formula in words before you trust a number, call `explain_formula`.

A call has this shape:

```json
{
  "tool": "gst_calculator",
  "operation": "split GST-inclusive amount",
  "inputs": { "amount": "1180", "rate": "18%" },
  "rounding": "nearest ₹1",
  "locale": "en-IN"
}
```

`list_formulas` is the catalogue of real operation names, inputs, and units. Use that list for the `operation` string. Optional fields are `units`, `rounding`, `locale`, and `rate_table_version`.

What comes back for that GST split is base ₹1,000 and GST ₹180, shown as CGST ₹90 and SGST ₹90 when the supply is intra-state, plus the formula, the inputs used, the rounding rule, and the GST table’s version and effective date.

### Rules that change the answer

- **Units.** If a unit matters and you did not send it, the tool refuses instead of guessing.
- **Rounding.** Always named on the answer. Do not assume half-up.
- **Indian conventions.** Lakh/crore grouping, amount in words, GST by place of supply, and TDS sections come from dated tables.
- **Rates that change.** GST and TDS come from those versioned tables, or from a rate you supply. Pass an older effective date when you need the rate that applied then. Foreign exchange is never looked up; you supply the rate.
- **No interest maths.** EMI, simple or compound interest, NPV, IRR, XIRR, present or future value, discounting, loan amortisation, and fixed-deposit or credit-card returns are refused, with a reason. Gratuity and other payroll figures are the statutory formula applied to the inputs you sent. They are estimates, not legal advice.
- **Not tax, legal, or financial advice.** The calculation shows its formula. Filing, a notice, or a board decision still needs a person who owns that decision.

### Worked prompts

- “What price gives 25% margin on cost 750?” → price ₹1,000, which is a 33.33% markup on cost.
- “Split ₹1,180 into base and GST.” → ₹1,000 + ₹180 at 18%. Intra-state, the ₹180 is CGST ₹90 and SGST ₹90.
- “Break-even units for fixed 50,000, price 250, cost 150.” → 500 units, ₹1,25,000 revenue.
- “10% then 5% successive discount.” → 14.5% effective, not 15%.
- “EOQ for demand 10,000, order cost 50, holding cost 2.” → 707.11 units.
- “Amount in words and Indian grouping for 12345678.5.” → 1,23,45,678.50.

### Workflows that chain tools

1. **Quote.** Cost and target margin → price → discount check → GST → quote total.
2. **Invoice.** Order lines through `invoice_calculator` → round-off → amount in words.
3. **Purchase.** Supplier quotes → landed cost per unit → compare → EOQ for the order size.
4. **MIS.** The month’s numbers → growth, margins, variance, and ratios, each with its formula.
5. **HR.** Joiner date → pro-rata salary → leave accrual.
6. **Marketing.** A campaign export → ROAS, CAC, and payback, then keep or cut.

## Tools

Alphabetical, as a client lists them. `explain_formula` and `list_formulas` are the helpers.

| Tool | Operations | A typical result | Not for |
|---|---|---|---|
| `ad_metrics_calculator` | CTR, CPC, CPM, CPL, ROAS, simple ROI | Spend 10,000, 50,000 impressions, 1,000 clicks → CTR 2%, CPC 10, CPM 200 | ROI that depends on the time value of money |
| `average_statistics_calculator` | Average, weighted average, median, mode, population and sample standard deviation, variance, percentile, quartiles, z-score | `[2, 4, 4, 4, 5, 5, 7, 9]` → population σ 2.0, sample s 2.138090 | A forecast. See trend, then Demand Forecasting |
| `break_even_calculator` | Break-even units and revenue, contribution margin and ratio, target-profit units, margin of safety | Fixed 50,000, price 250, variable 150 → 500 units, ₹1,25,000 | A priced quote with win probability |
| `budget_variance_calculator` | Actual versus budget in amount and percent, favourable or adverse, pro-rata allocation by a driver | 11.5 lakh versus 10 lakh → +1.5 lakh (+15%) | A cleaned dataset. Check the extract first if the source is messy |
| `currency_converter` | Amount times your rate, cross rates, rounding to the currency’s minor unit | $1,000 × 83.2 → ₹83,200 | A live foreign-exchange fetch. The rate is yours |
| `date_due_date_calculator` | Days between, working days from a holiday list you supply, add days or working days, ageing days, month-end | 10 Oct + 30 days → 9 Nov | A cash-flow calendar of who pays when |
| `depreciation_calculator` | Straight-line, written-down value, year-by-year schedule | WDV 15% on 1,00,000 → 15,000, then 12,750 | A tax filing. The schedule is the formula, not advice |
| `discount_calculator` | Single discount, successive (chain) discount, trade plus cash, MRP back-calculation, discount percent from two prices | 10% then 5% → 14.5% effective | Margin. A discount percent is not a margin percent |
| `eoq_reorder_calculator` | EOQ, reorder point, safety stock as z·σ·√L with a σ you supply, days of cover, order frequency | Demand 10,000, order cost 50, holding 2 → EOQ 707.11 | Safety stock fitted from a real demand history |
| `explain_formula` | Plain-language explanation and a worked example for any operation | The formula in words, then one numeric walk-through | A number for your own inputs. Use the owning calculator |
| `growth_share_calculator` | Growth percent, CAGR on values, market share, contribution to growth, index numbers (base 100) | 100 → 200 in 5 years → CAGR 14.87% | Discounted cash-flow growth |
| `gst_calculator` | Add GST, split a GST-inclusive amount, CGST/SGST versus IGST by place of supply, reverse charge, dated rate lookup | ₹1,180 at 18% → ₹1,000 + ₹180 (CGST 90 + SGST 90) | Adding GST again onto a price that is already inclusive. Split it |
| `inventory_metrics_calculator` | Inventory turnover, DIO, fill rate, FIFO and weighted-average valuation, dead-stock percent | Lots 100 at 10 and 100 at 12 → weighted average cost 11 | A reorder quantity from a demand forecast |
| `invoice_calculator` | Line totals, then discount, tax, round-off, grand total, amount in words | 2×500 + 1×1,250, −10%, +18% → 2,389.50, rounds to **2,390** | Interest or late fees on the invoice |
| `landed_cost_calculator` | Allocate freight, duty, insurance, and handling by value, weight, or quantity; landed unit cost | Freight 1,000 on 3,000 / 7,000 → 300 / 700 | Choosing the supplier. This is the cost build-up only |
| `list_formulas` | Catalogue of every operation, its inputs, its units, and the tool that owns it | The list to copy an `operation` name from | A calculation |
| `margin_markup_calculator` | Margin to markup and back, price from a target margin, price from a markup, maximum cost from price and margin | Cost 750, margin 25% → price 1,000 (markup 33.33%) | Treating 25% margin and 25% markup as the same price |
| `number_format_calculator` | Rounding modes, Indian grouping (lakh/crore), international grouping, amount in words, lakh/crore against million/billion | 12345678.5 → 1,23,45,678.50 | A currency conversion. Format is not a rate |
| `payroll_calculator` | Overtime pay, pro-rata salary, leave accrual, gratuity under the Indian statutory formula | 30,000 × 10 years → gratuity ₹1,73,076.92 | Legal advice, or a payroll run. The figure is the formula on your inputs |
| `percentage_calculator` | Percent of, what percent, percent change, percentage points, increase or decrease by a percent, reverse percent | 1,200 after a 20% increase → base 1,000 | Percentage points labelled as percent |
| `production_efficiency_calculator` | OEE, availability, performance, quality, capacity utilisation, takt time, cycle time, yield and scrap, Little’s law | 0.90 × 0.95 × 0.98 → OEE 83.79% | A production schedule or a solver |
| `profit_loss_calculator` | Profit or loss amount, percent on cost, percent on sale, gross profit | Cost 800, selling price 1,000 → +200, which is 25% on cost | Margin-versus-markup conversion. Use `margin_markup_calculator` when the question is the price |
| `ratio_analysis_calculator` | Gross, operating, and net margin, current ratio, quick ratio, debt-to-equity, working capital, asset turnover | Current assets 12 lakh / current liabilities 8 lakh → current ratio 1.5 | A forecast of the next period’s ratios |
| `ratio_proportion_calculator` | Simplify a ratio, split an amount by a ratio, rule of three, unitary method | ₹50,000 split 3:2 → 30,000 / 20,000 | A weighted index or a market share |
| `sales_funnel_calculator` | Stage conversion, overall conversion, win rate, average deal size, sales velocity, quota attainment, pipeline coverage | 40 × 25% × 50,000 / 30 → ₹16,666.67 a day | Who to call. This is the rate, not the account list |
| `shipping_weight_calculator` | Volumetric weight from a carrier divisor, chargeable weight, CBM, pallet and carton count | 50×40×30 cm / 5,000 → 12 kg | Freight cost allocation across items. That is landed cost |
| `survey_sample_calculator` | Confidence interval for a mean or a proportion, sample size for a proportion, margin of error | ±5% at 95% → 385 | A test-item or ability analysis |
| `tds_tcs_calculator` | TDS by section with a threshold check from the dated table or your rate, TCS, net payable | ₹50,000 fees at 10% → TDS 5,000, net 45,000 | A rate you hope the server will look up on the internet. It uses the dated table or your rate |
| `tier_pricing_commission_calculator` | All-units or incremental slab pricing, commission slabs, incentive payout | 1.5 lakh sales, 5% up to 1 lakh and 7% above → 8,500 | The price that wins the deal |
| `trend_basic_calculator` | Moving average, least-squares linear trend, correlation | A 3-month moving average, or a slope and intercept | Next month’s sales from a history. That is a forecast |
| `unit_converter` | Weight, length, area, volume, time, temperature, packaging units | 2.5 t → 2,500 kg | A missing unit. The tool refuses rather than assume |
| `unit_economics_calculator` | CAC, LTV with no discounting, LTV:CAC, CAC payback in months, ARPU, MRR/ARR, churn, retention, net revenue retention | ARPU 1,000, margin 70%, churn 5%, CAC 3,500 → LTV 14,000, 4.0×, payback 5 months | Discounted LTV, NPV, or IRR |
| `working_capital_days_calculator` | DSO, DPO, DIO, cash conversion cycle, ageing buckets 0–30 / 31–60 / 61–90 / 90+ | Receivables 4 lakh, revenue 24 lakh → DSO 60.83 days | When each customer will actually pay |
| `workforce_metrics_calculator` | Attrition, average headcount, absenteeism, cost per hire, revenue per employee, billable utilisation | 18 leavers / average headcount 120 → 15% attrition | Individual pay, performance, or hiring fairness. Send aggregates |

## Where the formulas sit in the business

| Department | Everyday jobs | Tools |
|---|---|---|
| Sales | Price a quote, apply a discount, check margin before approving a deal, commission, quota, pipeline coverage, sales velocity | `margin_markup_calculator`, `discount_calculator`, `tier_pricing_commission_calculator`, `sales_funnel_calculator`, `profit_loss_calculator` |
| Pricing | List price from cost, target-margin price, slab prices, MRP back-calculation, price per pack size | `margin_markup_calculator`, `discount_calculator`, `tier_pricing_commission_calculator`, `percentage_calculator` |
| Marketing | ROAS and simple ROI, CTR, CPC, CPM, CPL, CAC, LTV:CAC, payback, growth and share | `ad_metrics_calculator`, `unit_economics_calculator`, `growth_share_calculator` |
| Finance and accounts | Invoice total and round-off, GST split, TDS/TCS, depreciation, budget versus actual, ratios, working-capital days, ageing, FX | `invoice_calculator`, `gst_calculator`, `tds_tcs_calculator`, `depreciation_calculator`, `budget_variance_calculator`, `ratio_analysis_calculator`, `working_capital_days_calculator`, `currency_converter` |
| Procurement | Unit cost of quotes, landed cost, chain discounts, EOQ, DPO | `landed_cost_calculator`, `discount_calculator`, `eoq_reorder_calculator`, `working_capital_days_calculator` |
| Inventory and warehouse | Reorder point, safety stock from a given σ, days of cover, FIFO and weighted-average value, turnover, DIO, fill rate, dead stock | `eoq_reorder_calculator`, `inventory_metrics_calculator` |
| Production | OEE, capacity utilisation, takt and cycle time, yield and scrap, Little’s law for WIP, unit conversions | `production_efficiency_calculator`, `unit_converter` |
| Logistics | Chargeable weight, CBM, pallet and carton count, freight per unit, due dates | `shipping_weight_calculator`, `landed_cost_calculator`, `date_due_date_calculator` |
| HR and payroll | Overtime, pro-rata salary, leave accrual, gratuity estimate, attrition, absenteeism, cost per hire, utilisation | `payroll_calculator`, `workforce_metrics_calculator` |
| MIS | Growth, CAGR, percent versus percentage points, variance, index numbers, averages | `percentage_calculator`, `growth_share_calculator`, `budget_variance_calculator`, `average_statistics_calculator` |
| Subscriptions | MRR, ARR, churn, retention, net revenue retention, ARPU | `unit_economics_calculator` |
| Retail and e-commerce | Discount and MRP, margin per SKU, shipping weight, GST on an order | `discount_calculator`, `margin_markup_calculator`, `gst_calculator`, `shipping_weight_calculator` |

Call volume is highest in distribution and wholesale, retail and e-commerce, MSME manufacturing, import/export and logistics, professional services, SaaS, CA firms and bookkeeping, BPO and IT services, and real-estate and construction quoting.

## The methods

These are standard formulas, evaluated in decimal arithmetic. The server does not estimate them.

| Method | What the tool applies |
|---|---|
| **Percentages** | Percent of a base, reverse percent, percent change, and percentage points kept distinct. |
| **Cost-volume-profit** | Break-even units = fixed cost / (price − variable cost). Contribution margin, target profit, and margin of safety sit on the same identity. |
| **EOQ** | Harris (1913): order quantity from demand, order cost, and holding cost. Reorder point and safety stock use z·σ·√L with the σ you pass in. |
| **Little’s law** | Little (1961): average inventory in process = throughput × time in process. |
| **OEE** | Nakajima (1988): availability × performance × quality. |
| **Ratio analysis** | Gross, operating, and net margin, current and quick ratios, debt-to-equity, working capital, asset turnover, DSO, DPO, DIO, and the cash conversion cycle. |
| **GST and TDS** | Indian statutory rates from a dated, versioned table, or from a rate you supply. Place of supply chooses CGST+SGST or IGST. A section threshold is checked before TDS is applied. |
| **Descriptive statistics** | Mean, weighted mean, median, mode, population and sample variance, percentiles, quartiles, z-score. |

## When the calculator is the wrong depth

Some questions only look like arithmetic. The calculator answers the formula in front of it and can point at the SAIFS AI server that uses history or optimisation.

| Question | What this server does | Where the rest of the question goes |
|---|---|---|
| Safety stock from a real demand history | z·σ·√L once you already know σ | [Inventory Intelligence & Optimizer](../../inventory-intelligence-optimizer/README.md) |
| Next month’s sales | Moving average, or a slope and intercept | [Demand Forecasting](../../demand-forecasting/README.md) |
| When customers will actually pay | DSO and ageing buckets | [13-Week Cash Flow Forecast](../../cash-forecast/README.md) |
| The price most likely to win this quote | Margin, markup, and discount | [Quote Price Optimization](../../quote-pricing/README.md) |
| Which customers matter, and who to call | Conversion rates and deal size | [Customer Intelligence & Segmentation](../../customer-intelligence/README.md) |
| Whether the extract is clean enough to calculate on | Nothing. It will compute what you send | [Data Cleaning & Validation for AI Agents](../../data-cleaning/README.md) |

## Data and how rates are framed

Send numbers, units, and, for GST or TDS, the date that should govern the rate. Do not send a request that needs a live foreign-exchange rate; pass the rate yourself. The server reads a dated internal table or your inputs, computes, and returns. It does not store the request and it does not call out to another service.

Each user signs in with OAuth. Your own duties under India’s Digital Personal Data Protection Act, the GDPR, or any other law that applies to the figures you send stay with you. Payroll and workforce tools are for aggregates and stated formulas. Gratuity output is that formula, not a determination of what is owed.

Privacy policy: https://saifs.ai/privacy_policy

## Plans

| Tier | What it covers |
|---|---|
| **Free** | 5,000 calls a month, all tools |
| **Pro** | ₹830 / $10 per month — 100,000 calls, dated GST and TDS tables with change alerts |
| **Business** | ₹2,500 / $29 per month — unlimited calls, custom formulas and slabs, an audit export |
| **With any paid SAIFS AI server** | This calculator is included |

The free tier is enough to try a real invoice or GST split with no sales call. Current terms are on [saifs.ai](https://saifs.ai).

## Support

- Install and client setup: [server README](../README.md)
- Website: https://saifs.ai
- Registry: https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
