# 13-Week Cash Flow Forecast

Behaviour-based receipts and disbursements forecasting for SMB and mid-market finance. Open AR/AP, payroll, recurring items and payment history in → week-by-week cash in, cash out and closing balance for 13 weeks with P10/P50/P90, minimum-cash breach risk, drivers and accuracy versus the last forecast. No interest, borrowing or investment advice.

| | |
|---|---|
| **Registry name** | `ai.saifs/cash-forecast` |
| **Endpoint** | `https://cash-forecast-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-cash-forecast": {
      "type": "http",
      "url": "https://cash-forecast-mcp.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://cash-forecast-mcp.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-cash-forecast https://cash-forecast-mcp.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://cash-forecast-mcp.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

| Tool | What it does |
|---|---|
| `accuracy_report` | Forecast vs actual by week, bias and MAPE so trust builds over time. |
| `breach_risk` | Probability cash falls below the minimum in each week, and the earliest risky week. |
| `explain_week` | Which receipts and payments drive that week, and what would change it. |
| `forecast_cash` | 13-week P10/P50/P90 cash forecast from AR/AP, payroll, recurring and behaviour. |
| `payment_behaviour` | Days-to-pay distribution per customer (and segment for thin history). |
| `what_if` | Scenarios such as a late customer, delayed supplier, or large order in week 5. |

## More detail

What this server is, why a due-date forecast misses the cash, and how to use each tool are in [docs/README.md](docs/README.md).

## Example prompts

- “Will we have enough cash in week 9?”
- “Build our 13-week cash forecast.”
- “What if our top customer pays 3 weeks late?”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
