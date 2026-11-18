# Indian Railways IRCTC

Information-only Indian Railways assistant: train details by number, trains between two station codes, date-based search with classes and availability, the full route, live running status, and a station arrival and departure board. It plans a connection when no usable direct train exists. It does not book, pay, cancel, or check a PNR.

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
| `railway.train.getByNumber` | Train details for a 5-digit train number. |
| `railway.train.searchByRouteDate` | Trains, classes, and availability between two station codes on a date (`DD-MM-YYYY`, up to 60 days ahead). |
| `railway.train.betweenStations` | Trains between two station codes, with no date. |
| `railway.train.route` | Full route and stops of a train. |
| `railway.train.liveStatus` | Current running status and position of a train. |
| `railway.station.liveBoard` | Arrivals and departures at a station code. |

## More detail

What a station code and a journey date must look like, why a PNR is not looked up here, and how a connection is chosen are in [docs/README.md](docs/README.md).

## Example prompts

- “Which trains go from Ratlam (RTM) to Mumbai Central (MMCT) on 15-12-2026?”
- “Where is train 12961 right now?”
- “What is the full route of train 12961?”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
