# Assessment Intelligence

Psychometric analysis over assessment response data: Item Response Theory for item difficulty / discrimination / guessing, true ability (θ) separate from test difficulty, Cronbach’s alpha reliability, computerized adaptive next-item selection, and Bayesian Knowledge Tracing of skill mastery. Flags items that measure the wrong thing and learners whose trajectory predicts failure.

| | |
|---|---|
| **Registry name** | `ai.saifs/assessment-intelligence` |
| **Endpoint** | `https://assessment-intelligence-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-assessment-intelligence": {
      "type": "http",
      "url": "https://assessment-intelligence-mcp.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://assessment-intelligence-mcp.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-assessment-intelligence https://assessment-intelligence-mcp.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://assessment-intelligence-mcp.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

| Tool | What it does |
|---|---|
| `analyze_assessment` | Per-item IRT parameters (difficulty, discrimination, guessing) and flags for bad items. |
| `estimate_ability` | Each student’s true ability (θ), separate from how hard the test was. |
| `assessment_reliability` | Cronbach’s alpha and item-total correlations. |
| `adaptive_next_item` | Optimal next question from an item bank given current ability (CAT). |
| `track_mastery` | Bayesian Knowledge Tracing: P(mastered) per skill over a learning history. |
| `flag_at_risk_learners` | Students whose mastery / ability trajectory predicts failure — framed as support, not ranking. |
| `explain_item` | Why an item is good or bad, with its fitted parameters. |

## Example prompts

- “Here’s this mock’s response matrix. Which questions have negative discrimination and should be dropped?”
- “Estimate each student’s true ability on this test, not just their raw percentage.”
- “From this skill history, who is at risk of failing, and what has each person actually mastered?”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
