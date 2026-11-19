# 31 · Financial Anomaly Detector — Benford's Law & Forensic Analytics MCP
**Score: 75/100 · IW-Breadth: 58 · Phase: NEAR (pairs with Verification Bureau)**
**Classical models: Benford's Law (Newcomb 1881 / Benford 1938; Nigrini forensic application 1990s) · Z-score/outlier detection · Beneish M-Score (1999)**

## Problem statement
Finance, audit, and procurement teams sift through invoices, expense claims, journal entries, and vendor payments where fabricated or manipulated numbers hide. Manual review catches little; fraud and error slip through. Benford's Law (the counterintuitive fact that genuine numeric data has a predictable first-digit distribution) has been the forensic-accounting standard for detecting fabricated figures since the 1990s, and the Beneish M-Score flags earnings manipulation — but these live in audit-software suites most mid-market finance teams don't have. An LLM can't run Benford analysis rigorously across large datasets or track anomalies continuously.

## Solution
Forensic-analytics MCP over the customer's financial/transaction data — flag statistical anomalies for human review (never accuse; surface for investigation).

**Tool set (v1):**
- `benford_analysis(numeric_dataset)` → first/second-digit distribution vs. expected, chi-square, flagged deviations
- `flag_anomalous_transactions(transactions)` → statistical outliers (amounts, round-number bias, duplicate patterns)
- `detect_duplicate_payments(invoices)` → near-duplicate/split-invoice detection
- `earnings_manipulation_score(financials)` → Beneish M-Score (public-company screening)
- `watch_transactions(source)` → continuous Benford/anomaly monitoring, alert on deviation
- `explain_flag(item)` → why flagged, statistical basis (for the human investigator)

## THE ALGORITHMS (implementation guide)
### Benford's Law (Benford 1938, Nigrini forensic 1990s)
Expected first-digit frequency `P(d) = log₁₀(1 + 1/d)` (digit 1 ≈ 30.1%, digit 9 ≈ 4.6%). Compare observed vs. expected via chi-square / mean-absolute-deviation (MAD); Nigrini's conformity thresholds classify close/acceptable/nonconforming. Second-digit and first-two-digit tests for deeper analysis. Applies to naturally-occurring financial data (spanning orders of magnitude); **know when it does NOT apply** (assigned numbers, bounded ranges) — a naive tool misuses it.
### Anomaly/outlier detection
Z-score, IQR, round-number frequency (fraudsters love round numbers), duplicate/near-duplicate matching (fuzzy). `scipy`/`scikit-learn`.
### Beneish M-Score (1999)
8-variable formula from financial ratios flagging likely earnings manipulation; threshold > −1.78 suggests manipulation risk. Reference: Nigrini's forensic-analytics texts.

## Pressure test
G1 (26/30): Benford across large data + continuous monitoring is beyond interpreter's reliable one-shot; the rigorous, applicability-aware, watched version is the product. Native ✅ safe (audit suites are the un-bought competition). G2 (19/25): moats 5 (rigorous, explained, applicability-aware — the "when it doesn't apply" judgment) + 3 (continuous monitoring) + composes with Verification Bureau (doc 04). G3 (20/25): clear buyer (finance/audit/procurement), self-serve, high-value. G4 (13/20). **Total 75.**

## Audience & user profiles
Internal audit, finance/controllership, procurement (invoice fraud), forensic accountants, expense-management teams.
1. **Internal auditor** — agent: "the reimbursement dataset is nonconforming to Benford (MAD 0.021, digit 5 over-represented) — 40 claims flagged for review, here's the list."
2. **AP/procurement** — duplicate/split-invoice detection before payment.
3. **Controller** — continuous Benford monitoring of journal entries, alerted on drift.

## Where/how to get data & components
| Need | Source | Access |
|---|---|---|
| Transaction/financial data | ERP/accounting exports (Tally/QuickBooks), CSV | none for CSV |
| Math | `scipy`, `scikit-learn`, encoded Benford/Beneish | OSS; we build + validate |
| Watchers | TECH-00 §4 | internal |

## Monetization
- **Free**: single Benford analysis on ≤5K records
- **Pro ₹4,100/$49 mo**: full forensic suite, duplicate detection, medium datasets
- **Business ₹16,500/$199 mo**: continuous monitoring, M-Score, ERP pulls, audit-export reports
- Positioning: "the fraud-detection math forensic accountants use — flagging for review, on your books."

## India sole-prop notes
**Critical framing: flags anomalies for HUMAN review — never accuses or concludes fraud** (defamation/liability risk; a Benford flag is a signal, not proof — state this in every output). Financial data may include personal data → DPDP. Tally-export onboarding = Indian-SMB wedge. Export invoicing under LUT. Not audit/legal opinion — disclaim.

## Risks
False-positive harm (frame as "flagged for review," never "fraudulent"; explain statistical basis; human-in-loop mandatory); Benford misapplication (build in applicability checks — refuse on data types where Benford doesn't hold); liability (clear disclaimers: signal not proof, not an audit opinion).

---

---

## LISTING & DISCOVERABILITY
*Keep this section current. It drives marketplace search, AI tool selection and the buying decision.*

**Who discovers this MCP:** agent developers, developers and agent builders (Copilot Studio makers, Agentforce, Breeze and Gemini Enterprise builders, Claude/ChatGPT connector users). They find it, test it, and **champion the purchase** to the budget owner. Every field below is written for them first, with a business tagline the manager can understand.

### 1. Names
| Field | Use this | Rule |
|---|---|---|
| **Listing title** | **Financial Fraud & Anomaly Detection — Saifs AI** | Capability keyword first; ≤ 60 characters before "— Saifs AI" |
| **Tagline** (for the budget owner) | Spot unusual entries, possible fraud and earnings manipulation | Plain business words; ≤ 90 characters |
| **Server / connector ID** | `saifs-financial-anomaly` | Lowercase, hyphenated, no coined words |
| **Internal code name** | Financial Anomaly Detector (doc 31) | Docs and code only, **never** in a listing |

### 2. Description: the first 160 characters decide the click
1. **Developer contract:** Ledger / transactions (JSON) → flagged entries, Benford test, M-score, reasons.
2. **Business value (for the champion):** Investigation time cut 30–60%.
3. **Methods (specialist search and credibility):** uses Benford, Beneish M-Score, z-scores.
4. **Trust:** stateless; data is processed in the request and never stored; per-user OAuth sign-in. Flags for review; not an accusation.

### 3. Keywords
- **Developer search terms (title, description, tags):** fraud detection, anomaly detection, benford's law, journal entry testing, beneish m score, forensic accounting
- **Business phrases (tagline, FAQ, tool descriptions):** "find suspicious entries", "is this ledger fishy"
- **Method terms (description body):** Benford, Beneish M-Score, z-scores
- **Platform terms (only where actually listed):** MCP server, Copilot Studio connector, Agentforce action, Breeze agent tool, Gemini Enterprise, Amazon Bedrock AgentCore, Claude connector, ChatGPT app

### 4. Tools as shown in the listing (alphabetical)
`benford_analysis` · `detect_duplicate_payments` · `earnings_manipulation_score` · `explain_flag` · `flag_anomalous_transactions` · `watch_transactions`

**Tool-description rule:** the AI picks tools from these descriptions, so they count as discoverability too.
- Start with a verb and the business noun.
- Name the input fields.
- Say what comes back.
- Say when **not** to use the tool.

### 5. Example prompts (listing, demo video, tool descriptions)
- "Flag unusual journal entries this quarter"
- "Run Benford's test on expenses"
- "Check this company's M-score"

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
- Sample JSON payload shaped like a real export from: ERP ledger, Tally exports
- One copy-paste example call **and** its response
- A 60–90 second demo video: prompt → tool call → answer
- Link to the docs and the MCP/OpenAPI schema; privacy policy and terms

### 8. Champion kit (for the developer to forward upward)
- **One-line business case:** Investigation time cut 30–60%. Always as a range, never a bare number.
- **Security note:** stateless, nothing stored, per-user OAuth, DPDP/GDPR-friendly
- **Price:** the tier table from §Monetization, with the free tier stated first

### 9. Don'ts
- Coined names (e.g. "Financial Anomaly Detector") in titles or tags
- "Best", "#1" or "guaranteed" claims
- Bare money figures
- Promises beyond the guardrail: Flags for review; not an accusation
