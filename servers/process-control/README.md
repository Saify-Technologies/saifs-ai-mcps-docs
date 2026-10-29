# Statistical Process Control & Anomaly Alerts

Continuous statistical monitoring of any metric stream — support response time, error rates, delivery SLAs, defect rates, conversion — alerting only on genuine out-of-control signals. Shewhart charts plus Western Electric / Nelson run rules, with CUSUM and EWMA for small sustained drifts that a 3σ chart misses. Decision support only; operators own stop/go.

| | |
|---|---|
| **Registry name** | `ai.saifs/process-control` |
| **Endpoint** | `https://process-monitor-mcp-microsoft.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-process-control": {
      "type": "http",
      "url": "https://process-monitor-mcp-microsoft.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://process-monitor-mcp-microsoft.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-process-control https://process-monitor-mcp-microsoft.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://process-monitor-mcp-microsoft.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

| Tool | What it does |
|---|---|
| `detect_shift` | CUSUM / EWMA for small sustained drifts that Shewhart charts miss. |
| `explain_signal` | Which rule fired, how many sigma, and what it means. |
| `monitor_metric` | In-control / out-of-control verdict plus which rule fired. |
| `set_control_limits` | Compute ±3σ limits from a stable baseline. |
| `watch_process` | Continuous watch; alert on rule violations. |

## Example prompts

- “Is line 3 drifting out of spec?”
- “Alert me when defect rate shifts”
- “Calculate Cpk for this process”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
