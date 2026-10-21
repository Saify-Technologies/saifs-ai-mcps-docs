# GitHub MCP Registry onboarding request

Post the comment below on <https://github.com/github/github-mcp-server/discussions/1257>
**after** the steps in [PUBLISHING.md](PUBLISHING.md) are done. Replace `YYYY-MM-DD` with the date you
ran the two `curl` checks.

---

**Onboarding request: SAIFS AI — 9 hosted servers under the DNS-verified `ai.saifs` namespace**

Following the OSS-registry-first, manual-curation process confirmed in this thread (publish to the official registry, request initial onboarding, versions sync afterwards). Also sent to partnerships@github.com.

All 9 are remote Streamable HTTP servers, `active` / `isLatest` in the official MCP Registry.

| Server | Registry name | Ver | Endpoint | What it does |
|---|---|---|---|---|
| Customer Intelligence & Segmentation | `ai.saifs/customer-intelligence` | 1.0.2 | `https://customer-tiering-mcp-latest.saifs.ai/mcp` | Segment customers into A/B/C/D tiers, flag at-risk accounts and build a daily sales call list. |
| Demand Forecasting | `ai.saifs/demand-forecasting` | 1.0.1 | `https://demand-forecaster-mcp.saifs.ai/mcp` | Forecast product demand from historical sales and market signals. |
| Inventory Intelligence & Optimizer | `ai.saifs/inventory-intelligence-optimizer` | 1.0.1 | `https://inventory-optimizer-mcp.saifs.ai/mcp` | Work out what to order, when to reorder, what is overstocked and how much cash is tied up in stock. |
| Lead Intelligence | `ai.saifs/lead-intelligence` | 1.0.1 | `https://lead-scoring-mcp.saifs.ai/mcp` | Score leads to identify high-potential prospects and prioritize sales outreach. |
| Marketing Intelligence & Optimization | `ai.saifs/marketing-intelligence` | 1.0.1 | `https://marketing-analysis-mcp.saifs.ai/mcp` | Channel ROI and budget mix, product bundles, A/B test verdicts and CAC/LTV from marketing data. |
| Hiring Fairness & Selection ROI | `ai.saifs/hiring-fairness` | 1.0.1 | `https://hiring-fairness-mcp.saifs.ai/mcp` | Detect hiring adverse impact and estimate whether a selection method is worth its cost. |
| Media Generator — Images, Videos & Audios | `ai.saifs/media-generator-images-videos` | 1.0.1 | `https://mcp.saifs.ai/mcp` | Generate marketing images, logos, videos, music and speech for campaigns, products and brand assets. |
| Synthetic Data — Random User | `ai.saifs/synthetic-data-random-user` | 1.0.1 | `https://synthetic-mint-mcp.saifs.ai/mcp` | Generate synthetic user data for testing, demos and development without real personal data. |
| Indian Railways IRCTC | `ai.saifs/indian-railways-irctc` | 1.0.1 | `https://transport-agent-claude.saifs.ai/mcp` | Look up Indian Railways trains, routes, live running status, station boards and PNR status. |

- **Publisher:** SAIFS AI — https://saifs.ai
- **Repository:** https://github.com/saifs-ai/mcp (one folder per server: README, tool list, VS Code `mcp.json`, `server.json`; each `server.json` links its folder via `repository.subfolder`)
- **Auth:** OAuth 2.1 on every server — unauthenticated requests return `401` with `WWW-Authenticate: Bearer` and an RFC 9728 `resource_metadata` pointer; dynamic client registration and PKCE S256
- **Privacy:** https://saifs.ai/privacy_policy

**Verification (YYYY-MM-DD)**
```
curl -s "https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs"
→ all 9 servers, status: active, isLatest: true

curl -s "https://api.mcp.github.com/v0.1/servers?search=ai.saifs"
→ {"servers":[],"metadata":{"count":0,"total":0}}
```

Could the curation team consider onboarding these so future official-registry versions sync automatically? We're happy to provide test accounts, a demo, or any metadata changes needed. Thanks!

---

## Email to partnerships@github.com

**Subject:** GitHub MCP Registry onboarding request — SAIFS AI (ai.saifs, 9 servers)

Hello GitHub MCP Registry team,

We'd like to request onboarding of SAIFS AI's nine hosted MCP servers to github.com/mcp. All nine are
published and active in the official MCP Registry under our DNS-verified `ai.saifs` namespace, and we
publish new versions there, so they will stay in sync once onboarded.

- Servers and endpoints: see the table at https://github.com/saifs-ai/mcp#readme
- Public listing repository: https://github.com/saifs-ai/mcp
- Auth: OAuth 2.1 with dynamic client registration and PKCE on every server
- Onboarding comment: <link to your comment on discussion #1257>

We can provide test accounts or a short demo for review. Thank you!

Best regards,
<Name>, SAIFS AI
<email> · https://saifs.ai
