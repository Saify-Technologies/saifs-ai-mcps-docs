# Payment Reconciliation & Matching

Match receipts, bank lines and GST 2B rows to open invoices and the ledger. Proposes one-to-many and many-to-one matches with confidence and reasons, flags deductions (TDS, bank charges, part payments), and returns an exceptions queue plus updated matching rules. Low-confidence matches always go to a person; nothing is auto-posted. We never connect to banks or the GST portal.

| | |
|---|---|
| **Registry name** | `ai.saifs/payment-reconciliation` |
| **Endpoint** | `https://payment-reconciliation-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-payment-reconciliation": {
      "type": "http",
      "url": "https://payment-reconciliation-mcp.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://payment-reconciliation-mcp.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-payment-reconciliation https://payment-reconciliation-mcp.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://payment-reconciliation-mcp.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

| Tool | What it does |
|---|---|
| `explain_match` | Why these items match — amount combination, name score, date gap and reference. |
| `gst_2b_match` | India GSTR-2B vs purchase register: matched, mismatched, missing, and ITC at risk. |
| `match_bank_to_ledger` | Bank reconciliation with one-to-many and many-to-one matches. |
| `match_receipts` | Cash application including combinations, part payments and deductions. |
| `reconciliation_report` | Matched %, unmatched value, ageing of exceptions and ITC at risk. |
| `suggest_rules` | New matching rules learned from confirmed matches, returned in `rules_snapshot`. |

## Example prompts

- “Match today’s receipts to open invoices”
- “Reconcile the bank statement”
- “Run GSTR-2B vs purchase register”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
