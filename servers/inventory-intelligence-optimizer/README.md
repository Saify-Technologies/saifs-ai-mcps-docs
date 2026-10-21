# Inventory Intelligence & Optimizer

Takes a stock list (and sales history if available) and returns order quantities, reorder points, safety stock, overstock, cash tied up in inventory, and a buying list grouped by supplier. Accepts normal business column names (Item Code, Closing Stock, Open PO, Lead Time…) without renaming.

| | |
|---|---|
| **Registry name** | `ai.saifs/inventory-intelligence-optimizer` |
| **Endpoint** | `https://inventory-optimizer-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-inventory-optimizer": {
      "type": "http",
      "url": "https://inventory-optimizer-mcp.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://inventory-optimizer-mcp.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-inventory-optimizer https://inventory-optimizer-mcp.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://inventory-optimizer-mcp.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

| Tool | What it does |
|---|---|
| `inventory_optimizer` | Scores up to about 200 products and 2,500 sales rows: order quantities, reorder points, overstock, cash exposure and a supplier-grouped purchase queue. |
| `inventory_optimizer_get_engine` | For larger stock books: returns a runnable Python script that does the same analysis locally, so the data never leaves the user's machine. |

## Example prompts

- “Here's our stock report. What should we order this week and how much?”
- “Which items are overstocked and how much cash is stuck in them?”
- “Build a purchase list grouped by supplier within a budget of ₹5,00,000.”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
