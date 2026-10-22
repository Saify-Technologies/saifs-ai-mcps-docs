# Mastery Pathing

The “what should this learner study next?” engine: a validated prerequisite graph, per-skill mastery (Bayesian Knowledge Tracing / Elo), and a knowledge-frontier recommendation. Diagnoses the gaps blocking a goal and returns a mastery-aware sequence from the current state to that goal.

| | |
|---|---|
| **Registry name** | `ai.saifs/mastery-pathing` |
| **Endpoint** | `https://mastery-pathing-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-mastery-pathing": {
      "type": "http",
      "url": "https://mastery-pathing-mcp.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://mastery-pathing-mcp.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-mastery-pathing https://mastery-pathing-mcp.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://mastery-pathing-mcp.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

| Tool | What it does |
|---|---|
| `build_knowledge_graph` | Validated prerequisite DAG from topics and dependencies (cycle detection, gap flags). |
| `estimate_mastery` | Per-skill mastery via BKT / Elo across the graph from learner responses. |
| `next_best_topic` | Knowledge frontier: what the learner is ready to learn now (prerequisites met, not yet mastered). |
| `diagnose_gaps` | Prerequisite gaps blocking a target topic, plus a remediation path. |
| `optimal_sequence` | Topological, mastery-aware path from the current state to a goal. |
| `rate_difficulty` | Elo-based item and learner ratings on one scale. |
| `explain_recommendation` | Why this topic next, with mastery evidence. |

## Example prompts

- “Here’s our syllabus with prerequisites. What should this student study next?”
- “They want to reach quadratic equations. Which gaps are blocking that, and in what order should we remediate?”
- “Build a learning sequence from their current mastery to the chapter on recursion.”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
