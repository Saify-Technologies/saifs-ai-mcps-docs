# Indian Railways IRCTC

Information-only Indian Railways assistant: find trains between stations (with or without a date), see full routes and stops, live running status, station arrival/departure boards and PNR status. It does not book, pay for or cancel tickets.

| | |
|---|---|
| **Registry name** | `ai.saifs/indian-railways-irctc` |
| **Endpoint** | `https://transport-agent-claude.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-indian-railways": {
      "type": "http",
      "url": "https://transport-agent-claude.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://transport-agent-claude.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-indian-railways https://transport-agent-claude.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://transport-agent-claude.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

| Tool | What it does |
|---|---|
| `railway_train_getByNumber` | Train details for a 5-digit train number. |
| `railway_train_searchByRouteDate` | Trains, classes and availability between two stations on a date (DD-MM-YYYY, up to 60 days ahead). |
| `railway_train_betweenStations` | Trains between two station codes, no date needed. |
| `railway_train_route` | Full route and stops of a train. |
| `railway_train_liveStatus` | Current running status and position of a train. |
| `railway_station_liveBoard` | Arrivals and departures board for a station. |
| `railway_train_pnrStatus` | Status of a 10-digit PNR. |

## Example prompts

- “Which trains go from Ratlam (RTM) to Mumbai Central (MMCT) on 05-11-2026?”
- “Where is train 12961 right now?”
- “Check PNR 2832453235.”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/saifs-ai/mcp/issues
- Privacy: https://saifs.ai/privacy_policy
