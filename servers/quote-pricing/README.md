# Quote Price Optimization (Win Probability)

Win-probability and expected-margin pricing for B2B quotes and tenders. Quote history with outcomes + the new deal's attributes in → win-probability curve, recommended price range, expected margin at each price, and discount guardrails. Uses Friedman/Gates bidding and logistic win models. No competitor price-sharing; no pricing on protected attributes.

| | |
|---|---|
| **Registry name** | `ai.saifs/quote-pricing` |
| **Endpoint** | `https://quote-pricing-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-quote-pricing": {
      "type": "http",
      "url": "https://quote-pricing-mcp.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://quote-pricing-mcp.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-quote-pricing https://quote-pricing-mcp.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://quote-pricing-mcp.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

| Tool | What it does |
|---|---|
| `discount_leakage` | Price waterfall showing where margin leaks by rep, segment and product line. |
| `explain_price` | Why this range: segment, deal size, competition, and how win odds change with price. |
| `fit_win_model` | Win-probability model per segment; returned as a reusable snapshot. |
| `price_recommendation` | Price range, P(win) and expected margin at each price point. |
| `tender_bid` | Friedman/Gates sealed-bid recommendation for tenders. |
| `win_loss_report` | Win rate and margin by segment, price band and rep team. |

## More detail

What the price range means, why a flat discount leaves margin behind, and how to send closed quotes are in [docs/README.md](docs/README.md).

## Example prompts

- “What price should we quote for this deal?”
- “Bid for this tender with 4 competitors.”
- “Where are we leaking discount?”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
