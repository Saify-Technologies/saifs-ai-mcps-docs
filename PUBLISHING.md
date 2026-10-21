# Go-live checklist

Do these in order. Total time: about an hour.

## 1. Check the repo address

This kit assumes `https://github.com/Saify-Technologies/saifs-ai-mcps-docs`. If yours differs, find-and-replace it in every
`server.json`, every `README.md` and `ONBOARDING.md`.

## 2. Create the repo on GitHub

Create a **public** repository (for example `saifs-ai/mcp`), then from this folder:

```bash
git init -b main
git add .
git commit -m "Public listing for SAIFS AI MCP servers"
git remote add origin https://github.com/Saify-Technologies/saifs-ai-mcps-docs.git
git push -u origin main
```

Add a description, the website `https://saifs.ai`, and topics such as `mcp`, `mcp-server`,
`model-context-protocol`, `ai-agents`.

## 3. Republish to the official registry

Every `server.json` here has a bumped version and a new `repository` block pointing at its folder —
that link is what GitHub's registry uses. Publish once by hand:

```bash
# install the publisher (macOS/Linux)
curl -L "https://github.com/modelcontextprotocol/registry/releases/latest/download/mcp-publisher_$(uname -s | tr '[:upper:]' '[:lower:]')_$(uname -m | sed 's/x86_64/amd64/;s/aarch64/arm64/').tar.gz" | tar xz mcp-publisher

# log in with the same DNS key you used for ai.saifs
./mcp-publisher login dns --domain saifs.ai --private-key <your-private-key-hex>

for f in servers/*/server.json; do ./mcp-publisher publish "$f"; done
```

To automate future releases, add the key as the repository secret **`MCP_DNS_PRIVATE_KEY`**
(Settings → Secrets and variables → Actions) and run the **Publish to MCP Registry** workflow
(Actions tab), or push a `v*` tag.

## 4. Verify

```bash
curl -s "https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs"
```

Each server's newest version should show `isLatest: true` and include the `repository` block.

## 5. Clean up old versions (optional)

`ai.saifs/customer-intelligence` 1.0.0 points at an older endpoint
(`customer-tiering-mcp.saifs.ai`). If that endpoint is retired, mark 1.0.0 deprecated in the registry.

## 6. Request onboarding

Post the comment in [ONBOARDING.md](ONBOARDING.md) on GitHub discussion #1257 and send the email.

## Worth doing before you post

- **Tool annotations** — give every tool a `title` and `readOnlyHint` / `destructiveHint`. Curators and
  clients both look for these.
- **`resource_documentation`** — each server's OAuth metadata points at `/docs`, which currently returns
  `401`. Make `/docs` public, or point it at the README in this repo.
- **Icons** — once you have a square PNG/SVG logo at a public HTTPS URL, add an `icons` array to each
  `server.json` and republish.
