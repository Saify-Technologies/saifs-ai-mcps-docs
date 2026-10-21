# Marketing Intelligence & Optimization

Turns ad spend, sales, order baskets, A/B test counts and adoption data into ranked STOP / SCALE / SHIP / WAIT decisions with money impact and confidence. Covers marketing mix modelling (channel ROI, saturation and budget reallocation), market-basket bundles, A/B significance with power and peeking checks, Bass diffusion for new-product ramps, and CAC / LTV / payback by channel. Each part runs only when its data is supplied; MMM results are directional, not causal.

| | |
|---|---|
| **Registry name** | `ai.saifs/marketing-intelligence` |
| **Endpoint** | `https://marketing-analysis-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-marketing-intelligence": {
      "type": "http",
      "url": "https://marketing-analysis-mcp.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://marketing-analysis-mcp.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-marketing-intelligence https://marketing-analysis-mcp.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://marketing-analysis-mcp.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

| Tool | What it does |
|---|---|
| `marketing_intelligence` | Analyses payloads of up to 200 rows per data array — spend, sales, baskets, A/B tests, adoption, acquisitions, customer revenue — and returns ranked decisions plus channel ROI, budget moves, bundles, test verdicts and unit economics. |
| `marketing_intelligence_get_engine` | For larger exports (Meta/Google spend, Shopify orders): returns a numpy-only Python script that runs the same analysis locally, so no marketing data reaches the server. |

## Example prompts

- “Here's our weekly Meta, Google and email spend with sales. Which channel should we scale or cut?”
- “Is our landing-page A/B test win real, or is it under-powered?”
- “Which products should we bundle, based on these order baskets?”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
