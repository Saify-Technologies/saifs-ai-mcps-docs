# 58 · Payment Matching & Reconciliation: Receipt-to-Invoice, Bank & GST Matching MCP
**Initial moat: 75/100 (potential 78) · IW-Breadth: 85 · Phase: Wave 6 (build first)**
**Classical models: Subset-sum dynamic programming (Bellman 1957) · Hungarian assignment (Kuhn 1955) · Jaro-Winkler string similarity (Jaro 1989; Winkler 1990) · Tolerance-window rules · Many-to-many matching as bipartite assignment**

## 1. Problem statement
Every finance team spends hours each day matching money in and out to documents:
- receipts against open invoices
- payments against supplier bills
- bank statement lines against the ledger
- in India, the supplier's GSTR-2B against the purchase register (input tax credit is lost on unmatched invoices)

The hard cases are where time goes:
- one payment covering 3 invoices
- a part payment
- a payment net of TDS or bank charges
- a payer name that differs from the customer master ("SHARMA TRDRS" vs "Sharma Traders Pvt Ltd")
- invoice numbers typed differently

ERPs and Tally/Zoho auto-match only exact amount + reference. Enterprise cash-application suites (HighRadius, BlackLine, Billtrust) are priced for large companies. Mid-market teams match by hand in Excel.

## 2. Solution
Bank lines or receipts + open items + matching rules in → proposed matches with confidence and reason, deductions identified, exceptions queue and updated rules out. **Low-confidence matches always go to a person; nothing is auto-posted by us.**

**Tool set (v1, alphabetical):**
- `explain_match(match_id)`: why these items match (amount combination, name score, date gap, reference)
- `gst_2b_match(gstr2b_rows, purchase_register, tolerance)`: India mode; matched, mismatched (amount/date/GSTIN), missing-in-books, missing-in-2B, with ITC at risk
- `match_bank_to_ledger(bank_lines, ledger_entries, rules)`: bank reconciliation with one-to-many and many-to-one matches
- `match_receipts(receipts, open_invoices, rules_snapshot)`: cash application, including combinations, part payments and deductions
- `reconciliation_report(results)`: matched %, unmatched value, ageing of exceptions, ITC at risk
- `suggest_rules(confirmed_matches)`: new matching rules learned from the user's confirmations, returned in `rules_snapshot`

## 3. THE ALGORITHMS
### Candidate generation
For each payment, filter open items by counterparty score ≥ threshold and date window (e.g. invoice date ≤ payment date + 5 days).
### Subset-sum (combination matching)
Find invoice subsets whose total equals the payment within tolerance. DP over amounts in paise (integer), bounded by max items per match (default 8) and a tolerance band. When several subsets match, rank by oldest-first, fewest items and reference hits.
### Deductions
Test `payment = Σ invoices × (1 − tds_rate) − bank_charge` for the customer's known TDS rates (e.g. 1%, 2%, 10%) and charge bands. Flag short payments as disputes, not matches.
### Name similarity
Normalise (upper-case, strip "PVT LTD", "LLP", punctuation), then Jaro-Winkler plus token-set overlap. Learned aliases from `rules_snapshot` override.
### Global assignment
When many payments compete for the same invoices, solve the bipartite assignment (Hungarian / min-cost flow) maximising total match score, so one invoice is never used twice.
### Confidence
Exact reference + amount: 0.99. Combination + strong name: 0.85–0.95. Name-only: suggested (< 0.7) → human queue.

## 4. Pressure test
- **G1 Substitution (25/30):** the LLM alone is unreliable at combination search. A sandbox handles one small case, not thousands of lines daily with fuzzy rules. Threats: enterprise suites at the top, and cheap Indian GST software already does basic 2B matching.
- **G2 Moats (18/25):** continuity (daily), witness (audit trail per match), operations (search at scale). Learned rules live in the caller's snapshot.
- **G3 Business (19/25):** every company is a buyer; data is already in ERP and bank exports; per-transaction billing; self-serve.
- **G4 Time (13/20):** e-invoicing and GST tightening; rules improve with use. **Total 75; potential 78.**
- **Recurrence:** daily ✅

## 5. Audience & user profiles
AR/AP accountants, finance controllers, shared-service centres, CA firms doing bookkeeping and GST for clients, BPO finance-and-accounting teams.
1. **AR accountant at a distributor:** 640 receipts today; 571 matched automatically, 52 suggested, 17 exceptions, down from a full day of work to about an hour.
2. **CA firm GST associate:** monthly 2B reconciliation for 40 clients; ITC at risk ₹3.2–3.6 lakh flagged with supplier list.
3. **Controller:** month-end bank reconciliation ready on day 1 instead of day 5.

## 6. Data & components (stateless contract)
**Input JSON:** `receipts|bank_lines[{id, date, amount, narration, payer_name, ref}]`, `open_items[{doc_id, party_ref, party_name, date, amount, balance}]`, `rules_snapshot{aliases, tds_rates, tolerances}`, or for GST `gstr2b_rows[]` + `purchase_register[]`.
**Output JSON:** matches with confidence and reasons, exceptions, deductions, report, updated `rules_snapshot`.
Data comes from the caller's ERP, Tally export, bank statement or a bank/GST MCP. **We never connect to banks or the GST portal.** Nothing stored. Math: NumPy DP, `scipy.optimize.linear_sum_assignment`, rapidfuzz-style similarity (OSS).
**Refusal thresholds:** none for exact matching. Fuzzy matches are labelled "suggested" until the user confirms a rule.

## 7. Monetization
- Free: 200 lines/month
- Pro ₹2,500/$29 mo: 5,000 lines, GST 2B mode, rules
- Business ₹8,300/$99 mo: 50,000 lines, multi-entity, bank + AR + AP
- CA-firm plan: per client GSTIN, volume tiers
- Overage: per 1,000 lines

## 8. India notes & risks
GSTR-2B mismatches directly cost ITC, so the ROI is immediate. Supports TDS-net receipts and UPI narrations. DPDP: payer names of proprietors are personal data, processed in-request only, pseudonymised references returned where possible, nothing stored. **Not interest-related:** matching only, no payment-timing or discount advice. Risks: wrong auto-match (confidence thresholds and human queue), duplicate use of an invoice (global assignment), bank-format variety (parsers per bank export). Disclaimer: not tax or accounting advice; the accountant posts entries.

---

---

## LISTING & DISCOVERABILITY
*Keep this section current. It drives marketplace search, AI tool selection and the buying decision.*

**Who discovers this MCP:** agent developers, developers and agent builders (Copilot Studio makers, Agentforce, Breeze and Gemini Enterprise builders, Claude/ChatGPT connector users). They find it, test it, and **champion the purchase** to the budget owner. Every field below is written for them first, with a business tagline the manager can understand.

### 1. Names
| Field | Use this | Rule |
|---|---|---|
| **Listing title** | **Payment Reconciliation & Matching — Saifs AI** | Capability keyword first; ≤ 60 characters before "— Saifs AI" |
| **Tagline** (for the budget owner) | Match bank receipts and payments to invoices automatically | Plain business words; ≤ 90 characters |
| **Server / connector ID** | `saifs-payment-reconciliation` | Lowercase, hyphenated, no coined words |
| **Internal code name** | Payment Matching & Reconciliation (doc 58) | Docs and code only, **never** in a listing |

### 2. Description: the first 160 characters decide the click
1. **Developer contract:** Bank lines + open invoices (JSON) → matches with confidence, deductions, exceptions.
2. **Business value (for the champion):** Manual matching effort cut 60–85%.
3. **Methods (specialist search and credibility):** uses subset-sum, Hungarian, Jaro-Winkler.
4. **Trust:** stateless; data is processed in the request and never stored; per-user OAuth sign-in. Never auto-posts; human queue for low confidence.

### 3. Keywords
- **Developer search terms (title, description, tags):** bank reconciliation, payment matching, cash application, accounts receivable automation, gstr 2b reconciliation, invoice matching
- **Business phrases (tagline, FAQ, tool descriptions):** "match payments to invoices", "reconcile the bank"
- **Method terms (description body):** subset-sum, Hungarian, Jaro-Winkler
- **Platform terms (only where actually listed):** MCP server, Copilot Studio connector, Agentforce action, Breeze agent tool, Gemini Enterprise, Amazon Bedrock AgentCore, Claude connector, ChatGPT app

### 4. Tools as shown in the listing (alphabetical)
`explain_match` · `match_bank_to_ledger` · `match_receipts` · `reconciliation_report` · `suggest_rules`

**Tool-description rule:** the AI picks tools from these descriptions, so they count as discoverability too.
- Start with a verb and the business noun.
- Name the input fields.
- Say what comes back.
- Say when **not** to use the tool.

### 5. Example prompts (listing, demo video, tool descriptions)
- "Match today's receipts to open invoices"
- "Reconcile the bank statement"
- "Run GSTR-2B vs purchase register"

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
- Sample JSON payload shaped like a real export from: Tally, ERP, bank statements, GST exports
- One copy-paste example call **and** its response
- A 60–90 second demo video: prompt → tool call → answer
- Link to the docs and the MCP/OpenAPI schema; privacy policy and terms

### 8. Champion kit (for the developer to forward upward)
- **One-line business case:** Manual matching effort cut 60–85%. Always as a range, never a bare number.
- **Security note:** stateless, nothing stored, per-user OAuth, DPDP/GDPR-friendly
- **Price:** the tier table from §Monetization, with the free tier stated first

### 9. Don'ts
- Coined names (e.g. "Payment Matching & Reconciliation") in titles or tags
- "Best", "#1" or "guaranteed" claims
- Bare money figures
- Promises beyond the guardrail: Never auto-posts; human queue for low confidence
