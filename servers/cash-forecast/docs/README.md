# 13-Week Cash Flow Forecast

[SAIFS AI](https://saifs.ai) (`saifs.ai`) publishes this as a remote MCP server under the registry name `ai.saifs/cash-forecast`. It builds a 13-week direct cash forecast from open receivables, open payables, payroll, recurring items, and the way each customer has actually paid.

People search for this as a cash flow forecast, a 13-week cash forecast, cash forecasting, a receivables forecast, treasury forecasting, a liquidity forecast, or a working-capital forecast. The short install page is the [server README](../README.md). This page is what the server is, why a due-date spreadsheet misses the cash, and how to use each tool.

| | |
|---|---|
| **Registry name** | `ai.saifs/cash-forecast` |
| **Endpoint** | `https://cash-forecast-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## What it is

Finance teams, founders, and lenders ask whether there is enough cash in a given week. This server answers that with a week-by-week receipts-and-disbursements forecast for 13 weeks:

- Cash in, cash out, and closing balance.
- A range on each week: P10, P50, and P90, from 5,000 simulated paths of when receipts land.
- The probability that the balance falls below a minimum you set, and the earliest week that probability matters.
- The customers and bills that move the week.
- Accuracy against the previous forecast, once you send actuals.

A single days-sales-outstanding figure is a different question. That ratio lives on the [Business Calculator](../../business-calculator/README.md). This server is the week the cash arrives.

The call is stateless. Send the open items, the history, and, on a later run, the behaviour snapshot from the last call. Nothing is stored, and the server does not connect to a bank. Data comes from your Tally, Zoho Books, ERP, or ageing export. Customer and supplier references should be pseudonymous.

## Why a due date is not a cash date

The usual 13-week forecast puts each invoice in the week it falls due. Customers do not pay that way. Some pay on time, some two weeks late, some six. The spreadsheet is already wrong when it is emailed. An ageing report shows how late the book is. It does not say which week the money will arrive. A language model asked to sum invoices by week returns one number, with no range and no memory of how each customer behaves.

Teams that replace the due-date assumption with payment behaviour see the cash crunch 4–10 weeks earlier, and they can show whether last week’s forecast was high, low, or on track.

Two situations this is built for in Indian books:

- Receivable cycles that run long, often 60–120 days, so the due-date week and the cash week are far apart.
- Statutory outflows on known dates (GST, TDS, PF, ESI). Those enter as fixed-date payments you supply. The server does not compute interest, and it does not advise on borrowing, an overdraft, or where to invest a surplus. A loan repayment enters only as a fixed amount and date you provide.

Figures are estimates, shown as ranges. They are not financial advice.

## How to use it

Connect the endpoint in the [install README](../README.md) and sign in when the client asks. A Monday run is four steps. You hold the snapshot and the forecast between calls, because the server does not.

1. **`payment_behaviour`** on paid-invoice history. You get a `payment_behaviour_snapshot`.
2. **`forecast_cash`** with open receivables, open payables, payroll, recurring items, opening balance, and that snapshot.
3. **`breach_risk`** with the minimum cash you care about. **`explain_week`** on any week the board will ask about.
4. **`what_if`** when one customer, one supplier, or one large order changes. Next week, **`accuracy_report`** compares that forecast with what actually happened, and the next snapshot absorbs a persistent bias.

### What you send

| Input | Fields | Where it comes from |
|---|---|---|
| `ar_open` | `invoice_ref`, `customer_ref`, `amount`, `due_date` | Open receivables, ageing export |
| `ar_history` | `customer_ref`, `due_date`, `paid_date`, `amount` | Paid invoices, the behaviour source |
| `ap_open` | `bill_ref`, `supplier_ref`, `amount`, `due_date` | Open payables |
| `payroll` | `date`, `amount` | Payroll calendar |
| `recurring` | `name`, `amount`, `schedule` | Rent and other repeats |
| `fixed_outflows` | `date`, `amount`, `label` | GST, TDS, PF, ESI, and any loan repayment you already know |
| `opening_balance` | One amount | The cash you hold today |
| `minimum_cash` | One amount | The floor you set, not a figure the server invents |
| `payment_behaviour_snapshot` | The object from the last `payment_behaviour` or `forecast_cash` | Your own store. Optional on the first run |
| `previous_forecast` | Last week’s weekly forecast | Your own store, for `accuracy_report` |

```json
{
  "ar_open": [
    {"invoice_ref": "INV-1042", "customer_ref": "CUST-18", "amount": "450000", "due_date": "2026-11-20"}
  ],
  "ar_history": [
    {"customer_ref": "CUST-18", "due_date": "2026-08-01", "paid_date": "2026-08-22", "amount": "450000"}
  ],
  "ap_open": [
    {"bill_ref": "BILL-88", "supplier_ref": "SUP-4", "amount": "120000", "due_date": "2026-11-25"}
  ],
  "payroll": [{"date": "2026-11-30", "amount": "800000"}],
  "recurring": [{"name": "Rent", "amount": "150000", "schedule": "month-end"}],
  "fixed_outflows": [{"date": "2026-12-07", "amount": "90000", "label": "GST"}],
  "opening_balance": "2500000",
  "minimum_cash": "1000000"
}
```

With under three months of payment history, the forecast is labelled **terms-based (due dates only)**. It still runs. The label tells you the timing is the due date, because there is not enough paid history to speak for behaviour.

### What you get

A weekly forecast with cash in, cash out, and closing balance at P10, P50, and P90. Breach risk by week and the earliest risky week. The receipts and payments that drive a week. Accuracy against the last forecast, when you send it. An updated behaviour snapshot to store and send next time.

A founder’s question comes back in that shape: “Week 9 has a 27–35% chance of dipping below ₹10 lakh. It is driven by two customers who usually pay three weeks late.” The range is the answer. A controller puts the same 13 weeks in the Monday board pack and shows last week’s accuracy beside it. A fractional CFO uses one format across client companies. The advisor plan bills per client entity.

### Prompts

- “Will we have enough cash in week 9?”
- “Build our 13-week cash forecast.”
- “What if our top customer pays 3 weeks late?”
- “Which receipts and bills drive week 9, and what would move it?”
- “How far off was last week’s forecast, and is the bias persistent?”
- “How does this customer actually pay, compared with their terms?”

## Tools

| Tool | What you send | What comes back | Do not use it when |
|---|---|---|---|
| `accuracy_report` | `previous_forecast`, actuals by week | Forecast versus actual, bias, and MAPE. A persistent bias shifts the behaviour weights in the next snapshot | You are building this week’s forecast and have no prior forecast to score |
| `breach_risk` | A forecast and `minimum_cash` | The probability the balance falls below that minimum in each week, and the earliest risky week | You want advice on a loan, an overdraft, or where to place surplus cash |
| `explain_week` | A forecast and a week number | The receipts and payments that drive that week, and what would change it | You still need the forecast itself. Run `forecast_cash` first |
| `forecast_cash` | `ar_open`, `ap_open`, `payroll`, `recurring`, `opening_balance`, and a `payment_behaviour_snapshot` | Thirteen weeks of cash in, cash out, and closing balance at P10, P50, and P90 | You want one due-date total and no range, or you want interest calculated on the balance |
| `payment_behaviour` | `ar_history` | Days-to-pay distribution per customer, and per segment for thin history, as a `payment_behaviour_snapshot` | You want the cash balance. This tool builds the timing the forecast uses |
| `what_if` | A forecast and one change | The 13 weeks again under that change: a customer pays 3 weeks late, supplier Y slips 2 weeks, a large order lands in week 5 | The change is a new interest rate or a funding product. Describe cash you already know as a fixed amount and date |

## How the weeks are built

### Payment behaviour

For each customer, delay is `paid_date − due_date` on the history you send. That is an empirical distribution of days late (or early). Invoices still open are censored, and those are handled with Kaplan-Meier (1958) so an unpaid invoice is not treated as if it will never pay and not treated as if it paid on the due date. A customer with fewer than five paid invoices borrows the distribution of their segment. Partial payments are split probabilities, not a single pay date.

### Receipts

Each open invoice is spread across future weeks by the probability that this customer’s delay falls in that week. An invoice that is already late uses the conditional distribution: the probability of further delay given how late it already is. Cash in for a week is the sum of those probability-weighted amounts.

### Disbursements

Payables follow your own payment-run pattern, the days you actually take to pay suppliers, not only the bill’s due date. Payroll and statutory dues sit on the dates you give them. Recurring items follow their schedule. Anything else you already know, including a loan repayment, is a fixed amount on a fixed date. No interest is computed on any of these.

### The range

Receipt timing is simulated over 5,000 paths (Monte Carlo, Metropolis and Ulam, 1949). Optional assumptions about new sales can be included in those paths. Each week’s closing balances become P10, P50, and P90, and the share of paths below `minimum_cash` is the breach probability. P50 is the midline. P10 and P90 are the band. Quoting only P50 throws away the reason the forecast is a range.

### Accuracy

Each week you can compare the forecast with actuals. The report is MAPE, bias, and a tracking signal (Trigg, 1964). Bias that keeps the same sign is fed into the next behaviour snapshot, which you store and send back. Trust is the record of those comparisons, not a promise about the next quarter.

## How to read a result

- **P10 / P50 / P90.** Half the simulated paths close the week at or below P50. The band from P10 to P90 is the range to put in the pack. A wide band means timing is uncertain, which is a finding, not a failure of the sheet.
- **Breach probability.** “27–35% in week 9” means that share of paths fell below the minimum you set. It is not a recommendation to borrow.
- **Drivers.** `explain_week` names the receipts and payments that move that week. A week driven by two slow customers is a collections conversation. A week driven by GST and payroll is a calendar fact.
- **Terms-based label.** Under three months of history, behaviour is not claimed. The forecast is due dates only, and it says so.
- **Thin customers.** Under five paid invoices, that customer uses the segment’s distribution. The snapshot says so. A one-off large receipt belongs in `what_if`, where you can see the week with it and without it.
- **Accuracy.** A forecast that has been high for several weeks should be read with that bias, not as a fresh unbiased number.

## Data and what this will not do

Send pseudonymous customer and supplier references. The server processes the request and does not store it. It does not connect to a bank, and it does not fetch balances. Each user signs in with OAuth. Your own duties under India’s Digital Personal Data Protection Act, the GDPR, or any other law that applies to the exports you send stay with you.

The server will not compute interest, suggest borrowing, size an overdraft, or recommend an investment. Estimates are ranges, with accuracy beside them once you have a prior forecast.

Privacy policy: https://saifs.ai/privacy_policy

## Plans

| Tier | What it covers |
|---|---|
| **Free** | 1 entity, 50 open invoices |
| **Pro** | ₹4,100 / $49 per month — 1 entity, unlimited invoices, scenarios, accuracy tracking |
| **Business** | ₹12,400 / $149 per month — 10 entities, a consolidated view, board-pack export |
| **Advisor** | CA firms and fractional CFOs, priced per client entity |

The free tier is enough to run one company through a 13-week forecast with no sales call. Current terms are on [saifs.ai](https://saifs.ai).

## Support

- Install and client setup: [server README](../README.md)
- Everyday cash ratios (DSO, DPO, the cash conversion cycle): [Business Calculator](../../business-calculator/README.md)
- Website: https://saifs.ai
- Registry: https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
