# Synthetic Data — Random User

Generates fake but realistic user profiles — name, gender, location, email, login, date of birth, phone, picture and nationality — for demos, UI fixtures, QA and prototyping, so teams never need to copy real personal data.

| | |
|---|---|
| **Registry name** | `ai.saifs/synthetic-data-random-user` |
| **Endpoint** | `https://synthetic-mint-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-synthetic-data": {
      "type": "http",
      "url": "https://synthetic-mint-mcp.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://synthetic-mint-mcp.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-synthetic-data https://synthetic-mint-mcp.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://synthetic-mint-mcp.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

| Tool | What it does |
|---|---|
| `random_users` | Generates the requested number of synthetic user profiles (name, gender, location, email, login, date of birth, phone, picture, nationality). |

## Example prompts

- “Give me 20 random users to fill our contacts demo screen.”
- “Generate 5 test profiles with emails and phone numbers for our QA fixtures.”
- “I need placeholder avatars and names for a team directory mockup.”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/saifs-ai/mcp/issues
- Privacy: https://saifs.ai/privacy_policy
