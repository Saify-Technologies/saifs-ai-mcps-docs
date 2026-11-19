# 64 · 13-Week Cash Forecast: Behaviour-Based Receipts & Disbursements Forecasting MCP
**Initial moat: 73/100 (potential 77) · IW-Breadth: 80 · Phase: Wave 7 · Finance Operations bundle (58, 37, 31)**
**Classical models: Receipts-and-disbursements direct cash forecasting (classic treasury practice) · Per-customer days-to-pay distributions (empirical / Kaplan-Meier survival 1958) · Monte Carlo cash-balance simulation (Metropolis-Ulam 1949) · Forecast accuracy tracking (MAPE, bias, tracking signal; Trigg 1964)**

## 1. Problem statement
"Will we have enough cash in week 9?" is the question finance teams, founders and lenders ask most often, and the one AI agents are now asked to answer. The usual 13-week cash forecast is a spreadsheet that assumes customers pay on their due date. They don't: some pay on time, some 2 weeks late, some 6. The forecast is wrong by the time it's emailed. ERPs show receivable and payable ageing, not when cash will actually arrive. Enterprise treasury suites (Kyriba, HighRadius) model payment behaviour for large companies. SMB cash apps are mostly due-date based. An LLM asked to sum invoices by week gives a single number with no range and no memory of how each customer behaves.

## 2. Solution
Open receivables and payables, payroll, recurring items and payment history in → week-by-week cash in, cash out and closing balance for 13 weeks with P10/P50/P90, minimum-cash breach probability, the customers and bills that move the result, and accuracy versus last forecast out.

**Tool set (v1, alphabetical):**
- `accuracy_report(previous_forecast, actuals)`: forecast vs actual by week, bias, MAPE; trust builds over time
- `breach_risk(forecast, minimum_cash)`: probability cash falls below the minimum in each week, and the earliest risky week
- `explain_week(forecast, week)`: which receipts and payments drive that week, and what would change it
- `forecast_cash(ar_open, ap_open, payroll, recurring, opening_balance, behaviour_snapshot)`: 13-week P10/P50/P90 forecast
- `payment_behaviour(ar_history)`: days-to-pay distribution per customer (and per segment for new customers), returned as `payment_behaviour_snapshot`
- `what_if(forecast, change)`: scenario such as "Customer X pays 3 weeks late", "delay supplier Y by 2 weeks", or "large order lands in week 5"

## 3. THE ALGORITHMS
### Payment behaviour
For each customer, delay `d = paid_date − due_date` from history. Empirical distribution (or Kaplan-Meier where invoices are still open and censored). Customers with under 5 paid invoices borrow their segment's distribution (shrinkage). Partial-payment patterns are captured as split probabilities.
### Receipts forecast
Each open invoice's amount is spread across future weeks by `P(d falls in week w)`. Overdue invoices use the conditional distribution `P(d | d > days already late)`.
### Disbursements
Payables by the company's own payment-run pattern (actual days-to-pay to suppliers), payroll and statutory dues on fixed dates, recurring items, and caller-supplied fixed amounts for any loan repayments. **No interest is computed.**
### Monte Carlo balance
Simulate 5,000 paths of receipt timing (and optional new-sales assumptions). Closing balance per week gives P10/P50/P90, and `P(balance < minimum)`.
### Accuracy loop
Each week compare the forecast with actuals: MAPE, bias and tracking signal. Persistent bias shifts the behaviour weights in the next snapshot.

## 4. Pressure test
- **G1 Substitution (25/30):** a sandbox can sum by due date; behaviour-based timing with ranges and an accuracy loop is not a one-off script. Threats: enterprise treasury tools; rule-based SMB apps.
- **G2 Moats (18/25):** weekly continuity, accumulated behaviour (caller-held snapshot), witness (forecast vs actual log).
- **G3 Business (18/25):** every finance team; data already in ERP/Tally ageing; self-serve; per-entity billing.
- **G4 Time (12/20):** accuracy compounds week by week. **Total 73; potential 77.**
- **Recurrence:** weekly (daily in tight months) ✅

## 5. Audience & user profiles
CFOs and finance controllers, founders of SMBs and startups, treasury analysts, fractional CFOs and CA firms advising clients, FP&A teams.
1. **Founder:** "Week 9 has a 27–35% chance of dipping below ₹10 lakh. It's driven by two customers who usually pay 3 weeks late."
2. **Finance controller:** a 13-week forecast every Monday in the board pack, with last week's accuracy shown.
3. **Fractional CFO:** the same forecast format across 15 client companies.

## 6. Data & components (stateless contract)
**Input JSON:** `ar_open[{invoice_ref, customer_ref, amount, due_date}]`, `ar_history[{customer_ref, due_date, paid_date, amount}]`, `ap_open[{bill_ref, supplier_ref, amount, due_date}]`, `payroll[{date, amount}]`, `recurring[{name, amount, schedule}]`, `fixed_outflows[{date, amount, label}]`, `opening_balance`, `minimum_cash`, optional `payment_behaviour_snapshot`, `previous_forecast`.
**Output JSON:** weekly forecast with ranges, breach risk, drivers, accuracy, updated snapshot.
Data comes from the caller's ERP/Tally/Zoho export or an ERP MCP. **No bank connections from us.** Nothing stored. Math: NumPy Monte Carlo, Kaplan-Meier (shared with 22), propensity features (shared with 37).
**Refusal thresholds:** under 3 months of payment history, the forecast is labelled "terms-based (due dates only)."

## 7. Monetization
- Free: 1 entity, 50 open invoices
- Pro ₹4,100/$49 mo: 1 entity, unlimited invoices, scenarios, accuracy tracking
- Business ₹12,400/$149 mo: 10 entities, consolidated view, board-pack export
- Advisor plan (CA firms / fractional CFOs): per client entity
- Finance Operations bundle with 58, 37, 31

## 8. India notes & risks
Fits Indian SMBs where long receivable cycles (60–120 days) and GST/TDS payment dates drive cash crunches. Statutory dues (GST, TDS, PF/ESI) are templated as fixed-date outflows. **No interest logic:** no borrowing, investment, overdraft or rate advice; loan repayments only as fixed amounts the caller supplies. Pseudonymous customer references. Risks: thin history (segment shrinkage, labelled), one-off large receipts (what-if tool), over-trust (always ranges and accuracy shown). Disclaimer: estimates; not financial advice.

---

## LISTING & DISCOVERABILITY
*Keep this section current. It drives marketplace search, AI tool selection and the buying decision.*

**Who discovers this MCP:** agent developers, developers and agent builders (Copilot Studio makers, Agentforce, Breeze and Gemini Enterprise builders, Claude/ChatGPT connector users). They find it, test it, and **champion the purchase** to the budget owner. Every field below is written for them first, with a business tagline the manager can understand.

### 1. Names
| Field | Use this | Rule |
|---|---|---|
| **Listing title** | **13-Week Cash Flow Forecast — Saifs AI** | Capability keyword first; ≤ 60 characters before "— Saifs AI" |
| **Tagline** (for the budget owner) | Week-by-week cash forecast for 13 weeks, based on how customers actually pay | Plain business words; ≤ 90 characters |
| **Server / connector ID** | `saifs-cash-forecast` | Lowercase, hyphenated, no coined words |
| **Internal code name** | 13-Week Cash Forecast (doc 64) | Docs and code only, **never** in a listing |

### 2. Description: the first 160 characters decide the click
1. **Developer contract:** Open receivables, payables, payroll + payment history (JSON) → 13-week cash forecast P10/P50/P90, breach risk, drivers.
2. **Business value (for the champion):** Cash crunches seen 4–10 weeks earlier; forecast accuracy tracked every week.
3. **Methods (specialist search and credibility):** uses receipts & disbursements, Kaplan-Meier days-to-pay, Monte Carlo.
4. **Trust:** stateless; data is processed in the request and never stored; per-user OAuth sign-in. No interest, borrowing or investment advice; estimates as ranges.

### 3. Keywords
- **Developer search terms (title, description, tags):** cash flow forecast, 13 week cash forecast, cash forecasting, receivables forecast, treasury forecasting, liquidity forecast, working capital
- **Business phrases (tagline, FAQ, tool descriptions):** "will we have enough cash", "cash forecast for the board", "when will customers pay"
- **Method terms (description body):** receipts & disbursements, Kaplan-Meier days-to-pay, Monte Carlo
- **Platform terms (only where actually listed):** MCP server, Copilot Studio connector, Agentforce action, Breeze agent tool, Gemini Enterprise, Amazon Bedrock AgentCore, Claude connector, ChatGPT app

### 4. Tools as shown in the listing (alphabetical)
`accuracy_report` · `breach_risk` · `explain_week` · `forecast_cash` · `payment_behaviour` · `what_if`

**Tool-description rule:** the AI picks tools from these descriptions, so they count as discoverability too.
- Start with a verb and the business noun.
- Name the input fields.
- Say what comes back.
- Say when **not** to use the tool.

### 5. Example prompts (listing, demo video, tool descriptions)
- "Will we have enough cash in week 9?"
- "Build our 13-week cash forecast"
- "What if our top customer pays 3 weeks late?"

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
- Sample JSON payload shaped like a real export from: Tally, ERP, Zoho Books, AR/AP ageing exports
- One copy-paste example call **and** its response
- A 60–90 second demo video: prompt → tool call → answer
- Link to the docs and the MCP/OpenAPI schema; privacy policy and terms

### 8. Champion kit (for the developer to forward upward)
- **One-line business case:** Cash crunches seen 4–10 weeks earlier; forecast accuracy tracked every week. Always as a range, never a bare number.
- **Security note:** stateless, nothing stored, per-user OAuth, DPDP/GDPR-friendly
- **Price:** the tier table from §Monetization, with the free tier stated first

### 9. Don'ts
- Coined names (e.g. "13-Week Cash Forecast") in titles or tags
- "Best", "#1" or "guaranteed" claims
- Bare money figures
- Promises beyond the guardrail: No interest, borrowing or investment advice; estimates as ranges
