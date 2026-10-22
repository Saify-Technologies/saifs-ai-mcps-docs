# Procurement & Supplier Selection

Explained, audit-ready supplier choice and buying-quantity decisions from delivery, spend and supplier scores. Weights suppliers with AHP, ranks them with TOPSIS, scores on-time / defect reliability, sizes lots with quantity-discount EOQ, and maps the book into Kraljic spend quadrants. Public-tender mode treats the ranking as supporting evidence only.

| | |
|---|---|
| **Registry name** | `ai.saifs/procurement-and-supplier-selection` |
| **Endpoint** | `https://procurement-and-supply-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-procurement-supplier-selection": {
      "type": "http",
      "url": "https://procurement-and-supply-mcp.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://procurement-and-supply-mcp.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-procurement-supplier-selection https://procurement-and-supply-mcp.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://procurement-and-supply-mcp.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

| Tool | What it does |
|---|---|
| `procurement_supplier_selection` | For up to 1,000 delivery rows, 500 spend items and 60 suppliers: AHP weights, TOPSIS ranking, vendor reliability, EOQ order sizing, Kraljic spend map and a decision record (FOLLOW TENDER RULES, STOP USING, ORDER, NEGOTIATE…). |
| `procurement_get_engine` | For larger GRN / spend files: returns a runnable numpy-only script that runs the same engine locally, so the data never leaves the user's machine. |

## Example prompts

- “Here are last year’s GRNs and spend. Who should we keep, and who should we stop using?”
- “We have three quotes and on-time history. Rank the suppliers and say if the scores are too close to award.”
- “What lot size should we buy given these quantity-discount slabs?”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
