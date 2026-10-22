# Route & Logistics Optimizer

Daily routing for delivery fleets, field-service teams and sales territories: who goes where, in what order, in which vehicle. Solves VRP / TSP with time windows and capacities, assigns jobs with the Hungarian method, packs loads, and re-optimises when new orders land midday.

| | |
|---|---|
| **Registry name** | `ai.saifs/route-optimizer` |
| **Endpoint** | `https://route-optimizer-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-route-optimizer": {
      "type": "http",
      "url": "https://route-optimizer-mcp.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://route-optimizer-mcp.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-route-optimizer https://route-optimizer-mcp.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://route-optimizer-mcp.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

| Tool | What it does |
|---|---|
| `optimize_routes` | Per-vehicle ordered routes from stops, fleet and constraints (time windows, capacities, max shift length). |
| `optimize_single_route` | TSP for one vehicle or sales rep. |
| `assign_jobs` | Optimal worker→job assignment (Hungarian method) from a cost matrix. |
| `pack_loads` | Bin-packing load plans against vehicle capacities. |
| `reoptimize_midday` | Dynamic re-route from the current state plus new orders. |
| `route_map` | Map for a finished route. |
| `explain_route` | Cost breakdown, constraint bindings and savings versus a naive stop order. |

## Example prompts

- “Here are today’s 40 stops and 6 vans. Build routes that honour delivery windows.”
- “Assign these field jobs to technicians by skill and travel cost, then route each person.”
- “Three new orders just landed. Re-optimise from where the drivers are now.”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
