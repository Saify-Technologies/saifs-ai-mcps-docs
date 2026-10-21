# Lead Intelligence

Scores an open sales pipeline from lead records: probability and expected time to convert, money at risk and the value of one contact, a funnel-stage outlook, multi-touch channel credit (Shapley attribution), and a daily CALL / NURTURE / VERIFY queue for SDRs and sales managers. Works from any CRM or CSV export with a lead id and a created date.

| | |
|---|---|
| **Registry name** | `ai.saifs/lead-intelligence` |
| **Endpoint** | `https://lead-scoring-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-lead-intelligence": {
      "type": "http",
      "url": "https://lead-scoring-mcp.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://lead-scoring-mcp.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-lead-intelligence https://lead-scoring-mcp.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://lead-scoring-mcp.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

| Tool | What it does |
|---|---|
| `lead_pipeline` | Scores up to 500 leads: conversion probabilities, pipeline value at risk, stage outlook, channel attribution, manager decisions (CALL TODAY / PUSH FORWARD / QUALIFY) and a per-lead ledger. Optional touch log and stage history improve the results. |
| `lead_pipeline_get_engine` | For pipelines over 500 leads: returns a runnable Python script that scores the export locally with no network calls. |

## Example prompts

- “Here's our HubSpot lead export. Who should the team call today?”
- “Which leads are going cold and how much pipeline value is at risk?”
- “Which channels actually drive conversions — email, demo or webinar?”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/saifs-ai/mcp/issues
- Privacy: https://saifs.ai/privacy_policy
