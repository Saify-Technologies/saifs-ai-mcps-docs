# Financial Fraud & Anomaly Detection

[SAIFS AI](https://saifs.ai) (`saifs.ai`) publishes this as a remote MCP server under the registry name `ai.saifs/financial-anomaly`. It runs Benford’s Law, outlier checks, duplicate and split-invoice checks, and the Beneish M-Score on a ledger or a transaction export.

Every flag is a line for a person to review. A flag is a statistical signal. It is not a finding of fraud, not an accusation, and not an audit opinion. The investigator decides what the item is.

People search for this as fraud detection, anomaly detection, Benford’s Law, journal-entry testing, the Beneish M-Score, or forensic accounting. The questions are “find suspicious entries” and “is this ledger worth a closer look.” The short install page is the [server README](../README.md). This page is what gets flagged, why a digit test is not a verdict, and how to use each tool.

| | |
|---|---|
| **Registry name** | `ai.saifs/financial-anomaly` |
| **Endpoint** | `https://financial-anomaly-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## What it is

Finance, internal audit, and procurement teams review invoices, expense claims, journal entries, and vendor payments. Fabricated or mistyped numbers are hard to see one row at a time. This server applies the forensic tests those teams already know and returns the rows that failed, with the statistic that failed.

| Tool | The question it answers |
|---|---|
| `benford_analysis` | Do the leading digits look like naturally occurring amounts? |
| `flag_anomalous_transactions` | Which amounts are extreme, oddly round, or repeated? |
| `detect_duplicate_payments` | Which invoices look like the same bill twice, or like one bill split in two? |
| `earnings_manipulation_score` | Does a public company’s published ratios sit past the Beneish screen? |
| `explain_flag` | Why was this one item on the list? |
| `watch_transactions` | How does this extract differ from the summary of the last one you kept? |

A Benford screen inside a data clean is a different job. That screen lives on [Data Cleaning & Validation](../../data-cleaning/README.md), where a first-digit check is one quality flag among many. This server is the review of a ledger: the digit test, the conformity band, the rows to open, and the reason.

The call is stateless. The ledger is processed and discarded. You keep the prior summary if you want the next extract compared with it. Each user signs in with OAuth.

Teams that start from this list, instead of reading the book from the top, cut investigation time by about 30–60%. The list is the start of the investigation.

## Why a model should not declare the result

Benford’s Law and the Beneish M-Score have been standard forensic screens since the 1990s. They need a full column, a conformity test, and a check that the test even applies to that column. A language model can describe Benford’s Law. It does not reliably count digits across a large export, apply Nigrini’s conformity bands, refuse the test on numbers that were assigned rather than measured, and hand an investigator a stable list with the statistic attached.

The harm to avoid is a false positive treated as a conclusion. A reimbursement file can fail Benford because of a policy cap, a round per-diem, or a template, with no fabricated claim in it. The output names the test, the statistic, and the rows. It does not name a person as having committed fraud.

## How to use it

Connect the endpoint in the [install README](../README.md) and sign in when the client asks. Export the book from Tally, QuickBooks, or the ERP. Send amounts that arose in the course of business, not numbers someone assigned.

```json
{
  "transactions": [
    {"id": "JE-10442", "date": "2026-10-03", "account": "Travel", "amount": "18450.00", "party_ref": "V-018"},
    {"id": "JE-10443", "date": "2026-10-03", "account": "Travel", "amount": "5000.00", "party_ref": "V-018"}
  ]
}
```

Use your own references. A vendor name is personal data when it identifies a person. A party reference you can join back on your side is enough for the duplicate test.

### What a review sounds like

An internal auditor’s result reads like this: the reimbursement file is nonconforming on Benford (MAD 0.021, digit 5 over-represented), and 40 claims are listed for review. Accounts payable gets the near-duplicate and split-invoice groups before a payment run. A controller sends this quarter’s journals and the summary kept from last quarter, and reads the deviation between them.

The next sentence in each case is a review step. It is not “these are fraudulent.”

### When Benford’s Law is the wrong test

The expected first-digit share is

```text
P(d) = log10(1 + 1/d)
```

Digit 1 is about 30.1%. Digit 9 is about 4.6%. That shape appears in amounts that span several orders of magnitude and that were not chosen by a person to fit a range. The test is refused, with the reason, when the column is a poor fit for the law:

- Assigned identifiers: invoice numbers, cheque numbers, account codes, postal codes.
- A bounded range: a tariff, a percentage, an age band, a price list with a floor and a ceiling.
- Amounts that do not span orders of magnitude.
- A sample too small for a digit distribution to mean anything.

A refusal is the correct output. Running the test anyway would flag an ordinary price list.

### Prompts

- “Flag unusual journal entries this quarter.”
- “Run Benford’s test on expenses.”
- “Check this company’s M-score.”
- “Which vendor bills look like duplicates or splits before we pay them?”
- “Why was JE-10442 flagged? Show the statistical basis.”

## Tools

| Tool | What you send | What comes back | Do not use it when |
|---|---|---|---|
| `benford_analysis` | A numeric column from a ledger or a claim file | Observed first-digit, second-digit, and first-two-digit shares against the Benford shares, a chi-square, a mean absolute deviation, and the conformity band | The column is assigned, bounded, or too short. The tool refuses those. Do not use the result as proof of fraud |
| `detect_duplicate_payments` | Invoices: reference, party, amount, date | Groups that look like the same bill twice, or like one bill split across invoices | You want the groups merged or paid. This only lists them for review before payment |
| `earnings_manipulation_score` | The eight Beneish ratios from a public company’s financial statements | The M-Score and whether it sits past −1.78 | You have a private ledger of invoices and no financial statements, or you want an audit opinion. Past the threshold means “screen for review” |
| `explain_flag` | One flagged item and the test that flagged it | The statistical basis, in language an investigator can put in the file | You have not run a test yet. There is no flag to explain |
| `flag_anomalous_transactions` | Transactions with amounts | Extreme amounts (z-score and IQR), an excess of round numbers, and repeated patterns | You want those rows deleted or a person labelled. Rows are listed for review |
| `watch_transactions` | This extract and the summary you kept from the last one | A deviation in the Benford profile or in the outlier rate | You expect the server to remember last quarter. Send the summary again. Nothing is stored |

## The methods

### Benford’s Law

Newcomb (1881) and Benford (1938). Mark Nigrini’s forensic application is the one audit teams use. Observed digit shares are compared with `P(d) = log10(1 + 1/d)` by a chi-square test and by the mean absolute deviation of the nine (or ninety) proportions.

Nigrini’s first-digit MAD bands, which classify the column, are:

| MAD | Band |
|---|---|
| Up to 0.006 | Close conformity |
| 0.006 to 0.012 | Acceptable conformity |
| 0.012 to 0.015 | Marginally acceptable |
| Above 0.015 | Nonconforming |

Nonconforming means the digits are worth a review. MAD 0.021 with one digit over-represented is nonconforming. It is still not a finding that any row was fabricated. Second-digit and first-two-digit tests are the deeper pass when the first digit is already off. `explain_flag` names which digit, which test, and which rows pulled the share up.

### Outliers and round numbers

A z-score and the interquartile range mark amounts that sit far from the rest of the column. Round-number frequency is a separate screen: a column of claims that lands on 5,000 or 10,000 far more often than the rest of the book is listed, because rounded amounts are a known place investigators look. Fuzzy matching marks repeated and near-repeated references. Each of these is a list. None of them is a conclusion.

### Duplicate and split invoices

`detect_duplicate_payments` groups invoices that share a party, an amount, and a close date, and groups that look like one underlying bill entered more than once or broken apart. The groups go to accounts payable before payment. The tool does not block a payment and does not declare a duplicate to be an error. A progress invoice and a genuine second delivery can look the same to a matcher.

### Beneish M-Score

Beneish (1999). Eight ratios from the financial statements — days’ sales in receivables, gross margin, asset quality, sales growth, depreciation, sales and general expense, leverage, and accruals — combine into one score. A score above −1.78 is the published screen for a higher chance of earnings manipulation. The screen was built for public-company statements. It does not read a stack of invoices. A score past the line is a reason for a person to open the statements. It is not a restatement and not a legal finding.

## How to read a result

- **“Flagged for review” is the whole status.** The file an investigator keeps should say that. It should not say fraudulent, manipulated, or guilty.
- **Read the test name and the statistic** before the row list. A nonconforming MAD, a z-score, a duplicate group, and an M-Score are different kinds of signal.
- **A refusal on Benford** means the column should not have been tested. Do not force the test in the conversation.
- **A duplicate group can be legitimate.** Two deliveries, a deposit and a final invoice, or a reused reference all need a person.
- **The M-Score is a screen of statements,** not of one vendor. Below −1.78 the screen does not light up. Above it, someone qualified to read the accounts takes the next step.
- **The prior summary is yours.** `watch_transactions` compares what you send. A drift alert is a change in the statistics, not a stored case file.

## Data and what this will not do

Ledger extracts can contain personal data. Send references you control. The server processes the request and does not store it. It does not enrich a party against an outside register. Each user signs in with OAuth. Your own duties under India’s Digital Personal Data Protection Act, the GDPR, or any other law that applies to the export stay with you.

This is not an audit, not a legal opinion, and not a fraud finding. It will not accuse a person, close a case, or replace the human review. It will not run Benford’s Law on identifiers, bounded price lists, or a sample too small to support a digit distribution.

Privacy policy: https://saifs.ai/privacy_policy

## Plans

| Tier | What it covers |
|---|---|
| **Free** | One Benford analysis on up to 5,000 records |
| **Pro** | ₹4,100 / $49 per month — the forensic set, including duplicate detection, on medium extracts |
| **Business** | ₹16,500 / $199 per month — monitoring across extracts you send, the M-Score, ERP export pulls, and an audit-export report |

The free tier is enough to run Benford on one extract and read the conformity band, with no sales call. Current terms are on [saifs.ai](https://saifs.ai).

## Support

- Install and client setup: [server README](../README.md)
- A first-digit check while cleaning a file: [Data Cleaning & Validation](../../data-cleaning/README.md)
- Website: https://saifs.ai
- Registry: https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
