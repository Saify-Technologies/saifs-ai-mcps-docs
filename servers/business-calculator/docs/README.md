# 68 · Business Calculator: GST, Margin & Everyday Business Formulas MCP
**Initial moat: 56/100 (potential 60) · IW-Breadth: 92 · Phase: Wave 8 · Role: free / near-free ENTRY product that drives discovery to paid Saifs MCPs**
**Basis: standard business arithmetic and classical operations formulas: percentages, cost-volume-profit (break-even, 1930s), EOQ (Harris 1913), reorder point & safety stock, Little's law (1961), OEE (Nakajima 1988), DuPont-era ratio analysis, Indian GST/TDS statutory rules (dated tables), descriptive statistics.**

## 1. Problem statement
Agents and staff do simple business maths all day: 18% of a price, profit on cost, GST splits, invoice totals, break-even, EOQ, DSO, attrition. LLMs usually get these right, but "usually" isn't good enough when the number goes on an invoice, a quote or a board report. The common slips:
- adding GST on a GST-inclusive price
- confusing margin with markup
- percent vs percentage points
- wrong rounding
- lakh/crore mix-ups
- simple-ratio errors in long chains

Each run can also be done differently. Nobody gets a formula, a rounding rule or a rule version to point to.

## 2. Solution
One deterministic calculator MCP with **32 task-named tools + 2 helpers**, covering **100+ formulas**. Every answer returns:
`result` · `formula` · `inputs_used` · `rounding` · `rule_version` · `effective_date` · `explanation` (one plain sentence).
**The same inputs always give the same answer.**

### Design rules
1. **Task-named tools** (≈32), named the way people ask (e.g. `gst_calculator`). Each tool covers a few related formulas through an `operation` parameter. That's enough for the LLM and marketplace search to match, without flooding the agent's context or hitting platform tool limits.
2. **Optional "full" profile** (one tool per formula) for hosts with dynamic tool search; **compact profile (32 tools) is the default.**
3. **Money arithmetic:** decimal, not floating point.
4. **Rounding:** always explicit (half-up, nearest ₹1, nearest 50 paise, banker's).
5. **Units:** explicit; refused when missing and they matter.
6. **Indian conventions:** lakh/crore grouping, amount in words, GST place of supply, TDS sections (dated tables).
7. **Rates that change:** GST/TDS from our **dated, versioned tables** or caller-supplied. **FX always caller-supplied**; we never fetch it.
8. **No interest maths** (company rule): EMI, compound interest, NPV, IRR/XIRR, present/future value, discounting and loan schedules are refused with a reason.
9. **Upsell pointers:** when a question needs history or optimisation, the answer points to the deeper Saifs MCP.

---

## 3. TOOLS (alphabetical, 32 + 2 helpers)

| # | Tool | Operations inside | Example |
|---|---|---|---|
| 1 | `ad_metrics_calculator` | CTR, CPC, CPM, CPL, ROAS, simple ROI (no time value) | spend 10,000; 50,000 impr; 1,000 clicks → CTR 2%, CPC 10, CPM 200 |
| 2 | `average_statistics_calculator` | average, weighted average, median, mode, std dev (population/sample), variance, percentile, quartiles, z-score | [2,4,4,4,5,5,7,9] → σ 2.0, s 2.138090 |
| 3 | `break_even_calculator` | break-even units and revenue, contribution margin and ratio, target-profit units, margin of safety | F 50,000, P 250, V 150 → 500 units, ₹1,25,000 |
| 4 | `budget_variance_calculator` | actual vs budget (amount, %), favourable/adverse, pro-rata cost allocation by driver | 11.5 L vs 10 L → +1.5 L (+15%) |
| 5 | `currency_converter` | amount × caller-supplied rate, cross rates, rounding to currency minor units | $1,000 × 83.2 = ₹83,200 |
| 6 | `date_due_date_calculator` | days between, working days (caller holiday list), add days / working days, ageing days, month-end | 10 Oct + 30 days = 9 Nov |
| 7 | `depreciation_calculator` | straight-line, written-down value (WDV), schedule by year | WDV 15% on 1,00,000 → 15,000; 12,750 |
| 8 | `discount_calculator` | single discount, chain/successive, trade + cash, MRP back-calculation, discount % from prices | 10% + 5% = 14.5% effective |
| 9 | `eoq_reorder_calculator` | EOQ, reorder point, simple safety stock (z·σ·√L), days of cover, order frequency | D 10,000, S 50, H 2 → EOQ 707.11 |
| 10 | `growth_share_calculator` | growth %, CAGR (on values), market share, contribution to growth, index numbers (base 100) | 100 → 200 in 5 yrs → 14.87% |
| 11 | `gst_calculator` | add GST, split GST-inclusive amount, CGST/SGST vs IGST by place of supply, reverse charge, rate lookup (dated) | 1,180 @ 18% → 1,000 + 180 (CGST 90 + SGST 90) |
| 12 | `inventory_metrics_calculator` | inventory turnover, DIO, fill rate, stock valuation (FIFO, weighted average), dead-stock % | lots 100@10, 100@12 → WAC 11 |
| 13 | `invoice_calculator` | line totals → discounts → tax → round-off → grand total → amount in words | 2×500 + 1×1,250, −10%, +18% → 2,389.50 → **2,390** |
| 14 | `landed_cost_calculator` | allocate freight, duty, insurance and handling to items by value, weight or quantity; landed unit cost | freight 1,000 on 3,000 / 7,000 → 300 / 700 |
| 15 | `margin_markup_calculator` | margin ↔ markup, price from target margin, price from markup, max cost from price and margin | cost 750, margin 25% → price 1,000 (markup 33.33%) |
| 16 | `number_format_calculator` | rounding modes, Indian format (lakh/crore), international format, amount in words, lakh/crore ↔ million/billion | 12345678.5 → 1,23,45,678.50 |
| 17 | `payroll_calculator` | overtime pay, pro-rata salary, leave accrual, gratuity (India statutory formula) | 30,000 × 10 yrs gratuity → ₹1,73,076.92 |
| 18 | `percentage_calculator` | percent of, what percent, % change, percentage points, increase/decrease by %, reverse % | 1,200 after +20% → 1,000 |
| 19 | `production_efficiency_calculator` | OEE, availability/performance/quality, capacity utilisation, takt time, cycle time, yield/scrap, Little's law | 0.90 × 0.95 × 0.98 = 83.79% |
| 20 | `profit_loss_calculator` | profit/loss amount, % on cost, % on sale, gross profit | CP 800, SP 1,000 → +200 (25% on cost) |
| 21 | `ratio_analysis_calculator` | gross/operating/net margin, current ratio, quick ratio, debt-to-equity, working capital, asset turnover | CA 12 L / CL 8 L = 1.5 |
| 22 | `ratio_proportion_calculator` | simplify ratio, split amount by ratio, rule of three, unitary method | ₹50,000 split 3:2 → 30,000 / 20,000 |
| 23 | `sales_funnel_calculator` | conversion by stage, overall conversion, win rate, average deal size, sales velocity, quota attainment, pipeline coverage | 40 × 25% × 50,000 / 30 = ₹16,666.67/day |
| 24 | `shipping_weight_calculator` | volumetric weight (carrier divisor), chargeable weight, CBM, pallet/carton count | 50×40×30 cm / 5,000 = 12 kg |
| 25 | `survey_sample_calculator` | confidence interval for mean/proportion, sample size for a proportion, margin of error | ±5% @ 95% → 385 |
| 26 | `tds_tcs_calculator` | TDS by section with threshold check (dated table or caller rate), TCS, net payable | ₹50,000 fees @ 10% → TDS 5,000, net 45,000 |
| 27 | `tier_pricing_commission_calculator` | slab/volume pricing (all-units or incremental), commission slabs, incentive payout | 1.5 L sales, 5% to 1 L + 7% above → 8,500 |
| 28 | `trend_basic_calculator` | moving average, linear trend (least squares), correlation | 3-month MA; slope & intercept *(forecasting → MCP 18)* |
| 29 | `unit_converter` | weight, length, area, volume, time, temperature, packaging units | 2.5 t = 2,500 kg |
| 30 | `unit_economics_calculator` | CAC, LTV (no discounting), LTV:CAC, CAC payback months, ARPU, MRR/ARR, churn and retention, net revenue retention | ARPU 1,000, 70%, churn 5%, CAC 3,500 → LTV 14,000, 4.0×, 5 months |
| 31 | `working_capital_days_calculator` | DSO, DPO, DIO, cash conversion cycle, ageing buckets (0–30/31–60/61–90/90+) | AR 4 L, revenue 24 L → DSO 60.83 days |
| 32 | `workforce_metrics_calculator` | attrition, average headcount, absenteeism, cost per hire, revenue per employee, billable utilisation | 18 leavers / 120 avg = 15% |
| H1 | `explain_formula` | plain-language explanation and worked example for any operation | |
| H2 | `list_formulas` | catalogue of every operation with inputs, units and the tool that owns it | |

**Tool-description rule:**
- Start with the business task ("Split a GST-inclusive amount…").
- List the operation names **and** common synonyms (margin/markup/mark-up/profit %).
- Name the input fields and say what comes back.
- Say when not to use the tool (e.g. "not for interest or loan EMI").

### Excluded (company rule: no interest-based maths)
EMI, simple or compound interest, NPV, IRR/XIRR, present/future value, discounting, loan amortisation, FD or credit-card returns.

### Built-in upsell pointers
| Question | Answer here | Deeper Saifs MCP |
|---|---|---|
| Safety stock from real demand history | z·σ·√L with given σ | 16 Inventory Optimizer |
| Next month's sales | moving average / trend | 18 Demand Forecaster |
| When will customers actually pay | DSO | 64 13-Week Cash Forecast |
| Best price for this quote | margin maths | 59 Quote Price Optimization |
| Which customers matter / who to call | conversion rates | 14 Customer Segmentation |
| Is my data clean enough to calculate on | — | 67 Data Cleaning & Validation |

---

## 4. BUSINESS OPERATIONS IT HELPS

### By department and process
| Department | Everyday operations it covers | Main tools |
|---|---|---|
| **Sales** | Quoting prices, applying discounts, checking margin before approving a deal, commission and incentive payouts, quota tracking, pipeline coverage, sales velocity | margin_markup, discount, tier_pricing_commission, sales_funnel, profit_loss |
| **Pricing / commercial** | Setting list prices from cost, target-margin pricing, slab/volume price lists, MRP back-calculation, price comparisons across pack sizes | margin_markup, discount, tier_pricing_commission, percentage |
| **Marketing** | Campaign ROAS/ROI, CTR/CPC/CPM/CPL, CAC, LTV:CAC, payback, growth and market-share reporting | ad_metrics, unit_economics, growth_share |
| **Finance & accounts** | Invoice totals and round-off, GST splits for filing prep, TDS/TCS deduction, depreciation schedules, budget vs actual, ratio analysis for MIS, working-capital days, ageing buckets, FX conversion | invoice, gst, tds_tcs, depreciation, budget_variance, ratio_analysis, working_capital_days, currency_converter |
| **Procurement / purchase** | Comparing supplier quotes per unit, landed cost of imports, chain-discount checks, EOQ for order sizing, DPO tracking | landed_cost, discount, eoq_reorder, working_capital_days |
| **Inventory & warehouse** | Reorder points, safety stock, days of cover, stock valuation (FIFO/WAC), turnover and DIO, fill rate, dead-stock % | eoq_reorder, inventory_metrics |
| **Production / manufacturing** | OEE, capacity utilisation, takt and cycle time, yield and scrap, Little's law for WIP, unit conversions | production_efficiency, unit_converter |
| **Logistics & shipping** | Courier chargeable weight, CBM, pallet/carton counts, freight per unit, due dates | shipping_weight, landed_cost, date_due_date |
| **HR & payroll** | Overtime, pro-rata salary for joiners/leavers, leave accrual, gratuity estimates, attrition, absenteeism, cost per hire, utilisation (aggregates only) | payroll, workforce_metrics |
| **Management / MIS reporting** | Growth %, CAGR, % vs pp in board decks, variance commentary, index numbers, averages and spreads | percentage, growth_share, budget_variance, average_statistics |
| **Customer success / subscription business** | MRR/ARR, churn and retention, NRR, ARPU | unit_economics |
| **Retail & e-commerce** | Discount and MRP maths, margin per SKU, unit-price comparisons, shipping weight, GST on orders | discount, margin_markup, gst, shipping_weight |
| **Research / quality checks** | Survey sample size, confidence intervals, z-scores | survey_sample, average_statistics |
| **Admin / documents** | Amount in words for invoices and cheques, Indian number formatting, date and due-date maths | number_format, date_due_date |

### By industry (where call volume will be highest)
Distribution and wholesale · retail and e-commerce · manufacturing (MSME) · import/export and logistics · professional services and agencies · SaaS and subscription businesses · CA firms and bookkeeping · BPO/IT services (workforce metrics) · real-estate and construction (quotes, GST, depreciation).

### Typical agent workflows that call it
1. **Quote agent:** cost + target margin → price → discount check → GST → quote total.
2. **Invoice agent:** order lines → `invoice_calculator` → amount in words → ready to post.
3. **Purchase agent:** supplier quotes → landed cost per unit → compare → EOQ for order size.
4. **MIS agent:** month numbers → growth, margins, variance and ratios → board summary with formulas cited.
5. **HR agent:** joiner date → pro-rata salary → leave accrual.
6. **Marketing agent:** campaign export → ROAS, CAC, payback → keep or cut.

---

## 5. Pressure test (gate pass)
- **G1 Substitution (13/30):** the formulas are free; an LLM or its sandbox gets most simple ones right; Excel does them too. Value comes from determinism, dated rule tables and Indian conventions.
- **G2 Moats (12/25):** little accumulation; witness (formula, version and date on every answer) is the main structural strength.
- **G3 Business (20/25):** breadth ~92 (nearly every business); self-serve; extremely high call volume; all marketplaces; drives discovery to paid MCPs.
- **G4 Time (11/20):** GST/TDS rule changes keep a maintained table valuable; no compounding.
- **Total 56; potential 60.** **Recurrence:** many times a day ✅
- **Verdict:** **Build as a free / near-free entry product.** It isn't a moat product. It's the most-called and most-seen Saifs MCP, feeding users to 14, 16, 18, 59, 64 and 67.

## 6. Data & components (stateless contract)
**Input JSON:** `{tool, operation, inputs{...}, units?, rounding?, locale?, rate_table_version?}`
**Output JSON:** `{result, formula, inputs_used, rounding, rule_version, effective_date, explanation, upsell_hint?}`
Nothing stored. No outside calls (rates from dated internal tables or the caller). Math: Python `decimal`, standard library; shares formatting with 67 and rule tables with 08.

## 7. Monetization
- **Free:** 5,000 calls/month (all tools)
- **Pro ₹830/$10 mo:** 100,000 calls, dated GST/TDS tables with change alerts
- **Business ₹2,500/$29 mo:** unlimited calls, custom formulas and slabs, audit export
- **Bundled free** with any paid Saifs MCP

## 8. India notes & risks
GST slab and TDS section changes are kept as dated tables with effective dates, and old dates give old rates. **No interest maths.** Gratuity and payroll outputs are estimates under the stated formula, not legal advice. Risks: stale rate tables (version and date on every answer; change alerts), wrong units (refused if missing), low moat (positioned as an entry product, priced accordingly). Disclaimer: calculations with formulas shown; not tax, legal or financial advice.

---

## LISTING & DISCOVERABILITY
*Keep this section current. It drives marketplace search, AI tool selection and the buying decision.*

**Who discovers this MCP:** agent developers, developers and agent builders (Copilot Studio makers, Agentforce, Breeze and Gemini Enterprise builders, Claude/ChatGPT connector users). They find it, test it, and **champion the purchase** to the budget owner. Every field below is written for them first, with a business tagline the manager can understand.

### 1. Names
| Field | Use this | Rule |
|---|---|---|
| **Listing title** | **Business Calculator: GST, Margin & Everyday Formulas — Saifs AI** | Capability keyword first; ≤ 60 characters before "— Saifs AI" |
| **Tagline** (for the budget owner) | Exact answers for percentages, profit, GST, invoices, EOQ, DSO and 100+ business formulas | Plain business words; ≤ 90 characters |
| **Server / connector ID** | `saifs-business-calculator` | Lowercase, hyphenated, no coined words |
| **Internal code name** | Business Calculator (doc 68) | Docs and code only, **never** in a listing |

### 2. Description: the first 160 characters decide the click
1. **Developer contract:** Tool + operation + inputs (JSON) → exact result with formula, inputs, rounding rule and rule version.
2. **Business value (for the champion):** No arithmetic slips in agent answers; every number shows its formula, rounding and rule date.
3. **Methods (specialist search and credibility):** uses decimal arithmetic, dated GST/TDS tables, CVP analysis, EOQ, OEE.
4. **Trust:** stateless; data is processed in the request and never stored; per-user OAuth sign-in. No interest calculations; GST/TDS rates dated and versioned.

### 3. Keywords
- **Developer search terms (title, description, tags):** business calculator, gst calculator, profit margin calculator, markup calculator, discount calculator, break even calculator, invoice calculator, eoq calculator, percentage calculator, kpi calculator
- **Business phrases (tagline, FAQ, tool descriptions):** "what is 18% of", "calculate profit", "split GST", "break-even point", "amount in words"
- **Method terms (description body):** decimal arithmetic, dated GST/TDS tables, CVP analysis, EOQ, OEE
- **Platform terms (only where actually listed):** MCP server, Copilot Studio connector, Agentforce action, Breeze agent tool, Gemini Enterprise, Amazon Bedrock AgentCore, Claude connector, ChatGPT app

### 4. Tools as shown in the listing (alphabetical)
`ad_metrics_calculator` · `average_statistics_calculator` · `break_even_calculator` · `budget_variance_calculator` · `currency_converter` · `date_due_date_calculator` · `depreciation_calculator` · `discount_calculator` · `eoq_reorder_calculator` · `explain_formula` · `growth_share_calculator` · `gst_calculator` · `inventory_metrics_calculator` · `invoice_calculator` · `landed_cost_calculator` · `list_formulas` · `margin_markup_calculator` · `number_format_calculator` · `payroll_calculator` · `percentage_calculator` · `production_efficiency_calculator` · `profit_loss_calculator` · `ratio_analysis_calculator` · `ratio_proportion_calculator` · `sales_funnel_calculator` · `shipping_weight_calculator` · `survey_sample_calculator` · `tds_tcs_calculator` · `tier_pricing_commission_calculator` · `trend_basic_calculator` · `unit_converter` · `unit_economics_calculator` · `working_capital_days_calculator` · `workforce_metrics_calculator`

**Tool-description rule:** the AI picks tools from these descriptions, so they count as discoverability too.
- Start with a verb and the business noun.
- Name the input fields.
- Say what comes back.
- Say when **not** to use the tool.

### 5. Example prompts (listing, demo video, tool descriptions)
- "What price gives 25% margin on cost 750?"
- "Split ₹1,180 into base and GST"
- "Break-even units for fixed 50,000, price 250, cost 150"

### 6. Categories per marketplace
| Marketplace | Category / tags |
|---|---|
| Microsoft (Copilot Studio certified connector / AppSource) | Finance; Analytics; Compliance |
| Salesforce AgentExchange | Finance / Service |
| AWS Marketplace: AI Agents & Tools | Finance & Accounting |
| Google Cloud Marketplace / Gemini Agent Gallery | Finance |
| HubSpot Marketplace (Breeze) | Finance / Operations |

### 7. Developer try-it kit (ship with the listing)
- Free tier that works in under 10 minutes, no sales call
- Sample JSON payload shaped like a real export from: any values from the user, files or other MCPs
- One copy-paste example call **and** its response
- A 60–90 second demo video: prompt → tool call → answer
- Link to the docs and the MCP/OpenAPI schema; privacy policy and terms

### 8. Champion kit (for the developer to forward upward)
- **One-line business case:** No arithmetic slips in agent answers; every number shows its formula, rounding and rule date. Always as a range, never a bare number.
- **Security note:** stateless, nothing stored, per-user OAuth, DPDP/GDPR-friendly
- **Price:** the tier table from §Monetization, with the free tier stated first

### 9. Don'ts
- Coined names (e.g. "Business Calculator") in titles or tags
- "Best", "#1" or "guaranteed" claims
- Bare money figures
- Promises beyond the guardrail: No interest calculations; GST/TDS rates dated and versioned
