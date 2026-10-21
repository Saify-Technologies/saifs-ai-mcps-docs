# Hiring Fairness & Selection ROI

Group-level fairness and value analysis of a hiring process. From counts per stage and group it returns selection rates and impact ratios at every stage (EEOC four-fifths rule), whether each gap is larger than chance, the end-to-end funnel, and a like-for-like check across jobs that catches Simpson's paradox. From anonymous score/outcome pairs it tells you whether a test predicts job success, whether it underrates a group, a fair pass mark, and its money value net of cost. It never scores, ranks or selects individuals, and its output is analysis, not legal advice.

| | |
|---|---|
| **Registry name** | `ai.saifs/hiring-fairness` |
| **Endpoint** | `https://hiring-fairness-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-hiring-fairness": {
      "type": "http",
      "url": "https://hiring-fairness-mcp.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://hiring-fairness-mcp.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-hiring-fairness https://hiring-fairness-mcp.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://hiring-fairness-mcp.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

| Tool | What it does |
|---|---|
| `hiring_fairness` | Analyses up to 5,000 rows of stage counts (or stage/group/selected candidate rows, or anonymous score/outcome pairs) and returns decision cards such as REVIEW THIS STAGE, COMPARE LIKE WITH LIKE, TEST PREDICTS, SET CUTOFF and WORTH THE COST. |
| `hiring_fairness_get_engine` | For larger ATS exports: returns a numpy-only Python script that aggregates candidate rows to group counts locally and runs the same analysis, so no candidate data reaches the server. |

## Example prompts

- “Here are our applicant and hire counts by stage and gender. Is any stage showing adverse impact?”
- “Is our coding assessment worth what it costs, and what pass mark is fair?”
- “Compare selection rates across departments like for like.”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
