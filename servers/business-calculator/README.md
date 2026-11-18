# Business Calculator: GST, Margin & Everyday Formulas

A deterministic business maths MCP with task-named tools for GST, margin/markup, invoices, EOQ, DSO, break-even, TDS and 100+ everyday formulas. Every answer returns the result plus formula, inputs used, rounding rule and rule version — same inputs always give the same answer. No interest, EMI, NPV or loan maths.

| | |
|---|---|
| **Registry name** | `ai.saifs/business-calculator` |
| **Endpoint** | `https://business-calculator-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-business-calculator": {
      "type": "http",
      "url": "https://business-calculator-mcp.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://business-calculator-mcp.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-business-calculator https://business-calculator-mcp.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://business-calculator-mcp.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

| Tool | What it does |
|---|---|
| `ad_metrics_calculator` | CTR, CPC, CPM, CPL, ROAS and simple ROI (no time value). |
| `average_statistics_calculator` | Average, weighted average, median, mode, std dev, variance, percentile, quartiles, z-score. |
| `break_even_calculator` | Break-even units/revenue, contribution margin, target-profit units, margin of safety. |
| `budget_variance_calculator` | Actual vs budget (amount, %), favourable/adverse, pro-rata cost allocation. |
| `currency_converter` | Convert with a caller-supplied rate; cross rates and minor-unit rounding. |
| `date_due_date_calculator` | Days between, working days, add days, ageing days, month-end. |
| `depreciation_calculator` | Straight-line and WDV schedules by year. |
| `discount_calculator` | Single/chain/trade+cash discounts, MRP back-calculation, discount % from prices. |
| `eoq_reorder_calculator` | EOQ, reorder point, simple safety stock, days of cover, order frequency. |
| `explain_formula` | Plain-language explanation and worked example for any operation. |
| `growth_share_calculator` | Growth %, CAGR, market share, contribution to growth, index numbers. |
| `gst_calculator` | Add/split GST, CGST/SGST vs IGST, reverse charge, dated rate lookup. |
| `inventory_metrics_calculator` | Turnover, DIO, fill rate, FIFO/WAC valuation, dead-stock %. |
| `invoice_calculator` | Line totals → discounts → tax → round-off → grand total → amount in words. |
| `landed_cost_calculator` | Allocate freight, duty, insurance and handling; landed unit cost. |
| `list_formulas` | Catalogue of every operation with inputs, units and owning tool. |
| `margin_markup_calculator` | Margin ↔ markup, price from target margin/markup, max cost from price. |
| `number_format_calculator` | Rounding modes, Indian/international format, amount in words, lakh/crore. |
| `payroll_calculator` | Overtime, pro-rata salary, leave accrual, Indian gratuity formula. |
| `percentage_calculator` | Percent of, what percent, % change, percentage points, reverse %. |
| `production_efficiency_calculator` | OEE, capacity utilisation, takt/cycle time, yield/scrap, Little's law. |
| `profit_loss_calculator` | Profit/loss amount, % on cost, % on sale, gross profit. |
| `ratio_analysis_calculator` | Margins, current/quick ratio, debt-to-equity, working capital, asset turnover. |
| `ratio_proportion_calculator` | Simplify ratio, split by ratio, rule of three, unitary method. |
| `sales_funnel_calculator` | Conversion, win rate, deal size, velocity, quota attainment, pipeline coverage. |
| `shipping_weight_calculator` | Volumetric/chargeable weight, CBM, pallet/carton count. |
| `survey_sample_calculator` | Confidence intervals, sample size for a proportion, margin of error. |
| `tds_tcs_calculator` | TDS by section with threshold check, TCS, net payable. |
| `tier_pricing_commission_calculator` | Slab/volume pricing, commission slabs, incentive payout. |
| `trend_basic_calculator` | Moving average, linear trend, correlation. |
| `unit_converter` | Weight, length, area, volume, time, temperature, packaging units. |
| `unit_economics_calculator` | CAC, LTV (no discounting), LTV:CAC, payback, ARPU, MRR/ARR, churn/NRR. |
| `working_capital_days_calculator` | DSO, DPO, DIO, cash conversion cycle, ageing buckets. |
| `workforce_metrics_calculator` | Attrition, headcount, absenteeism, cost per hire, utilisation. |

## More detail

What this server is, why a model’s usual arithmetic is not enough, and how to use each tool are in [docs/README.md](docs/README.md).

## Example prompts

- “What price gives 25% margin on cost 750?”
- “Split ₹1,180 into base and GST.”
- “Break-even units for fixed 50,000, price 250, cost 150.”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
