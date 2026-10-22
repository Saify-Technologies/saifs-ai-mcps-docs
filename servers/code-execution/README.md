# Mathematical Code Execution

A Python-only sandbox for agent hosts. The model writes Python that prints a result (or a one-page PDF via preinstalled `fpdf`); this server runs it in an isolated process with no network, no user filesystem and no package installs. Other languages are refused. numpy is available; scipy and pandas are not.

| | |
|---|---|
| **Registry name** | `ai.saifs/code-execution` |
| **Endpoint** | `https://code-execution-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-code-execution": {
      "type": "http",
      "url": "https://code-execution-mcp.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://code-execution-mcp.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-code-execution https://code-execution-mcp.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://code-execution-mcp.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

| Tool | What it does |
|---|---|
| `execute_code` | Runs allow-listed Python and returns stdout, stderr, exit code, and any PDF written with `fpdf` (`output("name.pdf")`, download link expires in 1 hour). Non-Python is rejected with a one-line warning. |

## Example prompts

- “Run Python to compute the mean and standard deviation of this list and print the result.”
- “Write a short numpy snippet that multiplies these two matrices and prints the product.”
- “Generate a one-page PDF invoice summary with fpdf and give me the download link.”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
