# Optimization Solver (LP / MIP Scheduling)

A general constraint-optimization server: describe the objective and constraints, get the optimal plan plus shadow prices. Pre-built templates cover production mix, blending, transportation and shift coverage so callers do not have to write an LP. Infeasible problems return the conflicting constraints.

| | |
|---|---|
| **Registry name** | `ai.saifs/optimization-solver` |
| **Endpoint** | `https://optimization-solver-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-optimization-solver": {
      "type": "http",
      "url": "https://optimization-solver-mcp.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://optimization-solver-mcp.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-optimization-solver https://optimization-solver-mcp.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://optimization-solver-mcp.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

| Tool | What it does |
|---|---|
| `explain_solution` | Which constraints bind, shadow prices (what each constraint costs) and sensitivity. |
| `optimize_blend` | Least-cost mix meeting specs (diet / blending). |
| `optimize_production_mix` | Optimal quantities and profit given products, resources and constraints. |
| `optimize_schedule` | Min-cost shift coverage meeting staffing rules. |
| `optimize_transportation` | Min-cost shipping plan from sources, destinations, supply and demand. |
| `solve_custom_lp` | General LP solver for power users from an objective and constraints. |

## More detail

What a shadow price means, why trying numbers in a spreadsheet is not an optimum, and how to fill a template are in [docs/README.md](docs/README.md).

## Example prompts

- “Find the best production mix within capacity”
- “Allocate budget across projects”
- “Schedule jobs on machines”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
