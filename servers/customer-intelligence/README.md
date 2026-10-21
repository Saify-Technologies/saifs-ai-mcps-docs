# Customer Intelligence & Segmentation

Ranks a customer book into A/B/C/D tiers from purchase history, flags accounts that are slipping or ready to grow, estimates revenue at risk and upside, and builds a short SAVE / GROW / VERIFY call list for sales reps. Works from CSV, Excel, Tally, Salesforce, HubSpot, Zoho or any CRM export with a customer id, a date and an amount.

| | |
|---|---|
| **Registry name** | `ai.saifs/customer-intelligence` |
| **Endpoint** | `https://customer-tiering-mcp-latest.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-customer-intelligence": {
      "type": "http",
      "url": "https://customer-tiering-mcp-latest.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://customer-tiering-mcp-latest.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-customer-intelligence https://customer-tiering-mcp-latest.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://customer-tiering-mcp-latest.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

| Tool | What it does |
|---|---|
| `customer_tiering` | Scores a customer book of up to 500 transaction rows into A/B/C/D tiers (BG/NBD survival model, Gamma-Gamma spend model, tier migration, money layer) and returns decision cards and a per-customer ledger. Cleans customer-name variants before scoring. |
| `customer_tiering_get_engine` | For books over 500 rows: returns a runnable Python script that scores the file locally, so customer data never reaches the server. |

## Example prompts

- “Here's our sales export. Who are our A accounts, and who should we call this week?”
- “Which customers are going quiet and how much revenue is at risk?”
- “Why is customer ACME-104 a C account?”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/saifs-ai/mcp/issues
- Privacy: https://saifs.ai/privacy_policy
