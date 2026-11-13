# Financial Fraud & Anomaly Detection

Forensic-analytics MCP for finance, audit and procurement. Runs Benford's Law, outlier detection, duplicate/split-invoice checks and Beneish M-Score over ledger and transaction data — flags items for human review with a statistical explanation. Never accuses fraud; every flag is a signal for investigation, not proof.

| | |
|---|---|
| **Registry name** | `ai.saifs/financial-anomaly` |
| **Endpoint** | `https://financial-anomaly-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-financial-anomaly": {
      "type": "http",
      "url": "https://financial-anomaly-mcp.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://financial-anomaly-mcp.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-financial-anomaly https://financial-anomaly-mcp.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://financial-anomaly-mcp.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

| Tool | What it does |
|---|---|
| `benford_analysis` | First/second-digit distribution vs expected, chi-square / MAD, flagged deviations. |
| `detect_duplicate_payments` | Near-duplicate and split-invoice detection before payment. |
| `earnings_manipulation_score` | Beneish M-Score for public-company earnings-manipulation screening. |
| `explain_flag` | Why an item was flagged and the statistical basis (for the investigator). |
| `flag_anomalous_transactions` | Statistical outliers: amounts, round-number bias, duplicate patterns. |
| `watch_transactions` | Continuous Benford/anomaly monitoring; alert on deviation. |

## Example prompts

- “Flag unusual journal entries this quarter.”
- “Run Benford's test on expenses.”
- “Check this company's M-score.”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
