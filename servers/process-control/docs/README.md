# Statistical Process Control & Anomaly Alerts

[SAIFS AI](https://saifs.ai) (`saifs.ai`) publishes this as a remote MCP server under the registry name `ai.saifs/process-control`. It watches a metric over time and says whether a move is ordinary noise or a statistically real shift.

People search for this as statistical process control (SPC), a control chart, CUSUM, anomaly detection, process capability, or Cpk. The questions are “is the process drifting” and “which quality alerts are real.” The short install page is the [server README](../README.md). This page is what an out-of-control point means, why a dashboard threshold is not a control limit, and how to send a series.

| | |
|---|---|
| **Registry name** | `ai.saifs/process-control` |
| **Endpoint** | `https://process-monitor-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## What it is

Any stream with a time order can be charted: support response time, an error rate, a delivery SLA, a defect rate, the length of a sales cycle, a conversion rate. The server fits a Shewhart chart, applies run rules, and returns an in-control or out-of-control verdict that names the rule. A small drift that never crosses the 3σ line is a separate test, CUSUM or EWMA.

Five tools do that.

| Tool | What you ask |
|---|---|
| `set_control_limits` | What are the center line and the ±3σ limits of a period I believe was stable? |
| `monitor_metric` | Is this series inside those limits, and did a run rule fire? |
| `watch_process` | Given the latest points and the limits I stored, is there an alert right now? |
| `detect_shift` | Is there a small shift that has lasted, even though no point has left the 3σ lines? |
| `explain_signal` | Which rule, how many sigma, and what that pattern usually means? |

The call is stateless. You send the series and the limits. You keep the baseline. The server does not poll your database and does not store the metric. “Continuous” means you send the new points on your own schedule. An alert comes back only when a rule fired. A quiet window comes back in control.

The verdict is decision support. The operator decides whether to stop, adjust, or keep running. An alert is not itself a stop.

Plants that act on these signals rather than on every blip cut scrap and rework on the order of 10–25%. That is a range across processes, not a promise for one line. The same charts, on a response time or an error rate, are what keep a team from paging itself for ordinary noise.

A change between two file loads, rather than a chart of one running metric, is the drift report on [Data Cleaning & Validation](../../data-cleaning/README.md).

## Why eyeballing the dashboard pages the wrong days

Walter Shewhart’s control chart (1924) draws a center line at the process mean and limits at about three standard deviations, with the standard deviation estimated the way that chart requires. A point outside those limits is rare if the process is stable. It is a signal. A point inside them is the noise the process always makes.

Most dashboards use a round threshold instead. The team chases Tuesday because it looked high, and misses a slow drift that never quite crosses the line. The Western Electric rules (1956), and Nelson’s later statement of run rules (1984), catch the patterns a single 3σ test misses: a run on one side of the center, a trend, a cluster beyond 2σ. Encoding those rules, and citing which one fired, is the difference between a chart and an alarm.

A language model can sketch the limits once. It will not keep a baseline separate from the points under investigation, apply the run rules the same way on the next window, and stay quiet when nothing fired.

## How to use it

Connect the endpoint in the [install README](../README.md) and sign in when the client asks. Export the metric from a quality log, a support tool, or a monitoring extract. Mark a baseline period that you believe was stable, before the week you are worried about. Limits fit on a period that already contains the shift will hide the shift.

```json
{
  "metric": "defect_rate",
  "chart_type": "p",
  "baseline": [
    {"t": "2026-10-06", "value": 0.021, "n": 480},
    {"t": "2026-10-07", "value": 0.018, "n": 510}
  ],
  "series": [
    {"t": "2026-11-16", "value": 0.044, "n": 500},
    {"t": "2026-11-17", "value": 0.047, "n": 490}
  ],
  "spec": {"lsl": 0.0, "usl": 0.03}
}
```

`chart_type` is one of:

| Type | Use it for |
|---|---|
| `xbar_r` | Subgroup means and ranges, when you measure several units at each time |
| `x_mr` | Individuals and a moving range, when you have one number per time |
| `p` or `np` | A proportion or a count of defectives, with the sample size `n` |
| `c` or `u` | A count of defects, or defects per unit |

Sigma is estimated from that chart. A p-chart uses the binomial variance at the baseline proportion and the sample size. An individuals chart uses the moving range. Do not paste a standard deviation that was computed on the series after the special-cause points were already mixed in.

`set_control_limits` returns the center line and the upper and lower control limits. Store them. `monitor_metric` and `watch_process` use those limits plus the new points. `explain_signal` takes the timestamp that went out of control.

When you send specification limits (`lsl`, `usl`), the same reply can include process capability, Cpk: how the stable process sits inside the spec, which is a different pair of lines from the control limits. A process can be in control and still incapable, because it is stable and too wide for the spec. It can be out of control and still mostly inside the spec. Cpk does not replace the control verdict, and a control limit is not a specification.

### The rules that fire

Shewhart’s own rule is one point outside the 3σ control limits.

The run rules on top of that, which `explain_signal` cites by name, include:

- Two of three consecutive points beyond 2σ on the same side of the center.
- Eight consecutive points on one side of the center line.
- Six points in a row trending up, or six trending down.

A naive reading stops at the first rule and calls everything else noise. A slow climb of six points, all inside the limits, is a signal under the trend rule. Eight points all above the center, none of them dramatic, is a signal under the run rule. The verdict names which one.

### Small shifts

Shewhart is slow to see a shift of less than about one standard deviation that then stays there. `detect_shift` runs a cumulative sum (CUSUM, 1954) and an exponentially weighted moving average (EWMA, 1959). CUSUM adds up deviations from the baseline target until the sum crosses a decision line. EWMA discounts older points so a new level shows through the noise. Both are the Business-tier reading. They do not replace the Shewhart chart. They answer the question the 3σ chart is bad at.

### Prompts

- “Is line 3 drifting out of spec?”
- “Alert me when defect rate shifts.”
- “Calculate Cpk for this process.”
- “Here are six weeks of baseline and this week’s response times. Did a Western Electric rule fire, or is this ordinary noise?”
- “Which rule fired at 14:10, and how many sigma was that point?”

## Tools

| Tool | What you send | What comes back | Do not use it when |
|---|---|---|---|
| `detect_shift` | A series and a baseline target | A CUSUM or EWMA verdict for a small sustained drift | You already have a point outside 3σ. That is `monitor_metric`. CUSUM is for the shift that never quite leaves the limits |
| `explain_signal` | A timestamp from a verdict | The rule name, the sigma distance, and what the pattern means | Nothing fired. There is no signal to explain |
| `monitor_metric` | A series, a chart type, and limits from a stable baseline | In control or out of control, and which rule fired. Cpk when you also sent specification limits | The baseline you used already contains the excursion you are testing. Refit the limits on an earlier stable window |
| `set_control_limits` | Baseline points and a chart type | Center line and ±3σ control limits, with sigma estimated for that chart | You want an alert. This only computes the lines. Send them back with the new points |
| `watch_process` | The latest points and the limits you stored | An alert if a rule fired on this window, otherwise in control | You expect the server to poll a metric API on its own. Send the window. The server does not keep the stream |

## How to read a result

- **Out of control means the pattern is unlikely under the baseline.** It does not mean the line must stop. The operator owns that decision.
- **In control means no rule fired.** It does not mean the product meets the specification. Read Cpk, if you sent spec limits, for that second question.
- **The rule name is the content of the alert.** “Point beyond 3σ,” “eight above the center,” and “six trending” are different investigations. A single red dot on a dashboard hides which one.
- **A quiet window is a successful result.** The chart’s job is to stay silent when the variation is the variation you already accepted in the baseline.
- **Limits belong to the baseline.** Recompute them when you have deliberately changed the process. Do not recompute them on the week you are investigating, or the limits will move to excuse the shift.
- **CUSUM and EWMA do not cancel a Shewhart signal.** If a point is outside 3σ, that signal stands. The extra charts are for the small shift that Shewhart has not caught yet.

## Data and what this will not do

A metric stream of rates, times, and counts is usually operational data rather than a file of people. The server processes the request and does not store it. Each user signs in with OAuth. If a series does contain personal data, your duties under India’s Digital Personal Data Protection Act, the GDPR, or any other law that applies stay with you.

The server will not stop a line, page a person by itself, or treat a control limit as a customer specification. It will not invent a baseline from a series you have already said is broken. It will not watch a source overnight unless you send the next window.

Privacy policy: https://saifs.ai/privacy_policy

## Plans

| Tier | What it covers |
|---|---|
| **Free** | 3 metrics, charted when you send them |
| **Pro** | ₹4,100 / $49 per month — 25 metrics, a watch each time you send the window, Western Electric and Nelson run rules, and alerts |
| **Business** | ₹12,400 / $149 per month — 250 metrics, CUSUM and EWMA, baselines you choose, and pulls from a data export |

The free tier is enough to set limits on one stable week and test the next week against them, with no sales call. Current terms are on [saifs.ai](https://saifs.ai).

## Support

- Install and client setup: [server README](../README.md)
- Drift between two data loads, rather than a control chart: [Data Cleaning & Validation](../../data-cleaning/README.md)
- Website: https://saifs.ai
- Registry: https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
