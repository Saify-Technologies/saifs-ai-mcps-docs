# 11 · Synthetic Data Mint — Privacy-Safe Dataset Generation MCP
**Score: 58/100 · Phase: LATER / opportunistic — crowded, funded space**

## Problem statement
Teams need realistic data they cannot use: production data is confidential (DPDP/GDPR), test environments need volume, demos need believable records, ML training needs augmentation. Hand-rolled fakes are statistically wrong; agent-sandbox attempts produce toy fidelity with zero privacy guarantees. What buyers need is statistically-faithful synthetic data WITH a privacy attestation someone signed.

## Solution
Schema/sample in → synthetic datasets out, with fidelity metrics + privacy certificate (doc-04 signing applied to data).

**Tool set (v1 if built):**
- `mint_from_schema(schema, rules, row_count)` — structured synthetic records
- `mint_from_sample(sample_csv, privacy_level)` — model the shape, never copy the rows
- `mint_documents(type, locale, count)` — synthetic invoices/IDs/statements for testing pipelines (watermarked "SPECIMEN" — fraud-safety non-negotiable)
- `fidelity_report(dataset_id)` — statistical similarity metrics
- `privacy_certificate(dataset_id)` — attested generation parameters, DP budget where applied

## Honest competitive note (why 58)
Gretel, Mostly AI, Tonic, SDV occupy this space with funding and enterprise sales teams. Our only defensible angles: (a) agent-native delivery inside marketplaces they ignore, (b) the attestation layer, (c) India-shaped synthetic data (Aadhaar-format-safe specimens, Indian names/addresses/GST-valid invoice structures — genuinely underserved). Without at least one of these angles winning, do not build.

## Audience
QA/dev teams, sales engineers needing demo data, ML teams, and (India angle) fintech/insurer test environments under DPDP pressure.

## User profiles
1. **QA lead** — "mint 50K orders matching our schema with seasonal patterns" for load testing.
2. **Sales engineer** — believable demo tenant in minutes, zero real-customer risk.
3. **Indian fintech compliance** — production-like test data with a certificate to show auditors.

## Where to get the APIs / components
| Capability | Provider | URL |
|---|---|---|
| Synthetic engines | SDV (OSS, tabular), Gretel API (buy-not-build option), YData | sdv.dev, gretel.ai |
| Differential privacy | OpenDP / SmartNoise (OSS) | opendp.org |
| Faker-class primitives | Faker (OSS) + our Indian-locale packs | faker.readthedocs.io |
| Document rendering | Our doc-01/03 stack (specimen invoices etc.) | internal |

## How to get them
1. SDV/Faker/OpenDP: OSS installs — v1 buildable without vendors.
2. Gretel API: self-serve key — evaluate buy-vs-build; wrapping Gretel alone fails Gate 1, so only as a backend behind OUR attestation layer.
3. Indian-locale packs: we author (names, addresses, PIN/IFSC/GST-checksum-valid formats) — the differentiating grunt work.

## API documentation (shape)
- SDV: `SingleTablePreset(metadata).fit(df).sample(n)` — library, not API; ours to serve.
- Gretel: `POST /projects/{id}/models` train → generate. Docs: docs.gretel.ai
- Our contract: `{dataset_url, fidelity_report, privacy_certificate}` — no dataset ships without both attachments.

## Cost side
OSS engines ≈ compute only; Gretel usage-priced if used; storage trivial.

## Monetization
- Per-1K synthetic rows: 2 credits; specimen documents: doc-01 credit rates
- **Test-Data plan ₹8,200/$99 mo**: schemas saved, regeneration on demand, certificates included
- India compliance tier: certificate + DPDP-mapped documentation — premium.

## India sole-prop notes
Specimen documents MUST be visibly watermarked + logged (anti-fraud duty — also a certification review question). DPDP positioning is the sales hook domestically.

## Risks
Crowded market (only proceed on the India/attestation angles); misuse for fraud (watermarks, moderation, refusal logging); fidelity disputes (publish metrics, never promise "indistinguishable").

---

---

## LISTING & DISCOVERABILITY
*Keep this section current. It drives marketplace search, AI tool selection and the buying decision.*

**Who discovers this MCP:** agent developers, developers and agent builders (Copilot Studio makers, Agentforce, Breeze and Gemini Enterprise builders, Claude/ChatGPT connector users). They find it, test it, and **champion the purchase** to the budget owner. Every field below is written for them first, with a business tagline the manager can understand.

### 1. Names
| Field | Use this | Rule |
|---|---|---|
| **Listing title** | **Synthetic Test Data Generator — Saifs AI** | Capability keyword first; ≤ 60 characters before "— Saifs AI" |
| **Tagline** (for the budget owner) | Fake but realistic user profiles for demos, QA and testing | Plain business words; ≤ 90 characters |
| **Server / connector ID** | `saifs-synthetic-data` | Lowercase, hyphenated, no coined words |
| **Internal code name** | Synthetic Data Mint (doc 11) | Docs and code only, **never** in a listing |

### 2. Description: the first 160 characters decide the click
1. **Developer contract:** Count (JSON) → synthetic user profiles (name, email, address, phone, avatar).
2. **Business value (for the champion):** No real personal data in test environments.
3. **Methods (specialist search and credibility):** uses synthetic generation.
4. **Trust:** stateless; data is processed in the request and never stored; per-user OAuth sign-in. Synthetic only; never real people.

### 3. Keywords
- **Developer search terms (title, description, tags):** synthetic data, fake user generator, test data, mock data api, random user api, qa test data
- **Business phrases (tagline, FAQ, tool descriptions):** "give me sample users", "fake customers for a demo"
- **Method terms (description body):** synthetic generation
- **Platform terms (only where actually listed):** MCP server, Copilot Studio connector, Agentforce action, Breeze agent tool, Gemini Enterprise, Amazon Bedrock AgentCore, Claude connector, ChatGPT app

### 4. Tools as shown in the listing (alphabetical)
`random_users`

**Tool-description rule:** the AI picks tools from these descriptions, so they count as discoverability too.
- Start with a verb and the business noun.
- Name the input fields.
- Say what comes back.
- Say when **not** to use the tool.

### 5. Example prompts (listing, demo video, tool descriptions)
- "Generate 20 fake users for my demo"
- "Create sample contacts for testing"
- "Fill this table with test profiles"

### 6. Categories per marketplace
| Marketplace | Category / tags |
|---|---|
| Microsoft (Copilot Studio certified connector / AppSource) | Developer tools; IT & Management |
| Salesforce AgentExchange | Platform / Developer |
| AWS Marketplace: AI Agents & Tools | Developer tools → Testing |
| Google Cloud Marketplace / Gemini Agent Gallery | Developer tools |
| HubSpot Marketplace (Breeze) | Operations |

### 7. Developer try-it kit (ship with the listing)
- Free tier that works in under 10 minutes, no sales call
- Sample JSON payload shaped like a real export from: none needed
- One copy-paste example call **and** its response
- A 60–90 second demo video: prompt → tool call → answer
- Link to the docs and the MCP/OpenAPI schema; privacy policy and terms

### 8. Champion kit (for the developer to forward upward)
- **One-line business case:** No real personal data in test environments. Always as a range, never a bare number.
- **Security note:** stateless, nothing stored, per-user OAuth, DPDP/GDPR-friendly
- **Price:** the tier table from §Monetization, with the free tier stated first

### 9. Don'ts
- Coined names (e.g. "Synthetic Data Mint") in titles or tags
- "Best", "#1" or "guaranteed" claims
- Bare money figures
- Promises beyond the guardrail: Synthetic only; never real people
