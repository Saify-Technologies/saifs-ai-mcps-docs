# Data Cleaning & Validation for AI Agents

Deterministic clean-up for messy business exports before agents act. Maps headers, standardises dates/numbers/phones, validates and repairs with Fellegi-Holt, imputes missing values, flags outliers, finds duplicates and returns an agent-ready quality score — with a full change log and quarantine list. Same input + same profile → same output, byte for byte. Nothing is stored; nothing is silently deleted.

| | |
|---|---|
| **Registry name** | `ai.saifs/data-cleaning` |
| **Endpoint** | `https://data-cleaning-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-data-cleaning": {
      "type": "http",
      "url": "https://data-cleaning-mcp.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://data-cleaning-mcp.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-data-cleaning https://data-cleaning-mcp.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://data-cleaning-mcp.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

| Tool | What it does |
|---|---|
| `change_log` | Every edit (row, field, old → new, rule, method) and a reproducible run hash. |
| `check_digits` | Validates GSTIN, GTIN/EAN and other check-digit identifiers (Luhn, mod-11, mod-36). |
| `clean_dataset` | One-call pipeline: map → standardise → validate → repair → impute → flag → dedupe. |
| `compare_loads` | Drift vs previous load: PSI/KS, new/missing categories, volume and format change. |
| `discover_rules` | Finds functional dependencies with confidence and lists violating rows. |
| `fill_missing` | Hot-deck, EM or multiple imputation; values flagged “imputed” with a range. |
| `find_duplicates` | Duplicate groups only — never merges by itself. |
| `fix_errors` | Fellegi-Holt minimum-change repair so every rule passes. |
| `fix_typos_units` | Digit swaps, sign flips and ×10/×100/×1000 or lakh/crore slips. |
| `flag_outliers` | Robust z (MAD), Tukey fences, Benford screening; flags, never deletes. |
| `infer_patterns` | Learns each column's dominant format and flags values that don't fit. |
| `map_columns` | Maps messy headers to a target schema with confidence. |
| `profile_data` | Types, formats, missing %, distinct counts, suspected keys, rule suggestions. |
| `quality_score` | 0–100 agent-ready score by completeness, validity, consistency, uniqueness. |
| `relationship_outliers` | Rows whose combination of values is abnormal (Mahalanobis / robust regression). |
| `standardise_values` | Dates, numbers, currency, phones (E.164), GSTIN/PAN, company suffixes, casing. |
| `validate_rules` | Pass/fail per rule with failing rows. |

## More detail

What this cleaner returns, why a model should not repair the file, and how to use each tool are in [docs/README.md](docs/README.md).

## Example prompts

- “Clean this Tally export before you rank my customers.”
- “Check this CRM file: is it ready for the agent?”
- “Fix rows where amount doesn't equal quantity × price and show what you changed.”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
