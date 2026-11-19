# 67 · Data Cleaning & Validation for AI Agents: Deterministic Clean-Up, Rule Repair & Agent-Ready Scoring MCP
**Initial moat: 81/100 (potential 84) · IW-Breadth: 85 · Phase: Wave 8 · Priority P2 · Shared front end for every Saifs MCP** *(upgraded 29 Oct 2026 from 75 with seven added classical methods; see §3B)*
**Classical models: Fellegi-Holt automatic edit & minimum-change error localisation (1976) · Hot-deck imputation (US Census 1950s; Ford 1983) · EM algorithm (Dempster-Laird-Rubin 1977) · Multiple imputation (Rubin 1987) · Robust outliers: MAD (Hampel 1974), Tukey fences (1977), Benford (1938) · Fellegi-Sunter record linkage (1969) · Levenshtein (1966) / Jaro-Winkler (1989–90) · Soundex (1918) · Relational normalisation (Codd 1970–72) · **Added:** functional dependencies (Codd 1971; Armstrong 1974) · check digits (Luhn 1954; mod-11) · Shannon entropy (1948) & minimum description length (Rissanen 1978) · Mahalanobis distance (1936) & robust MCD / least-median regression (Rousseeuw 1984) · deductive correction of typos and unit slips · Gower distance (1971) · PSI and Kolmogorov-Smirnov drift tests with control charts (Shewhart 1924)**

## 1. Problem statement
Agents act on whatever data they're given. Business data is messy:
- headers like "Party Name" and "Inv Amt"
- dates in five formats
- "₹1,250.50", "(80)" and "n/a" in a number column
- qty × price ≠ amount
- duplicate customers, blanks and typos

Garbage in means garbage decisions out. Asking the LLM to clean the data is unreliable:
- **Repeatability:** "deterministic" LLM settings vary up to 15% across identical runs (Atil et al., 2024).
- **Accuracy:** LLM cleaning agents' decisions matched across runs only 44–50% of the time, and their repairs were correct about 3% of the time. A rule-based baseline found errors better (Fadlallah, 2026).

Existing MCPs mostly **monitor** or **validate** (Anomalo, SYNQ, Great Expectations) or are hobby-scope cleaners. No hosted, deterministic cleaner with a change log was found on the major marketplaces.

## 2. Solution
Messy rows plus optional rules in → clean standardised rows, a change log (every edit with its reason and rule), a quarantine list, a missing-value report with ranges, and an **agent-ready score** out. **Same input + same profile → same output, byte for byte.**

**Tool set (v1, alphabetical):**
- `change_log(run_id)`: every edit (row, field, old → new, rule, method) and a reproducible run hash
- `clean_dataset(rows, profile, rules)`: one-call pipeline: map → standardise → validate → repair → impute → flag → dedupe
- `fill_missing(rows, columns, method)`: hot-deck, EM or multiple imputation; values flagged "imputed" with a range
- `find_duplicates(rows, keys)`: duplicate groups (shares the engine with 66); never merges by itself
- `fix_errors(rows, rules, field_weights)`: Fellegi-Holt minimum-change repair; changes the fewest, least-reliable fields so every rule passes
- `flag_outliers(rows, columns)`: robust z (MAD), Tukey fences, Benford screening; flags, never deletes
- `map_columns(headers, target_schema)`: maps messy headers to standard fields with confidence
- `profile_data(rows)`: types, formats, missing %, distinct counts, suspected keys, rule suggestions
- `quality_score(rows, profile)`: 0–100 agent-ready score by dimension (completeness, validity, consistency, uniqueness)
- `standardise_values(rows, profile)`: dates, numbers, currency, units, phones (E.164), GSTIN/PAN formats, company suffixes, casing
- `validate_rules(rows, rules)`: pass/fail per rule with failing rows

**Added tools (moat upgrade):**
- `check_digits(rows, fields)`: validates GSTIN, GTIN/EAN barcodes and other check-digit identifiers (Luhn, mod-11, mod-36); catches typos with no lookup  *(v1)*
- `compare_loads(profile_prev, rows)`: drift between this load and the last: distribution shift (PSI, KS), new or missing categories, volume drop, format change  *(v2)*
- `discover_rules(rows)`: finds functional dependencies (e.g. pincode → city, GSTIN prefix → state, SKU → price band) with confidence, and lists violating rows  *(v2)*
- `fix_typos_units(rows, history)`: detects digit swaps, sign flips and ×10/×100/×1000 or lakh/crore slips against history and rules, and proposes the correction  *(v2)*
- `infer_patterns(rows, columns)`: learns each column's dominant format (e.g. `AA-9999`, `DD/MM/YYYY`) and flags values that don't fit  *(v1)*
- `relationship_outliers(rows, column_groups)`: rows whose **combination** of values is abnormal (e.g. amount far off for that qty and SKU), even if each value looks normal  *(v2)*

## 3. THE ALGORITHMS
### Column mapping
Score each header against a target-schema dictionary (synonyms + Jaro-Winkler + value-pattern checks, e.g. a column whose values match a GSTIN pattern). Accept above a threshold; otherwise return candidates for confirmation. Confirmed mappings are saved in the caller's `cleaning_profile`.
### Standardisation (deterministic parsers)
- Dates: try a fixed ordered list of formats with the locale (India: day-first)
- Numbers: strip currency and thousands separators; "(80)" → −80; "n/a"/"-" → missing
- Phones → E.164
- Names: Unicode normalise, trim, case rules, strip legal suffixes into a separate field

### Rule validation and Fellegi-Holt repair
Rules are linear or logical edits (e.g. `amount = qty × price`; `invoice_date ≤ payment_date`; `age < 15 ⇒ marital = never_married`). For a failing record, find the **smallest weighted set of fields** whose change can satisfy all rules (set-cover over implied edits, solved as a small MIP via the 29 solver). Then fill those fields with values consistent with the rules: derived where a rule determines the value, otherwise imputed. Field weights encode reliability (e.g. typed amounts less reliable than system quantities).
### Missing values
- **Mechanism check:** missingness vs other columns, to indicate MCAR/MAR
- **Methods:** hot-deck (donor from the most similar complete record), EM for numeric blocks, multiple imputation (m = 5) to give a range
- **Never imputed:** identifiers (keys)

### Outliers
Robust z = 0.6745·(x − median)/MAD > 3.5 → flag. Tukey fences (Q1 − 1.5·IQR, Q3 + 1.5·IQR) as a second view. Benford first-digit test per amount column. Outliers are flagged and quarantined only if they also break a rule.
### Duplicates
Fellegi-Sunter scoring with blocking (shared with 66). Output groups only.
### Determinism
- Ordering: stable row ordering; fixed tie-break rules
- Randomness: seeded randomness for imputation (seed in the profile)
- Run hash: SHA-256 over input + profile + engine version, so results are reproducible

## 3B. ADDED MATHS: moat upgrade (75 → 81)
### Functional-dependency discovery (Codd 1971; Armstrong's axioms 1974)
For column sets X → Y, measure how often X determines a single Y: confidence = 1 − violating rows / rows. Search levelwise over small left-hand sides (1–2 columns), pruning with Armstrong's axioms. Keep rules with confidence ≥ 0.95 and support ≥ 20. Violating rows become candidate errors, and confirmed rules are saved in the profile.
### Check digits
- Luhn (1954) mod-10 for account-style and card-style numbers (validation only; card numbers are never stored or echoed)
- GTIN/EAN mod-10 weighting for barcodes
- GSTIN mod-36 check character
- A wrong check digit means a typo, flagged with zero false positives

### Dominant-pattern detection (Shannon 1948; Rissanen 1978)
Map each value to a shape (A = letter, 9 = digit, punctuation kept). Choose the pattern set that minimises description length: the bits to describe the patterns plus the exceptions. Values outside the chosen patterns are flagged. Column entropy gives a type-confidence score.
### Relationship outliers (Mahalanobis 1936; Rousseeuw 1984)
For numeric groups (e.g. qty, amount per SKU): robust centre and covariance via minimum covariance determinant. Distance d² = (x − μ)ᵀ Σ⁻¹ (x − μ). Flag when d² > χ²₀.₉₉₉(p). For ratio relations (amount ≈ price × qty), least-median-of-squares regression residuals.
### Deductive correction of typos and unit slips
For a value failing a rule or far from its history, test candidates in fixed order:
1. adjacent-digit swap
2. sign flip
3. ×/÷ 10, 100, 1000
4. lakh ↔ crore

Accept the **only** candidate that passes all rules and falls inside the robust history range. If several pass, don't fix: quarantine.
### Gower-distance hot-deck (1971)
Donor = nearest complete record by Gower distance (range-scaled numeric plus match/mismatch categorical). Ties broken deterministically by record order and seed.
### Drift between loads (reuses 20 and 33)
Per column:
- **PSI:** `Σ (a − e)·ln(a/e)` on fixed bins. Below 0.1 is stable; 0.1–0.25 watch; above 0.25 shift.
- **KS test** for numeric columns.
- **New or missing categories.**
- **Row-count control chart** across loads (Shewhart limits).

The previous load's profile comes from the caller's `cleaning_profile`, so no storage is needed on our side.

### Version plan
| Version | Contents | Moat |
|---|---|---|
| v1 (launch) | Core doc-67 tools + `check_digits` + `infer_patterns` | ~77 |
| v2 | + `discover_rules`, `relationship_outliers`, `fix_typos_units`, `compare_loads`, Gower hot-deck | **~81** (potential 84) |

## 4. Pressure test
- **G1 Substitution (25/30, was 22):** the LLM alone is unreliable (evidence). Rule discovery, multi-column outliers, check digits and deductive unit correction go well beyond a quick sandbox pandas script. Power Query and OpenRefine still need a human to design steps.
- **G2 Moats (21/25, was 19):** continuity; accumulated per-source profile now carries discovered rules, learned correction weights and **load-to-load drift baselines**; strong witness (change log, run hash); operations.
- **G3 Business (20/25):** breadth ~85; self-serve; per-row billing; all marketplaces; internal leverage across all Saifs MCPs.
- **G4 Time (15/20, was 14):** "AI-ready data" priority; EU AI Act Art. 10; each load makes the profile more valuable. **Total 81; potential 84.**
- **Recurrence:** every load, sync or agent run ✅

## 5. Audience & user profiles
Agent developers and builders (the main champions), data and analytics engineers, RevOps/CRM admins, finance-ops teams loading Tally/ERP exports, consultants onboarding client data.
1. **Agent builder:** the agent calls `clean_dataset` on a Tally export before Customer Segmentation (14). 1,240 rows: 1,198 clean, 31 repaired (each logged), 11 quarantined; agent-ready score 91.
2. **Finance-ops lead:** "amount ≠ qty × price" on 46 invoices, fixed by changing the typed amount (lowest reliability), with the reason shown per row.
3. **Consultant:** the same `cleaning_profile` reused every month for a client gives identical cleaning each time.

## 6. Data & components (stateless contract)
**Input JSON:** `rows[{...}]` (any columns), optional `target_schema`, `rules[]`, `field_weights{}`, `cleaning_profile` (from the last run), `locale`, `seed`.
**Output JSON:** `clean_rows`, `change_log[]`, `quarantine[]`, `missing_report`, `outliers[]`, `relationship_outliers[]`, `discovered_rules[]`, `drift_report`, `duplicate_groups[]`, `quality_score`, `run_hash`, updated `cleaning_profile` (now holding rules, patterns, correction weights and drift baselines).
Nothing stored. No outside lookups or enrichment. Math: NumPy, SciPy, OR-Tools (29) for Fellegi-Holt, shared matching engine (58/66). Large files: a `get_engine` script runs locally (same pattern as built MCPs).
**Refusal thresholds:** imputation refused for columns with > 40% missing (flagged); < 30 rows → profiling and rules only.

## 7. Monetization
- Free: 5,000 rows/month
- Pro ₹2,500/$29 mo: 200,000 rows, profiles, rules, change log
- Business ₹8,300/$99 mo: 5M rows, Fellegi-Holt repair, multiple imputation, duplicate groups, multi-source profiles
- **Included free (capped) with every Saifs maths MCP** as the input stage; upsell for standalone volume

## 8. India notes & risks
Handles Indian formats: day-first dates, ₹ and lakh/crore separators, +91 phones, GSTIN/PAN patterns, Tally export quirks, transliteration variants in names. **DPDP:** personal fields processed in-request only, optional hashing, nothing stored, no enrichment. Risks:
- **Wrong repair:** minimum-change plus weights, every edit logged, uncertain rows quarantined.
- **Over-imputation:** ranges, flags, refusal threshold.
- **"Just use pandas" objection:** sell on determinism, change log, agent-ready score and token cost.

Disclaimer: cleaning suggestions with a full audit trail; the data owner approves use.

---

## LISTING & DISCOVERABILITY
*Keep this section current. It drives marketplace search, AI tool selection and the buying decision.*

**Who discovers this MCP:** agent developers, developers and agent builders. They hit the garbage-in problem first and **champion the purchase**.

### 1. Names
| Field | Use this | Rule |
|---|---|---|
| **Listing title** | **Data Cleaning & Validation for AI Agents — Saifs AI** | Capability keyword first |
| **Tagline** (for the budget owner) | Turn messy exports into clean, consistent data your agents can trust, with every change logged | Plain business words |
| **Server / connector ID** | `saifs-data-cleaning` | Lowercase, hyphenated |
| **Internal code name** | Data Cleaning & Validation (doc 67) | Docs and code only |

### 2. Description: the first 160 characters decide the click
1. **Developer contract:** messy rows (JSON) → clean rows, change log, quarantine list and an agent-ready quality score; same input gives the same output.
2. **Business value:** stops garbage-in, garbage-out before agents act; cleaning runs in milliseconds with no LLM tokens.
3. **Methods:** Fellegi-Holt minimum-change repair, automatic rule discovery (functional dependencies), check digits, multi-column outliers (Mahalanobis), multiple imputation, load-to-load drift detection.
4. **Trust:** stateless; nothing stored; never silently changes or deletes data.

### 3. Keywords
- **Developer search terms:** data cleaning, data validation, data quality, deduplication, missing value imputation, outlier detection, schema mapping, column mapping, ETL cleaning, AI-ready data, data preprocessing, data drift detection, rule discovery, gstin validation, data observability
- **Business phrases:** "clean up this Excel", "fix messy Tally export", "standardise dates and amounts", "is this data ready for the agent"
- **Method terms:** Fellegi-Holt, multiple imputation, EM, MAD, Tukey, Benford, Fellegi-Sunter, Jaro-Winkler
- **Platform terms:** MCP server, Copilot Studio connector, Agentforce action, Breeze agent tool, Gemini Enterprise, Amazon Bedrock AgentCore, Claude connector

### 4. Tools as shown in the listing (alphabetical)
`change_log` · `check_digits` · `clean_dataset` · `compare_loads` · `discover_rules` · `fill_missing` · `find_duplicates` · `fix_errors` · `fix_typos_units` · `flag_outliers` · `infer_patterns` · `map_columns` · `profile_data` · `quality_score` · `relationship_outliers` · `standardise_values` · `validate_rules`

**Tool-description rule:**
- Start with a verb and the business noun.
- Name the input fields.
- Say what comes back.
- Say when **not** to use the tool, e.g. "don't use for free-text summarisation."

### 5. Example prompts
- "Clean this Tally export before you rank my customers"
- "Check this CRM file: is it ready for the agent?"
- "Fix rows where amount doesn't equal quantity × price and show what you changed"

### 6. Categories per marketplace
| Marketplace | Category / tags |
|---|---|
| Microsoft (Copilot Studio certified connector / AppSource) | Data & Analytics; IT & Management; AI + Machine Learning |
| Salesforce AgentExchange | Data / Platform |
| AWS Marketplace: AI Agents & Tools | Data → Data quality & preparation |
| Google Cloud Marketplace / Gemini Agent Gallery | Data analytics / Data quality |
| HubSpot Marketplace (Breeze) | Operations / Data sync & quality |

### 7. Developer try-it kit
- Free tier, under 10 minutes to first result
- Sample messy Tally, Excel and HubSpot files plus the cleaned output and change log
- Copy-paste example call and response
- 60–90 s demo: messy file → agent calls `clean_dataset` → clean data plus log → Customer Segmentation runs on it
- Docs, MCP/OpenAPI schema, privacy policy, terms

### 8. Champion kit
- **Business case:** cleaning before agents act prevents wrong decisions; it is deterministic and auditable, and it costs milliseconds instead of thousands of LLM tokens per run. Cite the research (Atil 2024; Fadlallah 2026) and Gartner's AI-ready-data point.
- **Security:** stateless, nothing stored, per-user OAuth, DPDP/GDPR-friendly
- **Price:** free tier first, then Pro and Business

### 9. Don'ts
- Coined names in titles
- "100% accurate" or "fixes everything" claims
- Implying it silently corrects data (it logs and quarantines)
- Enrichment from outside sources
