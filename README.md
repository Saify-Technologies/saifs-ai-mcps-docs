# SAIFS AI — MCP servers

Hosted [Model Context Protocol](https://modelcontextprotocol.io) servers from [SAIFS AI](https://saifs.ai) for sales,
inventory, marketing, HR, media generation and travel. Every server is remote (Streamable HTTP) with
OAuth 2.1 sign-in, so there is nothing to install — add the URL to any MCP client.

All servers are published in the [official MCP Registry](https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs)
under the DNS-verified `ai.saifs` namespace.

| Server | Registry name | What it does |
|---|---|---|
| [Customer Intelligence & Segmentation](servers/customer-intelligence) | `ai.saifs/customer-intelligence` | Segment customers into A/B/C/D tiers, flag at-risk accounts and build a daily sales call list. |
| [Demand Forecasting](servers/demand-forecasting) | `ai.saifs/demand-forecasting` | Forecast product demand from historical sales and market signals. |
| [Inventory Intelligence & Optimizer](servers/inventory-intelligence-optimizer) | `ai.saifs/inventory-intelligence-optimizer` | Work out what to order, when to reorder, what is overstocked and how much cash is tied up in stock. |
| [Lead Intelligence](servers/lead-intelligence) | `ai.saifs/lead-intelligence` | Score leads to identify high-potential prospects and prioritize sales outreach. |
| [Marketing Intelligence & Optimization](servers/marketing-intelligence) | `ai.saifs/marketing-intelligence` | Channel ROI and budget mix, product bundles, A/B test verdicts and CAC/LTV from marketing data. |
| [Hiring Fairness & Selection ROI](servers/hiring-fairness) | `ai.saifs/hiring-fairness` | Detect hiring adverse impact and estimate whether a selection method is worth its cost. |
| [Media Generator — Images, Videos & Audios](servers/media-generator-images-videos) | `ai.saifs/media-generator-images-videos` | Generate marketing images, logos, videos, music and speech for campaigns, products and brand assets. |
| [Synthetic Data — Random User](servers/synthetic-data-random-user) | `ai.saifs/synthetic-data-random-user` | Generate synthetic user data for testing, demos and development without real personal data. |
| [Indian Railways IRCTC](servers/indian-railways-irctc) | `ai.saifs/indian-railways-irctc` | Look up Indian Railways trains, routes, live running status, station boards and PNR status. |

## Quick start

1. Open the folder for the server you want and copy its URL.
2. Add it to your client — VS Code / Copilot (`mcp.json`), Claude (Settings → Connectors), Claude Code, Cursor or any MCP client.
3. Sign in when the client prompts you.

Each folder's README has client-specific config, the tool list and example prompts.

## This repository

This repo holds the public listing for each server: README, `server.json` (the registry manifest) and
install config. The servers themselves are hosted by SAIFS AI. New versions are published to the
official MCP Registry by [`.github/workflows/publish-mcp.yml`](.github/workflows/publish-mcp.yml).

## Support

Open an [issue](https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues) or visit https://saifs.ai.

## License

The documentation and configuration in this repository are MIT licensed. Use of the hosted services is
subject to SAIFS AI's terms.
