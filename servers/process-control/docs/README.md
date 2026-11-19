# 20 · Process Monitor — Statistical Control Charts MCP
**Score: 77/100 · Phase: NEAR (most general — any KPI stream)**
**Classical model: Shewhart Control Charts (Walter Shewhart, 1924) · Western Electric Rules (1956) · CUSUM/EWMA (1954/59)**

## Problem statement
"Is this metric drifting abnormally, or is it just noise?" Every ops team watches KPIs — support response time, error rates, delivery SLAs, defect rates, sales-cycle length, conversion — and reacts to random blips while missing real drift, or vice versa. Statistical Process Control answered this in 1924 (built for factories, now applied to *any* metric stream) with rigorous rules separating signal from noise. But SPC lives in quality-management software and Six-Sigma consulting; most teams eyeball dashboards. An LLM can chart once; it cannot continuously monitor, apply run-detection rules, and alert only on statistically real shifts.

## Solution
Continuous statistical monitoring of any metric stream, alerting only on genuine out-of-control signals.

**Tool set (v1):**
- `monitor_metric(series, chart_type)` → in-control / out-of-control verdict + which rule fired
- `set_control_limits(baseline_data)` → compute ±3σ limits from a stable baseline
- `watch_process(metric_source)` → continuous; alert on rule violations (the core product)
- `detect_shift(series)` → CUSUM/EWMA for small sustained drifts control charts miss
- `explain_signal(timestamp)` → which rule, how many sigma, what it means

## THE ALGORITHMS (implementation guide)
### Shewhart charts (1924)
X̄-R (means/ranges), X-mR (individuals), p/np (proportions), c/u (counts). Center line = mean; control limits = mean ± 3σ (σ estimated appropriately per chart type). A point outside limits = out of control.
### Western Electric / Nelson rules (1956/84)
Run-pattern detection beyond single points: 2-of-3 beyond 2σ, 8 consecutive one side of center, 6 trending, etc. **Encoding these rules correctly is the value** — a naive LLM applies 3σ only and misses drift.
### CUSUM (1954) & EWMA (1959)
Cumulative-sum and exponentially-weighted charts detect *small sustained shifts* fast (Shewhart is slow on these). Premium tier. Reference: SPC texts, NIST/SEMATECH handbook (public); OSS implementations to harden.

## Pressure test
G1 (26/30): interpreter charts once ⚠️; continuous monitoring + full rule-set + alerting is fundamentally continuity — un-replicable in-session. Native ✅ mostly safe. G2 (19/25): moats 3 (continuous watch — this IS the product) + 5 (rule-cited signals) + mild 4 (baseline history). G3 (20/25): extremely general (any metric), self-serve, feeds into every ops agent. G4 (13/20). **Total 77.**

## Audience & user profiles
Ops, quality, support, manufacturing, SaaS reliability, sales-ops — anyone with a KPI they must keep in range.
1. **Support ops lead** — agent alerts only when response-time genuinely degrades (rule-fired), not on daily noise.
2. **Manufacturing QA** — classic defect-rate control charting.
3. **SaaS ops** — error-rate/latency monitoring with statistical rigor, not arbitrary thresholds.

## Where/how to get data & components
| Need | Source | Access |
|---|---|---|
| Metric time-series | CSV/upload, metric APIs, DB queries, Dataverse | none for CSV |
| Math | `numpy`/`scipy` + encoded WE/Nelson rules (OSS refs) | we build + test vs. NIST examples |
| Watchers | TECH-00 §4 | internal |

## Monetization
- **Free**: 3 monitored metrics, manual charting
- **Pro ₹4,100/$49 mo**: 25 metrics, continuous watchers, WE/Nelson rules, alerts
- **Business ₹12,400/$149 mo**: 250 metrics, CUSUM/EWMA, custom baselines, data-source pulls
- Positioning: "statistical rigor on your KPIs — real signals, not dashboard anxiety."

## India sole-prop notes
Metric data typically non-personal (light DPDP). Highly horizontal — works for any industry, easing the "which vertical" question. Export invoicing under LUT.

## Risks
Baseline quality (guided baseline-period selection); alert fatigue if limits mis-set (conservative defaults + tuning); rule-encoding correctness (validate against NIST/SEMATECH published examples).

---

---

## LISTING & DISCOVERABILITY
*Keep this section current. It drives marketplace search, AI tool selection and the buying decision.*

**Who discovers this MCP:** agent developers, developers and agent builders (Copilot Studio makers, Agentforce, Breeze and Gemini Enterprise builders, Claude/ChatGPT connector users). They find it, test it, and **champion the purchase** to the budget owner. Every field below is written for them first, with a business tagline the manager can understand.

### 1. Names
| Field | Use this | Rule |
|---|---|---|
| **Listing title** | **Statistical Process Control & Anomaly Alerts — Saifs AI** | Capability keyword first; ≤ 60 characters before "— Saifs AI" |
| **Tagline** (for the budget owner) | Catch process drift and quality problems before they become defects | Plain business words; ≤ 90 characters |
| **Server / connector ID** | `saifs-process-control` | Lowercase, hyphenated, no coined words |
| **Internal code name** | Process Monitor (doc 20) | Docs and code only, **never** in a listing |

### 2. Description: the first 160 characters decide the click
1. **Developer contract:** Measurements over time (JSON) → control charts, out-of-control alerts, capability (Cpk).
2. **Business value (for the champion):** Scrap and rework cut 10–25%.
3. **Methods (specialist search and credibility):** uses Shewhart, Western Electric, CUSUM, EWMA.
4. **Trust:** stateless; data is processed in the request and never stored; per-user OAuth sign-in. Decision support; operators own stop/go.

### 3. Keywords
- **Developer search terms (title, description, tags):** statistical process control, spc, control chart, cusum, anomaly detection, process capability, cpk, quality monitoring
- **Business phrases (tagline, FAQ, tool descriptions):** "is the process drifting", "quality alerts"
- **Method terms (description body):** Shewhart, Western Electric, CUSUM, EWMA
- **Platform terms (only where actually listed):** MCP server, Copilot Studio connector, Agentforce action, Breeze agent tool, Gemini Enterprise, Amazon Bedrock AgentCore, Claude connector, ChatGPT app

### 4. Tools as shown in the listing (alphabetical)
`detect_shift` · `explain_signal` · `monitor_metric` · `set_control_limits` · `watch_process`

**Tool-description rule:** the AI picks tools from these descriptions, so they count as discoverability too.
- Start with a verb and the business noun.
- Name the input fields.
- Say what comes back.
- Say when **not** to use the tool.

### 5. Example prompts (listing, demo video, tool descriptions)
- "Is line 3 drifting out of spec?"
- "Alert me when defect rate shifts"
- "Calculate Cpk for this process"

### 6. Categories per marketplace
| Marketplace | Category / tags |
|---|---|
| Microsoft (Copilot Studio certified connector / AppSource) | Operations & Supply Chain; Manufacturing |
| Salesforce AgentExchange | Manufacturing / Operations |
| AWS Marketplace: AI Agents & Tools | Industry → Manufacturing |
| Google Cloud Marketplace / Gemini Agent Gallery | Manufacturing |
| HubSpot Marketplace (Breeze) | Operations |

### 7. Developer try-it kit (ship with the listing)
- Free tier that works in under 10 minutes, no sales call
- Sample JSON payload shaped like a real export from: MES, SCADA exports, QC logs
- One copy-paste example call **and** its response
- A 60–90 second demo video: prompt → tool call → answer
- Link to the docs and the MCP/OpenAPI schema; privacy policy and terms

### 8. Champion kit (for the developer to forward upward)
- **One-line business case:** Scrap and rework cut 10–25%. Always as a range, never a bare number.
- **Security note:** stateless, nothing stored, per-user OAuth, DPDP/GDPR-friendly
- **Price:** the tier table from §Monetization, with the free tier stated first

### 9. Don'ts
- Coined names (e.g. "Process Monitor") in titles or tags
- "Best", "#1" or "guaranteed" claims
- Bare money figures
- Promises beyond the guardrail: Decision support; operators own stop/go
