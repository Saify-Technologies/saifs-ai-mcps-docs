# Dynamic Pricing & Overbooking

Yield-management math over booking, capacity and price data for perishable capacity — hotel rooms, event tickets, rental equipment, clinic slots, restaurant covers. Recommends the price now, protection levels for higher-value segments, elasticity, a safe overbooking limit, and alerts when booking pace runs ahead or behind. Returns ranges and the probability of denying service; it does not auto-execute prices.

| | |
|---|---|
| **Registry name** | `ai.saifs/revenue-optimization` |
| **Endpoint** | `https://revenue-optimizer-mcp-microsoft.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-revenue-optimization": {
      "type": "http",
      "url": "https://revenue-optimizer-mcp-microsoft.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://revenue-optimizer-mcp-microsoft.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-revenue-optimization https://revenue-optimizer-mcp-microsoft.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://revenue-optimizer-mcp-microsoft.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

| Tool | What it does |
|---|---|
| `denied_service_risk` | Probability and expected number of customers turned away at a booking limit. |
| `estimate_elasticity` | How demand responds to price from price and sales history. |
| `explain_recommendation` | The revenue-management logic and assumptions behind a recommendation. |
| `optimal_overbooking` | Booking limit that maximises expected profit given show-up rate. |
| `optimize_price` | Recommended price now from remaining capacity, days to deadline and demand. |
| `protection_levels` | How much capacity to protect for higher-value segments (Littlewood / EMSR). |
| `recommend_overbooking` | Safe overbooking level from no-show rate, capacity and costs. |
| `show_rate_estimate` | Show-up rate by route, day or segment with confidence. |
| `watch_booking_pace` | Alert when booking pace runs ahead or behind, triggering price moves. |

## Example prompts

- “How many rooms should we overbook?”
- “Set fare buckets for next week”
- “What price maximizes revenue?”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
