# Data Cleaning & Validation for AI Agents

[SAIFS AI](https://saifs.ai) (`saifs.ai`) publishes this as a remote MCP server under the registry name `ai.saifs/data-cleaning`. It turns a messy export into standardised rows, a change log, a quarantine list, and an agent-ready score. The same rows and the same profile produce the same output, byte for byte.

People search for this as data cleaning, data validation, data quality, deduplication, missing-value imputation, outlier detection, schema mapping, ETL cleaning, or data preprocessing. The questions are “clean up this Excel,” “fix this Tally export,” and “is this file ready for the agent.” The short install page is the [server README](../README.md). This page is what gets cleaned, why a model should not clean it, and how to use each tool.

| | |
|---|---|
| **Registry name** | `ai.saifs/data-cleaning` |
| **Endpoint** | `https://data-cleaning-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## What it is

Agents act on the file they are given. This server is the step before that. You send rows, an optional target schema, optional rules, and an optional `cleaning_profile` from the last run. You get back:

| Piece | What it is |
|---|---|
| `clean_rows` | Standardised rows. Imputed cells are marked imputed. |
| `change_log` | Every edit: row, field, old value, new value, rule, and method. |
| `quarantine` | Rows the cleaner would not guess. They are not deleted and they are not silently “fixed.” |
| `missing_report` | What was missing, and a range where a value was imputed. |
| `outliers` and `relationship_outliers` | Values, and combinations of values, that sit far from the rest. Flagged. |
| `duplicate_groups` | Groups that look like the same entity. Nothing is merged unless you decide to merge. |
| `discovered_rules` | Regularities in the file, with confidence, and the rows that break them. |
| `drift_report` | How this load differs from the profile of the previous load. |
| `quality_score` | An agent-ready score from 0 to 100, split by completeness, validity, consistency, and uniqueness. |
| `run_hash` | A SHA-256 over the input, the profile, and the engine version. |
| `cleaning_profile` | Mappings, patterns, rules, correction weights, and drift baselines. You store it. The server does not. |

`clean_dataset` runs the whole pipeline in one call: map headers, standardise values, validate rules, repair, impute, flag, then group duplicates. The other tools are the same steps on their own, when you want one of them and not the rest.

A Tally export of 1,240 rows, in the shape this pipeline returns, can come back as 1,198 clean rows, 31 repairs (each in the log), 11 quarantined rows, and an agent-ready score of 91. Those counts are an example of the summary. Your file produces its own.

The call is stateless. Rows are processed and discarded. There is no lookup against an outside directory and no enrichment. Confirmed mappings and learned rules live in the profile you send back next month, which is why the same client file cleans the same way every time.

Cleaning is a suggestion with an audit trail. The data owner approves what an agent is allowed to use. A score of 100 means the checked dimensions passed. It does not mean every business fact in the file is true.

Once the rows are clean, a customer book can be ranked with [Customer Intelligence & Segmentation](../../customer-intelligence/README.md). Rank the cleaned file, not the raw export.

## Why a model should not clean the file

Headers arrive as “Party Name” and “Inv Amt.” Dates arrive in five formats. A number column contains `₹1,250.50`, `(80)`, and `n/a`. Quantity times price does not equal amount. The same customer appears twice, and a GSTIN is one character off.

Asking a language model to repair that is not repeatable. Identical runs of “deterministic” model settings have varied by as much as 15% (Atil et al., 2024). In a study of cleaning agents, decisions matched across runs only 44–50% of the time, and the repairs were correct about 3% of the time. A rule-based pass found the errors more reliably (Fadlallah, 2026).

This server is that rule-based pass, with the log attached. It does not spend a model call on each cell. The next agent then sees the clean rows and the list of what changed.

## How to use it

Connect the endpoint in the [install README](../README.md) and sign in when the client asks.

Send the rows as they are. Add rules when you know them (`amount = qty × price`, `invoice_date ≤ payment_date`). Add `field_weights` when one column is typed by hand and another comes from a system: the repair changes the less reliable field first. Set `locale` so dates are read day-first for an Indian file. Pass `seed` inside the profile when you want imputation draws fixed. Pass last month’s `cleaning_profile` when you have one.

```json
{
  "rows": [
    {"Party Name": "Acme Pvt Ltd", "Inv Amt": "₹1,250.50", "Qty": "10", "Rate": "100", "Inv Date": "07/11/2026"},
    {"Party Name": "ACME PRIVATE LIMITED", "Inv Amt": "(80)", "Qty": "2", "Rate": "40", "Inv Date": "n/a"}
  ],
  "target_schema": ["customer_name", "amount", "qty", "price", "invoice_date"],
  "rules": ["amount = qty * price"],
  "locale": "en-IN",
  "cleaning_profile": null
}
```

Under 30 rows, the run profiles the file and checks rules. It does not pretend a repair or an imputation is identified. Imputation is refused on a column that is more than 40% missing. That column is flagged instead.

### What standardisation does

Indian exports are parsed on purpose.

| Input | Result |
|---|---|
| Dates | A fixed order of formats, day-first for an Indian locale. |
| `₹1,250.50`, `1,23,456` | A decimal number. Lakh and crore grouping is understood. |
| `(80)` | −80. |
| `n/a`, `-` | Missing, not zero. |
| Phone | E.164, so a local mobile becomes `+91…`. |
| GSTIN, PAN | Format and, for GSTIN, the mod-36 check character. |
| Company name | Unicode-normalised, trimmed, cased, with a legal suffix lifted into its own field. |

### A repair, and when it stops

`fix_errors` uses Fellegi-Holt (1976). On a row that breaks a rule it changes the smallest weighted set of fields that can make every rule pass. A typed amount is changed before a system quantity when the weights say the amount is less reliable. Forty-six invoices where amount does not equal quantity times price come back with the amount changed and the reason on each row, when the amount is the field the weights mark as least reliable.

`fix_typos_units` tries candidates in a fixed order: an adjacent-digit swap, a sign flip, a factor of 10, 100, or 1,000, then a lakh/crore slip. It accepts a candidate only when that candidate is the only one that passes every rule and sits inside the robust history range. If two candidates pass, the row is quarantined.

Outliers are flagged and left in place. A row is quarantined when the outlier also breaks a rule. `find_duplicates` returns groups. It does not merge them.

### Prompts

- “Clean this Tally export before you rank my customers.”
- “Check this CRM file: is it ready for the agent?”
- “Fix rows where amount doesn't equal quantity × price and show what you changed.”
- “Which GSTINs fail the check character?”
- “How does this month’s export differ from the profile I saved last month?”

## Tools

| Tool | What you send | What comes back | Do not use it when |
|---|---|---|---|
| `change_log` | A `run_id` from a clean you just ran | Every edit and the run hash | You want the rows cleaned. Call `clean_dataset` first |
| `check_digits` | Rows and the identifier fields | Pass or fail for GSTIN, GTIN/EAN, Luhn, mod-11, and mod-36. A bad check character is a typo | You want the number looked up in a registry. Nothing is enriched. Card numbers are not stored and not echoed |
| `clean_dataset` | `rows`, optional `profile`, `rules`, `target_schema`, `locale` | Clean rows, the log, quarantine, missingness, outliers, duplicate groups, the score, the hash, and an updated profile | The file has fewer than 30 rows and you expected repairs, or you wanted a prose summary of the sheet |
| `compare_loads` | This load’s rows and the previous `cleaning_profile` | PSI, a Kolmogorov-Smirnov result on numeric columns, new or missing categories, a volume change, a format change | You have no saved profile. The first load has nothing to drift from |
| `discover_rules` | Rows | Functional dependencies with confidence, and the rows that violate them. A kept rule has confidence at least 0.95 and support at least 20 | You want those rules applied as repairs immediately. Confirm them into the profile, then repair |
| `fill_missing` | Rows, columns, and a method | Hot-deck, EM, or multiple imputation (five draws) with each filled cell flagged and a range attached | The column is an identifier, or more than 40% of it is missing. Those are refused |
| `find_duplicates` | Rows and the keys or fields to compare | Groups that match. Fellegi-Sunter scores with blocking | You want the groups merged automatically |
| `fix_errors` | Rows, rules, and field weights | The minimum-change repair that makes the rules pass, with each edit explained | You have no rules. Profile and discover first. Uncertain rows are quarantined |
| `fix_typos_units` | Rows and history or rules | A single accepted correction for a digit swap, a sign, a factor of ten, or a lakh/crore slip | More than one candidate passes. The row is quarantined |
| `flag_outliers` | Rows and the numeric columns | Robust-z, Tukey, and Benford flags | You want those rows deleted. Flagged rows stay |
| `infer_patterns` | Rows and columns | The dominant shape of each column (`AA-9999`, `DD/MM/YYYY`) and the values that do not fit | You want the values rewritten. This tool flags them |
| `map_columns` | Headers and a target schema | A mapping with confidence, or candidates for you to confirm | Confidence is low and you have not confirmed the candidates. Unconfirmed maps are not applied as fact |
| `profile_data` | Rows | Types, formats, missingness, distinct counts, suspected keys, and rule suggestions | You want the file changed. Profiling only reads |
| `quality_score` | Rows and a profile | 0–100 by completeness, validity, consistency, and uniqueness | You want a guarantee that the business facts are correct. The score is the checked dimensions |
| `relationship_outliers` | Rows and column groups | Rows whose combination is abnormal, even when each column looks normal | You want a single-column outlier. That is `flag_outliers` |
| `standardise_values` | Rows and a profile | Dates, numbers, currency, units, phones, GSTIN/PAN, suffixes, and casing, in canonical form | You want rule repairs. Standardising does not make quantity times price true |
| `validate_rules` | Rows and rules | Pass or fail per rule, with the failing rows | You want the failures rewritten. That is `fix_errors` |

## The methods

### Headers

Each header is scored against the target schema with synonyms, Jaro-Winkler similarity, and a check of the values (a column of GSTIN-shaped strings is not a phone). A score above the threshold is accepted. Anything else comes back as candidates. A confirmed map is written into the profile you keep.

### Rules and minimum change

Rules are edits such as `amount = qty × price`, `invoice_date ≤ payment_date`, or `age < 15 ⇒ marital_status = never_married`. For a failing row, Fellegi-Holt finds the smallest weighted set of fields that can satisfy all of the rules, then fills those fields from the rule when the rule determines the value, and from imputation when it does not.

### Missing values

Missingness is checked against the other columns so the report can indicate whether it looks missing completely at random or missing at random. Hot-deck copies from the nearest complete donor. Donors are nearest by Gower distance (1971): numeric fields scaled to their range, categorical fields as match or mismatch, ties broken by row order and the seed. EM (Dempster, Laird, and Rubin, 1977) is used for numeric blocks. Multiple imputation (Rubin, 1987), five draws, is what produces a range rather than one filled-in number. Identifier columns are never imputed.

### Outliers

A robust z uses the median and the median absolute deviation (Hampel, 1974):

```text
robust z = 0.6745 × (x − median) / MAD
```

A point with robust z above 3.5 is flagged. Tukey fences (1977) are the second view: below Q1 − 1.5 × IQR, or above Q3 + 1.5 × IQR. Benford’s first-digit test (1938) is a screen on amount columns. None of these delete the row.

Combinations use Mahalanobis distance (1936) on a robust centre and covariance (minimum covariance determinant, Rousseeuw, 1984). A row is flagged when that distance passes the 0.999 chi-square cutoff for its dimension. For a ratio such as amount against price times quantity, the residual comes from least-median-of-squares regression. A quantity and an amount can each look ordinary and still be flagged together.

### Patterns, check digits, and discovered rules

Each value is reduced to a shape: letters, digits, and the punctuation kept. The pattern set that minimises description length (Shannon, 1948; Rissanen, 1978) is the column’s dominant format. Values outside it are flagged. Column entropy is the type-confidence score.

Check digits catch a typo with no external lookup. Luhn (1954) is mod-10 for account-style and card-style numbers, and it is validation only. GTIN/EAN uses the barcode mod-10 weights. GSTIN uses the mod-36 check character. A wrong check character is a typing error. The identifier is not completed from a government database.

Functional dependencies (Codd, 1971; Armstrong, 1974) are regularities such as pincode determining city, or a GSTIN prefix determining state. Confidence is one minus the share of violating rows. The search keeps rules at confidence of at least 0.95 with support of at least 20. Violating rows are candidate errors. A rule enters the profile when you confirm it.

### Duplicates

Fellegi-Sunter (1969) scores pairs inside blocks, using string similarity that includes Levenshtein, Jaro-Winkler, and Soundex where the field is a name. The output is groups. The merge is yours.

### Drift

Against the previous profile, each column reports the population stability index:

```text
PSI = Σ (actual − expected) × ln(actual / expected)
```

Below 0.10 is stable. From 0.10 to 0.25 is a watch. Above 0.25 is a shift. Numeric columns also get a Kolmogorov-Smirnov test. New and missing categories are listed. Row count is compared with Shewhart limits across the loads whose profiles you have kept.

### Determinism

Row order is stable. Ties break the same way every time. Imputation uses the seed in the profile. The run hash is SHA-256 of the input, the profile, and the engine version. Same input and same profile give the same bytes back.

## How to read a result

- **The log is the product.** An edit without a row, a field, an old value, a new value, and a rule is not a result this server returns.
- **Quarantine means “do not guess.”** Use the original row, or fix it at the source. Do not tell an agent the quarantined value was repaired.
- **Imputed means estimated.** Read the range. A column that was refused is still missing on purpose.
- **The score is readiness, not truth.** Completeness, validity, consistency, and uniqueness can all be high while a price in the source system is still the wrong price.
- **Duplicate groups are candidates.** Merging two customers is a decision. The tool stops at the group.
- **Drift is against your last profile.** A shift above 0.25 PSI, a new category, or a volume break is a reason to look before the next agent runs.
- **Under 30 rows,** trust the profile and the rule failures. Do not trust a repaired distribution.

## Data and what this will not do

Personal fields are processed in the request and discarded. Send hashes in place of raw identifiers when the next step does not need the raw value. Card numbers are checked and not written back out. Nothing is enriched from outside. Each user signs in with OAuth. Your own duties under India’s Digital Personal Data Protection Act, the GDPR, or any other law that applies to the export stay with you.

The server will not silently delete rows, silently merge duplicates, impute a key, or repair a row when more than one correction fits. It will not summarise a document. It will not certify that the cleaned file is true in the world, only that the rules, patterns, and checks you ran are recorded.

Privacy policy: https://saifs.ai/privacy_policy

## Plans

| Tier | What it covers |
|---|---|
| **Free** | 5,000 rows a month |
| **Pro** | ₹2,500 / $29 per month — 200,000 rows, profiles, rules, and the change log |
| **Business** | ₹8,300 / $99 per month — 5 million rows, Fellegi-Holt repair, multiple imputation, duplicate groups, and multi-source profiles |
| **With a SAIFS AI maths server** | A capped volume of cleaning is included as the input stage |

The free tier is enough to clean one export and read the log with no sales call. Current terms are on [saifs.ai](https://saifs.ai).

## Support

- Install and client setup: [server README](../README.md)
- After the export is clean, rank the customers: [Customer Intelligence & Segmentation](../../customer-intelligence/README.md)
- Website: https://saifs.ai
- Registry: https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
