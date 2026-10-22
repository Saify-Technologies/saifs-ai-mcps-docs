# Learning Optimizer

A personalised spaced-repetition engine: it models each learner’s forgetting curve per item and schedules the fewest reviews that still hit a target retention. Built on Ebbinghaus, Leitner / SM-2 (Anki’s baseline), Half-Life Regression and FSRS. Also builds a full study plan that peaks retention by exam day.

| | |
|---|---|
| **Registry name** | `ai.saifs/learning-optimizer` |
| **Endpoint** | `https://learning-optimizer-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-learning-optimizer": {
      "type": "http",
      "url": "https://learning-optimizer-mcp.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://learning-optimizer-mcp.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-learning-optimizer https://learning-optimizer-mcp.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://learning-optimizer-mcp.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

| Tool | What it does |
|---|---|
| `schedule_review` | Next optimal review time and predicted recall probability for one learner-item after the last result. |
| `due_reviews` | Today’s review queue for a learner, priority-ordered. |
| `predict_retention` | Probability of recall for an item at a future date. |
| `optimize_study_plan` | Full schedule over an item bank so retention peaks on exam day. |
| `forgetting_curve` | Fitted decay curve and parameters for one learner-item. |
| `explain_schedule` | Why this interval: model, evidence and confidence. |

## Example prompts

- “This student just missed item 142. When should they review it next, and what’s P(recall) then?”
- “What should this learner review today, in priority order?”
- “Exam is 28 days out. Build a study plan over this item bank so they peak on the day.”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
