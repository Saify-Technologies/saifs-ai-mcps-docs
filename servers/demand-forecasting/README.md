# Demand Forecasting

Forecasts demand per product (and optionally per region or channel) from sales history — daily, weekly or monthly — with a range for every number. It picks the best-fitting model by backtest, handles slow and intermittent movers (Croston/SBA), refuses to guess when history is too short, and hands lead-time demand to the Inventory Intelligence & Optimizer server for safety stock. Reads Excel, Tally and CSV exports with their own column names.

| | |
|---|---|
| **Registry name** | `ai.saifs/demand-forecasting` |
| **Endpoint** | `https://demand-forecaster-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-demand-forecasting": {
      "type": "http",
      "url": "https://demand-forecaster-mcp.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://demand-forecaster-mcp.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-demand-forecasting https://demand-forecaster-mcp.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://demand-forecaster-mcp.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

| Tool | What it does |
|---|---|
| `demand_forecaster` | Forecasts demand from sales rows (date + quantity is enough; SKU, region, channel and lead time optional) for up to about 200 products and 2,500 rows. Returns forecasts with prediction intervals, the chosen model and its backtest score, and lead-time demand for reordering. |
| `demand_forecaster_get_engine` | For larger sales files: returns a runnable Python script that produces the same forecasts locally, so the data never leaves the user's machine. |

## Example prompts

- “Here's our Tally sales export. How much of each item will we sell over the next 13 weeks?”
- “Which products are slow movers, and what should we expect from them next month?”
- “Forecast monthly demand for spare part SP-220 with a 95% range.”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
