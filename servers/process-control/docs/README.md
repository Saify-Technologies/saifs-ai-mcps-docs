# Statistical Process Control & Anomaly Alerts - Saifs AI

**Statistical Process Control & Anomaly Alerts - Saifs AI** helps operations and quality teams decide **whether a metric is really changing, or is just noise**. You send measurements over time. It returns a control chart, limits from a stable baseline, the rule that fired, and process capability when you supply a spec or SLA.

It is built for support operations, manufacturing quality, SaaS reliability, sales operations, logistics, and any team that must keep a KPI in range.

**In plain language:** you give it a metric over time. It tells you if the process is in control, which rule fired, and whether the process can meet a spec.

**For technical readers:** the calculations use Shewhart control charts, Western Electric or Nelson rules, CUSUM, EWMA, and process capability (Cp, Cpk).

**Publisher:** Saifs AI
**Website:** [https://saifs.ai](https://saifs.ai)
**Learn more:** [https://saifs.ai/statistical-process-control](https://saifs.ai/statistical-process-control)
**Pricing:** [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
**Contact and help:** [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
**Privacy:** [https://saifs.ai/privacy_policy/statistical-process-control](https://saifs.ai/privacy_policy/statistical-process-control)
**Terms:** [https://saifs.ai/eula](https://saifs.ai/eula)
**Sign-in:** Microsoft single sign-on (OAuth 2.0). No API keys.
**MCP endpoint:** [https://process-monitor-mcp-microsoft.saifs.ai/mcp](https://process-monitor-mcp-microsoft.saifs.ai/mcp)

| If you are | Start with |
| --- | --- |
| A business owner, operations or quality lead | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [Where the data comes from](#where-the-data-comes-from) |
| An administrator or security reviewer | [Data handling and security](#data-handling-and-security), [Security and compliance](#security-and-compliance), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [Where the data comes from](#where-the-data-comes-from), [For developers](#for-developers) |
| A quality reviewer checking the method | [Inside the calculation](#inside-the-calculation) |

## The problem it solves

Most teams still judge a dashboard by eye. A spike gets a reaction. A slow drift gets missed. A fixed threshold is wrong for half the year, because ordinary variation is not a fault. Statistical process control has standard rules for this question, and most teams never apply them to every KPI. The same check is what operators ask for when they search for quality monitoring, and what makers ask for when they search for anomaly detection on a time series.

Statistical Process Control makes that check available to any AI agent:

- **Is this change real, or just noise:** a control chart, with an in-control or out-of-control verdict.
- **Which pattern fired:** Western Electric run rules by default, or Nelson rules when you ask. A single point past 3 sigma is only the basic rule.
- **Is a small shift building:** CUSUM and EWMA on the same series, for drifts a control chart is slow to show.
- **Can the process meet the spec:** process capability (Cp, Cpk) when you supply a specification or SLA.
- **One problem, one incident:** nearby signals are grouped, so a long shift is one incident rather than a pile of points.

## Why choose it: the same answer every time

Statistical process control here is deterministic. The same rows and the same settings produce the same limits, the same signals, and the same process capability. The verdict is calculated from fixed rules. It is not written afresh by a language model each time you ask.

That matters because operators and quality leads act on the verdict. They need a result they can rerun and compare week to week. The agent's wording around the numbers may vary. The numbers do not.

| What it does | Established method |
| --- | --- |
| Judges whether points sit inside normal variation | Shewhart control chart, with limits from a stable baseline |
| Finds runs, trends and clusters, not only single points | Western Electric rules, or Nelson rules when you select them |
| Finds a small sustained shift | CUSUM and EWMA |
| Measures whether the process can meet a spec or SLA | Process capability (Cp and Cpk) |
| Checks new points against a baseline you already accepted | Stored control limits from an earlier run |

**It does not guess.** When successive points depend on each other, the tool says CHECK DATA and reduces the rules, instead of calling the process out of control. Process capability is calculated only when you supply a spec or SLA. An in-control result means do not treat the movement as a change.

**It recommends and does not act.** It never stops a line, never rolls back a release, never opens a ticket, and never writes to a system of record. Operators own the stop or go decision.

## Who should use this

| Role | How it helps |
| --- | --- |
| **Support operations leads** | Checks whether response time has genuinely degraded, instead of reacting to the usual daily swing |
| **Manufacturing quality and production** | Control charts for defect rates, measurements and counts, with the rule that fired |
| **SaaS reliability and engineering** | Error rate and latency checks with limits from the service's own baseline, and hourly or daily buckets for minute-level data |
| **Sales operations** | The same check on sales-cycle length, conversion, or any other KPI that must stay in range |
| **Logistics and service delivery** | Delivery time and SLA series, with process capability when a target is set |
| **Quality engineers** | Shewhart, Western Electric, Nelson, CUSUM, EWMA and Cpk in one call, with the working returned |
| **Operations managers** | One method for every KPI stream, so teams are not each inventing a threshold |
| **Agent makers and developers** | A tested statistical process control tool for anomaly detection on a metric stream, so nobody has to encode the rules |

**Best fit:** manufacturing, support, SaaS, logistics and sales operations teams that keep at least one KPI in range and can export it as rows over time.

**Not required:** a quality-management module, a statistician, or a data science team. A file of measurements is enough to start.

## Use cases

Each use case below can be built as an agent. The agent types are listed in [Agents you can build](#agents-you-can-build).

**1. Is this measurement drifting**
Scenario: a production or operations lead has a daily measurement and cannot tell a blip from a shift.
Outcome: a control chart, limits from the stable part of the series, and an in-control or out-of-control verdict with the rule that fired.
Ask: "Is line 3 drifting out of spec?"

**2. Defect rate or error rate**
Scenario: the metric is a rate. Defects, errors or misses only make sense against how many chances there were.
Outcome: a p, np, u or c control chart from count and size together, with signals and incidents.
Ask: "Has the defect rate shifted?"

**3. Support response time**
Scenario: response time bounces every day, and the team cannot tell a bad day from a real degradation.
Outcome: a verdict for a higher-is-bad metric, so ordinary swing is not treated as a failure.
Ask: "Is this response-time spike real, or just noise?"

**4. Minute-level server metrics**
Scenario: error rate or latency is exported every one to five minutes, and the points are not independent.
Outcome: the series is rolled up by hour or day before the control chart, which is what the tool expects for that frequency.
Ask: "Are these error-rate points just noise?"

**5. A small shift that never crosses the limit**
Scenario: the average has crept for weeks, and no single point looks extreme.
Outcome: CUSUM and EWMA results for that sustained drift, alongside the Shewhart control chart.
Ask: "Is this process drifting slowly?"

**6. Process capability against a spec**
Scenario: a customer spec or an SLA exists, and the question is whether the process can meet it.
Outcome: Cp and Cpk from the spec. The spec is not used as the control limit.
Ask: "Calculate Cpk for this process."

**7. Keep the same baseline**
Scenario: the team already accepted a baseline, and new days should be judged against it.
Outcome: pass the stored limits back. The tool does not rebuild them and re-flag a shift you already accepted.
Ask: "Check this week's points against last month's limits."

**8. A long metric history**
Scenario: the file has more than about 20,000 rows, or the data must stay in your environment.
Outcome: the same calculation runs inside the agent's own sandbox through the local engine.
Ask: "Run SPC on this full QC export."

## Agents you can build

Each agent is one agent in your agent host with this connector added as a tool. The last column shows what else to add.

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Process check agent | Returns a control chart and an in-control or out-of-control verdict for one or many metrics | A measurement file, or a data source from [Where the data comes from](#where-the-data-comes-from) | 1, 2 |
| Support response agent | Judges response time as higher-is-bad against its own baseline | A ticket or workforce export with a time and a duration | 3 |
| Reliability agent | Rolls minute-level error or latency data up by hour or day, then charts it | A monitoring export, and `subgroup_by` set to hour or day | 4 |
| Small-shift agent | Adds CUSUM and EWMA so a slow drift is visible before a point crosses a limit | The same data source | 5 |
| Capability agent | Returns Cp and Cpk against a spec or SLA | Lower spec, upper spec, or both, in the call or the metric settings | 6 |
| Baseline watch agent | Judges new rows against limits stored from an earlier run | The `limits` from that earlier result | 7 |
| Large-history agent | Runs the local engine on a long export | Code execution on the agent host | 8 |
| System-of-record agent | Reads rows from the system that holds them, then calculates the verdict for a person to review | That system's own MCP server, such as a MES, a data platform, or Dataverse | 1, 2 |
| Scheduled check agent | Repeats the check on a cadence and can forward the incidents | A Power Automate flow with a recurrence trigger, and email or Teams | 1, 7 |
| Quality review agent | Pairs the control-chart verdict with process capability for the same metric | A spec or SLA in the metric settings | 1, 6 |

The tools run on request. Scheduling comes from the flow, not from the connector. Pass stored limits back on later calls, or the baseline is rebuilt.

## At a glance

| Item | Detail |
| --- | --- |
| Tools | `process_monitor` (hosted calculation) and `process_monitor_get_engine` (local engine) |
| Minimum input | Rows with a time and a value, or `count` and `size` together for a rate |
| Hosted tool size | About 20,000 rows, about 500 metrics, and about 500 stored-limit rows per call |
| Metrics charted per call | 200 by default (`max_metrics`). Extra metrics are reported as skipped. |
| Larger files | Local engine, runs in your agent's code sandbox, needs only `numpy` |
| Writes to your systems | None |
| Local result file | `process_check.json` |

## Where the data comes from

Statistical Process Control does not connect to your systems. The agent brings the data to it, from any source that can supply a metric over time.

| Data source | How the agent gets the data | Best route |
| --- | --- | --- |
| Spreadsheet or CSV export from a MES, SCADA, a QC log, a support desk, or a monitoring tool | The user uploads the file to the agent | Local engine for large files, hosted tool for smaller ones |
| A system that has its own MCP server | The agent reads the rows with that server, then passes them on | Hosted tool up to about 20,000 rows. Above that, export a file and use the local engine. |
| Dataverse tables | The Dataverse MCP server or the Dataverse connector | Same as above |
| SQL database, Excel workbook online, or SharePoint list | A Power Platform connector on the same agent or flow | Same as above |
| A scheduled flow | A Power Automate flow reads the source on a schedule and calls this connector | Hosted tool, with stored limits passed back |
| An earlier run of this connector | Pass `limits` from that result with the new rows | Either route |

The connector needs only rows. Any other tool that returns a time and a measurement can feed it the same way. These systems are examples of where the rows live. The connector is not certified against each one.

**Works alongside your system of record.** That system's MCP server gives the agent the records. This connector adds the statistical process control calculation. No quality module has to be configured. The system supplies the rows, this connector returns the verdict, and a person decides what to do.

**What to pull from the source**

| Data in your system | Field it fills | Why it matters |
| --- | --- | --- |
| Metric name | `metric` | Keeps each KPI separate. Omit it on a wide file that already has one column per metric. |
| When it was recorded | `timestamp` | Orders the series. A date or a full datetime both work. |
| The measurement | `value` | The number on an individuals or subgroup chart |
| Events and chances | `count` and `size` | Required together for a rate: defects and units, errors and requests, misses and totals |
| Subgroup or batch | `sample` | Ties several readings to one sample for an X-bar chart |
| Which way is bad | `direction` | Higher is bad for latency, errors and defects. Lower is bad for conversion, uptime and yield. |
| Spec or SLA | `lsl`, `usl` | Unlocks process capability. Not used as the control limits. |
| End of the normal period | `baseline_end` | Limits come from the dates up to here. Later points are the ones being checked. |

**Column names can stay as they are.** The hosted tool accepts the aliases below. You do not need to rename headers before the call.

| Field | Headers recognised (examples) |
| --- | --- |
| `metric` | kpi, metric_name, series, name, sensor, measure, characteristic, tag |
| `timestamp` | time, date, datetime, period, day, week, month, ts, created_at, recorded_at |
| `value` | measurement, reading, metric_value, observation, result, y, avg, average, mean |
| `count` | defects, errors, nonconforming, defectives, failures, failed, incidents, complaints, misses, breaches, rejects |
| `size` | n, sample_size, total, requests, units, inspected, opportunities, orders, tickets, calls |
| `sample` | subgroup, batch, lot, sample_id, subgroup_id, shift |

A wide export also works. One time column, plus one numeric column per metric.

**File formats for the local engine.** CSV, TSV or JSON. The default path is `metrics.csv` when you do not pass one. Optional files hold per-metric settings and stored limits. Save an Excel sheet as CSV first, or ask the agent to convert it in the sandbox.

## How it works

The connector offers two routes to the same calculation. The agent picks the route by the size of the data.

| | Hosted tool | Local engine |
| --- | --- | --- |
| Tool name | `process_monitor` | `process_monitor_get_engine` |
| Use when | Up to about 20,000 rows, 500 metrics and 500 stored-limit rows | Anything larger, or when data must stay in your environment |
| What the agent sends | The series, and any settings | File paths and settings only |
| What comes back | The full result | A ready-to-run Python script |
| Where the calculation runs | Saifs AI endpoint | Your agent's code-execution sandbox |
| Where your metric data goes | To the Saifs AI endpoint for calculation | Nowhere. It stays in the sandbox. |
| Requires | Nothing extra | An agent host with code execution, and `numpy` |

**Local engine, step by step**

1. The user uploads or points the agent to a metric file.
2. The agent calls `process_monitor_get_engine` with the file path and any settings.
3. The connector returns the calculation script.
4. The agent saves that script exactly as given and runs it in its sandbox against the file.
5. The script prints decision cards and one status line per metric, and writes the full detail to `process_check.json`.
6. The agent reports those results to the user.

Do not shorten, retype or rewrite the script. Do not replace it with other code. If the agent cannot run code, say so. Do not judge the series by eye instead.

### Inside the connector

1. The user asks whether a metric is really changing, or is just noise.
2. The agent chooses a tool and calls the connector, which passes the call to the Saifs AI MCP server over HTTPS at `https://process-monitor-mcp-microsoft.saifs.ai/mcp`.
3. The server checks the signed-in account.
4. The hosted tool runs the engine on the rows and returns the result. The engine tool returns the same engine as a script, for the agent to run in its sandbox.
5. The agent reads the result and explains it in plain language. The numbers come from the engine. The wording comes from the agent.

### Inside the calculation

Both routes run the same engine. The steps below are the ones the live tool describes, with the defaults from its schema.

| Step | What the engine does | Method |
| --- | --- | --- |
| 1. Read the rows | Takes long rows (metric, time, value, or count and size, or a sample id) or a wide sheet with one number column per metric. | The column aliases in the schema |
| 2. Choose the chart | `auto` picks I-MR, X-bar R, X-bar S, X-bar with individuals, p, np, u or c from the columns you sent. | Chart rules. Leave `auto` unless the user named a chart. |
| 3. Adjust a rate chart when needed | For a large, overdispersed p or u chart, applies a Laney adjustment so the limits are not too tight. | On when `laney` is `auto`, overdispersion is above 1.2, and average sample size is at least 500 |
| 4. Set the baseline | Uses `baseline_end` when you name the last normal date. Otherwise the leading portion of the series, the first half, kept between 20 and 250 points. | Limits from that stable period. Later points are the ones being checked. |
| 5. Clean the baseline | Sets aside baseline points that are themselves outside the limits, so one bad day does not widen them. | `clean_baseline`, default true |
| 6. Estimate sigma | `auto` uses the moving range. `median_mr` resists outliers. `overall` uses the ordinary standard deviation. | Moving range, median moving range, or standard deviation |
| 7. Apply run rules | `western_electric` is the default. `nelson` adds the Nelson rules. `basic` is the 3-sigma rule only. | Western Electric, Nelson, or 3-sigma |
| 8. Check dependence | If successive points correlate above 0.5, the rules are reduced and the result says CHECK DATA. | Autocorrelation threshold, default 0.5 |
| 9. Look for a small shift | Runs CUSUM and EWMA on the series. | CUSUM slack 0.5 sigma and decision limit 5 sigma. EWMA weight 0.2 and limit width 3, with the exact limit that widens over the first points. |
| 10. Group incidents | Signals this close together are one incident. | `incident_gap`, default 5 points |
| 11. Capability | Returns Cp and Cpk when a lower spec, an upper spec, or both are present. Specs do not move the control limits. | Process capability against the spec or SLA |
| 12. State the current point | Calls out the latest points as the current state, and caps how many signal points are listed. Incidents stay complete. | `recent_points` default 3. `max_points_listed` default 50. |

**Why the answer does not change.** Each step is a fixed rule or formula. The server describes no random step. The same rows and the same settings return the same numbers. The agent's wording around those numbers may vary.

**What happens when data is thin or dependent.** There is no separate spec, so there is no process capability figure, and the control chart still runs. A rate without `size` is not treated as a rate. Send count and size together. When successive points correlate above 0.5, the tool does not press an out-of-control call. It says CHECK DATA and reduces the rules. For 1 to 5 minute server data, set `subgroup_by` to `hour` or `day` instead of ignoring that warning. If you omit stored limits, the tool builds a new baseline from this file, and a shift you already accepted can appear again.

**Where the working is shown.** The result carries the limits, the signals, the incidents, the CUSUM and EWMA figures, process capability when a spec was sent, and `limits_to_store` for the next run. The local engine prints decision cards and one status line per metric, and writes that detail to `process_check.json`.

<!-- TODO Saifs AI: capture one signed-in response and name the exact JSON fields for limits, signals, incidents, capability and limits_to_store. The endpoint returned 401 without a token, so those field names are not invented here. -->

## Data handling and security

| Question | Local engine | Hosted tool |
| --- | --- | --- |
| Does metric data reach Saifs AI? | No. The tool returns a script. The rows stay in the file you point at. | Yes. The rows passed in the call are sent for calculation. |
| Where is it processed? | Your agent host's code sandbox | `process-monitor-mcp-microsoft.saifs.ai`, over HTTPS |
| What happens to the rows after the call? | They never left your sandbox | They are processed in the request and are not stored as your metric history. |
| What does Saifs AI record? | Only that the engine script was requested | The account used to sign in, for access and billing. Not the metric rows. |

<!-- TODO Saifs AI: confirm the hosted-tool retention line for this server, including whether call time, tool name, or a hash of the request is logged. The sentence above matches the public product statement that rows are processed in the request and not stored. It does not copy Inventory Intelligence's audit-hash wording, because that detail is not confirmed here. -->

**What "sandbox" means.** The sandbox is the code-execution environment of your own agent host, such as Copilot Studio or Azure AI Foundry. It belongs to your tenant, not to Saifs AI, and your host's data policies continue to apply to it.

**Use the local engine only.** If your policy requires that metric data never leaves your environment, turn off the hosted tool. In Copilot Studio, open the agent's **Tools** tab, select the Statistical Process Control server, turn off **Allow all**, and switch off `process_monitor`. Leave `process_monitor_get_engine` on. The agent can then use only the local engine, and any tool added to the server later stays off by default. This setting is per agent.

**Sign-in.** Users sign in with Microsoft single sign-on. Saifs AI holds only the account details needed for sign-in and billing. No API key is created, shared or stored.

**No write access.** The connector has no action that creates, changes or deletes records in a MES, a SCADA system, Dataverse, a support desk, or any other system.

**The script is open to review.** The local engine script is plain Python with a single dependency, `numpy`. Your security team can read it before allowing it to run. Run it as it was returned.

## Security and compliance

This section separates what an independent party has reviewed from what Saifs AI states about itself.

**Reviewed by Microsoft**

| Item | What it covers |
| --- | --- |
| Microsoft connector certification | Microsoft reviews and tests this connector before listing it as a certified connector. The review covers the connector's function and packaging, not Saifs AI's internal systems. |

**Self-assessed by Saifs AI**

Self-assessments are statements Saifs AI makes about its own practices. They are published so that you can check them, and they are not independent audits.

<!-- PUBLISH EACH ROW ONLY AFTER IT IS COMPLETED. Until then, keep the row inside this comment. Add the public link when you have it.

| Self-assessment | What it is | Where to read it |
| --- | --- | --- |
| Microsoft Publisher Attestation | Saifs AI's answers to Microsoft's questionnaire on security, data handling and compliance. Microsoft publishes the answers and does not verify them. Renewed every year. | [link to the Microsoft app compliance page for this offer] |
| CSA STAR Level 1 | Saifs AI's completed Cloud Security Alliance questionnaire (CAIQ), published on the public STAR Registry. Updated every year. | [link to the STAR Registry entry] |
| CSA STAR for AI Level 1 | Saifs AI's completed AI questionnaire (AI-CAIQ) on the same registry. | [link to the STAR Registry entry] |

-->

| Design fact | Detail |
| --- | --- |
| Customer metric rows stored | None on the hosted tool, which processes them in the request. The local engine keeps rows in your sandbox. |
| Passwords held | None. Sign-in is Microsoft single sign-on. |
| Write access to your systems | None |
| Local engine | Plain Python, one dependency (`numpy`), returned as a script you can read |

**Independent audits.** Saifs AI does not yet hold an independently audited certificate such as SOC 2 or ISO 27001. If your review needs a completed security questionnaire, request it through the [contact page](https://saifs.ai/contact-us).

## Token usage and cost

With the hosted tool, the model has to send every row, so token use grows with the file. With the local engine, the rows stay in the sandbox. The model handles the script and the printed summary, not the file. That is why a long history does not grow the model's row traffic.

The figures below count only the tokens used to send the rows to the hosted tool. They do not include the tool's reply, and they do not include the local engine script. Those two counts have not been measured on this server, so this page states no run cost and no savings multiplier.

| Metric file | Hosted tool: tokens to send the rows | Local engine |
| --- | --- | --- |
| 30 daily points, one metric | about 680 tokens | Not measured. The script has not been captured from a signed-in call. |
| 20,000 rows, the hosted limit | about 440,000 tokens | Not measured |
| 60,000 rows | Not accepted by the hosted tool. The rows alone are about 1,320,000 tokens. | The local engine is the route. Script tokens are not measured. |

**Estimate the rows you would send.** A row with a metric name, a date and a value is about 22 tokens in the request JSON. Thirty rows are about 680 tokens. Twenty thousand rows are about 440,000 tokens.

**Which route to choose, on file size alone**

- **A short series:** the hosted tool. It is one call, and the row payload is small.
- **More than about 20,000 rows, 500 metrics, or 500 stored-limit rows:** the local engine. It is the only route.
- **The file must not leave your environment:** the local engine, with the hosted tool switched off.
- **A repeated check in one session:** the local engine keeps the script in the sandbox. The token saving is not stated here, because the script size is not measured.

**Hosts that bill per action.** Some agent hosts charge a fixed amount for each tool call and do not bill by token. On those hosts the token counts above do not change the bill. Choose the local engine for file size and for where the rows are processed.

How the row counts were measured: the `o200k_base` tokenizer, on generated JSON, three fields per row (`metric`, `timestamp`, `value`), plus `direction`, `rules` and `chart`. Counts vary with column names and with how many metrics you send. Measured on 17 November 2026. Reply tokens and script tokens are excluded on purpose.

<!-- TODO Saifs AI: call process_monitor_get_engine, count the script with the same tokenizer, call process_monitor on the 30-row sample, count the reply, then add the cost table at $1.25 per million input tokens and $10 per million output tokens (November 2026 reference). Do not publish a dollar figure before those two counts exist. -->

## Prerequisites

- A Saifs AI account, created by signing in with Microsoft single sign-on
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Access to Copilot Studio, Microsoft 365 Copilot, Azure AI Foundry, Power Automate, Power Apps or Azure Logic Apps
- A metric history with a time and a value, or count and size for a rate
- For the local engine: code execution enabled on the agent host

## How to get credentials

No keys or secrets are needed. When you create a connection, you are redirected to sign in with your Microsoft account and approve access. If the connection later expires, sign in again from the connection page.

## Get started with your connector

1. In Copilot Studio, open your agent and go to **Tools**, then **Add a tool**.
2. Search for **Statistical Process Control** and select it.
3. Create a connection and sign in.
4. Add this to the agent's instructions: "For any question about drifting, out of control, a spike, a control chart, SPC, Shewhart, run rules, CUSUM, EWMA, control limits, process capability, or whether a change is real, call the Statistical Process Control tool. Do not judge by eye and do not compute limits, sigma, Cp or Cpk yourself. If the result is in control, do not treat the movement as a change. If the tool returns Not permitted, or a line starting Subscribe here, paste that link in full."
5. Provide the series by pasting it, uploading it, or pointing the agent to a file. For a rate, include count and size. If you have a spec, include it.
6. Ask in plain language, for example "Is line 3 drifting out of spec?" or "Calculate Cpk for this process."

Treat the result as decision support. A person decides whether to stop, adjust or carry on.

## For administrators

Enabling this connector gives the organisation one approved way to decide whether a metric is really changing, or is just noise, inside the agents your makers already build.

**Why this is an administrator decision.** Without a calculation tool, makers ask an agent to judge a spreadsheet by eye. A language model can give a different reading on each run, and the result cannot be compared week to week. A governed connector replaces that with one statistical process control method for every agent and team.

- **You choose who can connect it.** Tenant and environment permissions apply. Only users who can create a connection can call the tools.
- **You keep your system of record.** The connector recommends and explains. It does not stop a process or write back to any system.
- **You can check the number.** The same file and settings can be rerun. Missing specs and dependent points are reported, not assumed.
- **You control where data goes.** Turn off the hosted tool per agent to keep metric data in your environment.
- **You set data policy as usual.** Place the connector in the Business, Non-business or Blocked group with Power Platform data loss prevention policies.
- **There are no secrets to rotate.** Expired connections are renewed by signing in again.

**Review checklist**

- Decide whether the hosted tool is allowed, or local engine only
- Assign the connector to the correct data policy group
- Review the privacy policy and terms
- Read the self-assessments listed under Security and compliance, once they are published
- Review the local engine script if your policy requires it

## For developers

Send the columns the business already has. A time and a value are enough for an individuals chart. A rate needs `count` and `size` together. A spec is needed only for process capability.

### Series fields

| Field | Meaning | Needed for |
| --- | --- | --- |
| `metric` | Which KPI this row belongs to. Omit it on a wide file. | Separating metrics |
| `timestamp` | When the row was recorded. A date or a datetime. | Ordering the series |
| `value` | The measurement. Leave it out when you send `count` and `size`. | Individuals and measurement charts |
| `count` | Events in the period: defects, errors, failures, misses. Zero is allowed. | Rate and count charts |
| `size` | How many chances there were. Must be greater than zero. | Rate charts. Send it with `count`. |
| `sample` | Subgroup id when several readings belong to one sample | X-bar charts |

The `series` array allows up to 20,000 rows. Extra properties on a row are accepted.

### Per-metric settings

Optional. One row per metric, up to 500. A value here overrides the same setting sent for every metric.

| Field | Meaning | Needed for |
| --- | --- | --- |
| `metric` | Must match the series. Required on each settings row. | Applying the row |
| `chart` | `auto`, `imr`, `xbar_r`, `xbar_s`, `xbar_imr`, `p`, `np`, `u` or `c` | Forcing a chart the user named |
| `direction` | `higher_is_bad`, `lower_is_bad` or `both` | Which side of the move matters |
| `lsl`, `usl` | Lower and upper spec or SLA | Process capability only |
| `baseline_end` | Last date of the normal period, `YYYY-MM-DD` | Freezing the baseline on a known date |
| `baseline_points` | How many leading points form the baseline. Minimum 2. | When you do not have a baseline date |
| `subgroup_by` | `none`, `minute`, `hour`, `day`, `week` or `month` | Rolling up high-frequency points |

### Stored limits

Optional. Up to 500 rows, taken from `limits_to_store` on an earlier result. Pass them back with new rows to keep the same baseline.

| Field | Meaning |
| --- | --- |
| `metric` | Required. The metric these limits belong to. |
| `chart` | The chart the limits were built for |
| `center` | Centre line. For p, np, u and c this is the baseline rate. |
| `sigma` | Process sigma stored with the limits |
| `laney_factor` | Laney factor, when an earlier run adjusted a p or u chart |
| `ucl`, `lcl` | Upper and lower control limits, when sigma was not stored |

### Options and defaults

| Option | Default | Purpose |
| --- | --- | --- |
| `chart` | `auto` | Chart for every metric that does not set its own |
| `rules` | `western_electric` | `nelson`, `basic` (3-sigma only), or a list of rule ids |
| `direction` | None | Set it, or set it per metric. Higher is bad for latency, errors and defects. |
| `baseline_points` | First half of the data, between 20 and 250 points | The normal period when `baseline_end` is absent |
| `baseline_end` | None | Last date of the normal period |
| `clean_baseline` | true | Drop baseline points that fall outside the limits |
| `subgroup_by` | `none` | Use `hour` or `day` for 1 to 5 minute server metrics |
| `sigma_method` | `auto` | Moving range. Also `median_mr` or `overall`. |
| `laney` | `auto` | Laney p' or u' when overdispersion is detected |
| `laney_threshold` | 1.2 | Overdispersion above this turns Laney on |
| `laney_min_size` | 500 | Average sample size Laney requires |
| `cusum_k` | 0.5 | CUSUM slack, in sigma. Detects a shift of about 1 sigma. |
| `cusum_h` | 5 | CUSUM decision limit, in sigma |
| `ewma_lambda` | 0.2 | Weight on the latest point. Smaller values catch slower drifts. |
| `ewma_L` | 3 | EWMA control-limit width, in sigma |
| `ewma_limits` | `exact` | The widening early limit. `asymptotic` uses the steady-state limit. |
| `autocorrelation_threshold` | 0.5 | Above this, rules are reduced and the tool says CHECK DATA |
| `lsl`, `usl` | None | Spec or SLA for metrics that do not set their own. Capability only. |
| `recent_points` | 3 | How many latest points are called out as the current state. Range 1 to 1,000. |
| `incident_gap` | 5 | Signals this many points apart, or closer, are one incident. Range 0 to 10,000. |
| `max_points_listed` | 50 | Cap on individual signal points listed per metric. Range 1 to 100,000. |
| `max_metrics` | 200 | Most metrics charted in one call. Range 1 to 5,000. Further metrics are skipped. |

### Sample request

```json
{
  "series": [
    {"metric": "line3_defects", "timestamp": "2026-10-01", "count": 4, "size": 200},
    {"metric": "line3_defects", "timestamp": "2026-10-02", "count": 3, "size": 198},
    {"metric": "line3_defects", "timestamp": "2026-10-03", "count": 5, "size": 210}
  ],
  "direction": "higher_is_bad",
  "usl": 0.03,
  "rules": "western_electric",
  "baseline_points": 20
}
```

A real series needs enough points for the baseline you set. `baseline_points` cannot be larger than the series you send. The three rows above show the shape only.

### Sample response (shortened)

<!-- TODO Saifs AI: replace this note with a shortened response from a signed-in call to process_monitor. Do not invent fields. The call on 17 November 2026 returned 401 Unauthorized. -->

A live response is not printed here, because the endpoint requires sign-in and this page does not invent result fields. What the tool contract says you get back:

- Decision cards, and one status line per metric
- Limits from the baseline, including values to pass back as `limits_to_store`
- Signals, with the run rule that fired, grouped into incidents
- CUSUM and EWMA for a small sustained shift
- Process capability (Cp, Cpk) when a spec or SLA was sent
- CHECK DATA when successive points correlate above the threshold
- When the call is not permitted, `subscribe_url` in `structuredContent`, and a line that starts "Subscribe here:". Paste that Microsoft Marketplace URL in full.

The local engine writes the same detail to `process_check.json`.

### Response sections

| Section | Use it for |
| --- | --- |
| Decision cards | The plain-language status for each metric. An in-control status means do not react to the movement. |
| Limits | Centre, sigma and control limits. Store them and pass them back on the next check. |
| Signals and incidents | Which rule fired, and the grouped incident so one shift is one item |
| CUSUM and EWMA | Whether a small sustained shift is present |
| Capability | Cp and Cpk, only when a spec or SLA was supplied |
| `limits_to_store` | The object to send as `limits` on the next call |
| `process_check.json` | The full local-engine output, written in the sandbox |
| Subscribe line | The exact marketplace URL, when the tool returns Not permitted |

## Known issues and limitations

- **Decision support only.** It does not stop a line, roll back a release, open a ticket, or update any system. Operators own the stop or go decision.
- **On request.** The tools do not watch a stream by themselves. A scheduled flow can call them again. Pass stored limits back, or the baseline is rebuilt.
- **Hosted tool size.** About 20,000 rows, 500 metrics and 500 stored-limit rows per call. Use the local engine for more.
- **Metrics per call.** 200 are charted by default. Raise `max_metrics` or split the call. Extra metrics are skipped, not silently charted.
- **Rates.** Send `count` and `size` together. A count alone is not a rate.
- **High-frequency data.** For 1 to 5 minute server metrics, set `subgroup_by` to `hour` or `day`. Raw points often depend on each other and signal too often.
- **Dependent points.** Correlation above 0.5 returns CHECK DATA and reduced rules.
- **Capability.** Cp and Cpk are returned only when you supply `lsl`, `usl`, or both. Specs are not control limits.
- **Chart choice.** Leave `chart` on `auto` unless the user named a chart.
- **Script.** The local engine script must be run as returned. Do not rewrite it.
- **Regions.** Not available in US Government (GCC, GCC High, DoD) or China (21Vianet) clouds.

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| The tool cannot run, or asks you to sign in again | The connection has expired | Open the connection and sign in again |
| Not permitted, or a line starting "Subscribe here:" | The signed-in account cannot use this server | Paste the full marketplace URL the tool returns. Do not shorten it. |
| CHECK DATA, with fewer rules applied | Successive points correlate above 0.5 | Set `subgroup_by` to `hour` or `day`, especially for 1 to 5 minute data |
| A shift you already accepted appears again | Stored limits were not passed back | Send the earlier `limits_to_store` in `limits` |
| A rate chart is missing or looks empty | `count` was sent without `size` | Send both, for the same rows |
| The agent states a verdict without calling the tool | The agent instructions do not require the tool | Add the instruction in Get started |
| The local engine does not run | Code execution is not enabled | Enable code execution, or use the hosted tool under the size limit |
| Some metrics are skipped | The call exceeded `max_metrics` | Raise `max_metrics` or split the metrics |

## FAQ

**Do I need a statistician or a quality engineer to use this?**
No. A user asks in plain language. The statistical process control rules run inside the tool.

**What is the least data I need?**
A time and a value for each point. For a rate, send count and size together. A spec is needed only if you want process capability.

**Does my data leave my environment?**
Not with the local engine. With the hosted tool, the rows in the call are sent to Saifs AI for calculation. You can turn the hosted tool off.

**Will it change anything in my systems?**
No. It does not write to a MES, SCADA, Dataverse, or any other system. It recommends. Operators own the stop or go decision.

**I already have a MES or a monitoring tool. Why add this?**
That system holds the measurements. This connector adds the control chart, the run rules, CUSUM, EWMA and process capability. It does not replace the system of record.

**What happens if my data is incomplete?**
The control chart still runs. Process capability is omitted when no spec is sent. When points are too dependent, the tool says CHECK DATA instead of forcing an out-of-control call.

**Can I check how a result was produced?**
Yes. The result includes the limits, the rule that fired, the CUSUM and EWMA figures, and process capability when you asked for it. The local engine writes the same detail to `process_check.json`.

**How large a file can it handle?**
The hosted tool takes about 20,000 rows. Larger files use the local engine.

**Can it run on a schedule?**
Yes, by calling it from a scheduled Power Automate flow. Pass the stored limits on each run so the baseline stays put.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment). Microsoft Premium connector licensing applies separately.

## Support

**Statistical Process Control & Anomaly Alerts - Saifs AI** is published by Saifs AI.

- Website: [https://saifs.ai](https://saifs.ai)
- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Learn more: [https://saifs.ai/statistical-process-control](https://saifs.ai/statistical-process-control)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Privacy: [https://saifs.ai/privacy_policy/statistical-process-control](https://saifs.ai/privacy_policy/statistical-process-control)
- Terms: [https://saifs.ai/eula](https://saifs.ai/eula)
- Sign-in: Microsoft single sign-on (OAuth 2.0). No API keys.
- MCP endpoint: [https://process-monitor-mcp-microsoft.saifs.ai/mcp](https://process-monitor-mcp-microsoft.saifs.ai/mcp)
