# Collections Optimizer

A daily collections work-queue for banks, NBFCs, MFIs, BNPL, telecom and utilities. Scores propensity-to-pay and self-cure, ranks accounts by expected recovery *uplift from contact* (the moveable middle, not oldest or biggest first), recommends channel / timing / tone, and assigns work to agents. Contact-hour and frequency caps are hard constraints.

| | |
|---|---|
| **Registry name** | `ai.saifs/collections-optimizer` |
| **Endpoint** | `https://collections-optimizer-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-collections-optimizer": {
      "type": "http",
      "url": "https://collections-optimizer-mcp.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://collections-optimizer-mcp.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-collections-optimizer https://collections-optimizer-mcp.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://collections-optimizer-mcp.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

| Tool | What it does |
|---|---|
| `score_propensity` | P(will pay), P(self-cure) and expected recovery for an account. |
| `prioritize_queue` | Daily work-list ranked by expected recovery uplift from contact. |
| `recommend_contact_strategy` | Best channel, timing and tone-tier (call / WhatsApp / notice) within fair-collection hours. |
| `flag_self_cure` | Accounts likely to pay without contact — skip to avoid wasted or excess outreach. |
| `predict_roll_rate` | Survival-based bucket-to-bucket roll / cure forecast for provisioning. |
| `optimize_agent_allocation` | Assign accounts to agents by skill and success history (Hungarian method). |
| `explain_priority` | Why this account and this strategy — for agent trust and audit. |
| `watch_queue` | Nightly re-score so the morning queue is fresh. |

## Example prompts

- “Here’s our overdue book. Who should agents work today, and who will likely self-cure?”
- “For account A-1042, what’s the best channel and time to contact, within permitted hours?”
- “Allocate this queue across 8 agents by who recovers which product best.”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
