<!-- DRAFT for review. Do not replace intro.md until this file is accepted.
FIELD BLOCK (Microsoft validated fields). Confirm before submission.

Title (30 characters): HR Hiring Fairness & Selection
Description (416 characters):
Checks whether a hiring process treats groups fairly, and whether a test or interview is worth its cost. Send group counts by stage, or anonymised rows with stage, group, and outcome. Returns which stage to review, whether a gap could be chance, a like-with-like check, a fair pass mark, and the money value of the method. Group level only. The same inputs return the same numbers. It does not hire or reject anyone.

Operation summary (35 characters): Hiring Fairness and Selection Value
Operation description:
Checks group selection rates by stage and the money value of a test or interview. It does not score or select an individual person.
-->

# HR Hiring Fairness & Selection - Saifs AI

**HR Hiring Fairness & Selection - Saifs AI** helps talent, DEI, and compliance teams decide **whether a hiring process treats groups fairly, and whether a test is worth its cost**. You send group counts by stage, or anonymised rows with only stage, group, and outcome. It returns which stage to review, whether a gap could be chance, a like-with-like check when jobs differ, whether the method predicts success, whether it underrates a group, a fair pass mark, and the money value of the method.

It is built for teams that review hiring from an applicant-tracking export or a spreadsheet, and who need a group-level result they can rerun from one quarter to the next.

| If you are | Start with |
| --- | --- |
| A talent, DEI, or HR lead | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [Where the data comes from](#where-the-data-comes-from) |
| An administrator, counsel, or security reviewer | [Responsible use](#responsible-use), [Data handling and security](#data-handling-and-security), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [For developers](#for-developers) |
| A counsel or audit reviewer checking the method | [Inside the calculation](#inside-the-calculation) |

## The problem it solves

Most teams still judge a funnel by gut feel, or by one company-wide hire rate. A stage can treat one group less favourably while the overall number looks fine. A paid assessment can cost more than the quality it adds. The formulas that check this usually sit with specialists many teams do not have on staff.

HR Hiring Fairness & Selection makes that review available to any AI agent:

- **Which stage to review**: selection rate by group, and an impact ratio against the four-fifths line (default 0.80).
- **Whether the gap could be chance**: a significance check, with a small-count method when groups are thin.
- **Whether the overall number misleads**: the same rates inside each job, department, or location.
- **Whether the method predicts success**: a validity read from anonymous score and outcome pairs, when you send them.
- **Whether the method underrates a group**: the same score checked against later outcomes by voluntary group. Individual scores are not shown back.
- **A fair pass mark**: a cutoff from validity and the selection ratio, with the group effect of that line.
- **Whether the method is worth its cost**: a money value from hires, tenure, the value of performance differences, and cost per applicant.

## Why choose it: the same review every time

HR Hiring Fairness & Selection is deterministic. The same stage counts, and the same anonymous score and outcome pairs when you send them, produce the same impact ratios, the same chance check, the same pass mark, and the same money value. Results are calculated from established selection models. They are not written afresh by a language model each time you ask.

That matters because talent, DEI, and compliance may have to show a number to a leader or to counsel. They need a result they can rerun and compare. The agent's wording around the numbers may vary. The numbers do not.

| What it does | Established method |
| --- | --- |
| Flags a stage where a group's selection rate falls below four-fifths of the highest group's rate | Four-fifths rule (impact ratio, default 0.80) |
| Checks whether that gap could be chance | Significance test at `alpha` (default 0.05), including a small-count method |
| Stops an overall rate from hiding a gap inside one job, department, or location | Stratified comparison, like with like |
| Estimates whether the method predicts later success, and corrects a figure computed only on people already hired | Validity, with range-restriction correction when you supply it |
| Suggests a pass mark and shows the group effect of that line | Cutoff from validity and selection ratio |
| Estimates the money value of a test, interview, or assessment | Selection utility |

**It does not guess.** A group smaller than `min_group_size` (default 30) is not treated as a stable rate. Counts below `min_cell_display` (default 5) are hidden. Do not try to reconstruct them.

**It recommends and does not act.** It analyses groups and processes. It does not hire, reject, advance, or write to an applicant-tracking system or Dataverse. It does not score, rank, rate, or select an individual person. It is not legal advice. Employment law differs by country and state. A four-fifths ratio under 0.80 flags a stage for review. It is not, by itself, a legal finding.

## Who should use this

| Role | How it helps |
| --- | --- |
| **Talent-acquisition leads** | See which stage shows a low impact ratio, and whether that gap could be chance |
| **DEI and fairness officers** | Run the same funnel check each quarter and keep a result that can be rerun |
| **HR compliance and people operations** | Pair the four-fifths screen with a chance check, and compare like with like when roles differ |
| **HR at a growing company** | Ask whether a new assessment is worth its cost, and get a money value |
| **Recruitment agencies** | Check a client's stage rates and a screening test's validity before recommending the process |
| **Hiring managers who own a funnel** | See a fair pass mark and the group effect of that mark. The tool does not rank the people on the slate |
| **Agent makers and developers** | Add a group-level fairness and selection-value check to an agent |

**Best fit:** talent, DEI, compliance, and agency teams that hire regularly and can export stage counts, or anonymised stage outcomes, by voluntary group.

**Not required:** an industrial-organisational psychology practice, or an applicant-tracking suite. Group counts are enough to start.

**Outside this product:** scoring or ranking individual candidates, predicting one person's job performance, termination or layoff ranking, promotion ranking, and productivity surveillance.

## Use cases

Each use case below can be built as an agent. The agent types are listed in [Agents you can build](#agents-you-can-build).

**1. Funnel review before a hiring committee**
Scenario: a talent lead has stage counts for applied, screened, interviewed, offered, and hired.
Outcome: the stages under the four-fifths line, with a chance check, so the committee reviews the stage.
Ask: "Which hiring stage should we review for fairness?"

**2. One stage, one question**
Scenario: leadership asks whether the interview stage treats a voluntary group less favourably.
Outcome: the selection rate, the impact ratio, and whether chance is a weak explanation.
Ask: "Does the interview stage treat groups differently?"

**3. Like with like across jobs**
Scenario: several roles are pooled, and the company-wide rate looks even while one job does not.
Outcome: pass `stratum` (job, department, or location). Rates are compared inside that stratum.
Ask: "Compare selection rates like with like, by job."

**4. Quarterly fairness file**
Scenario: a DEI officer repeats the same export every quarter.
Outcome: the same settings produce the same cards, so quarters can be compared. Small groups are called out.
Ask: "Run this quarter's fairness review on these stage counts."

**5. Is this assessment worth its fee?**
Scenario: a test has a known validity, a cost per applicant, and a role with a salary and a hire volume.
Outcome: a money value for the method, including a comparison with the method it replaces when you send current validity and current cost.
Ask: "Is this assessment worth its cost?"

**6. A fair pass mark**
Scenario: the team wants a cutoff it can explain, and the group effect of that line.
Outcome: a pass mark from validity and the selection ratio, with group impact shown. People are not ranked.
Ask: "Suggest a fair pass mark and show the group effect."

**7. Validity without showing individual scores**
Scenario: a validity study has anonymous score and later-outcome pairs.
Outcome: whether the method predicts success, and whether it underrates a voluntary group. Individual scores are not returned.
Ask: "Does this test predict success, at group level only?"

**8. A large applicant export**
Scenario: more than about 5,000 rows.
Outcome: the same calculation runs in the agent's sandbox. Identifying columns are dropped when the script reads the file.
Ask: "Review this large hiring export at group level."

## Agents you can build

Each agent is one agent in your agent host with this connector added as a tool. The last column shows what else to add.

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Funnel review agent | Answers which stage to review, and whether a gap could be chance | Group counts, or a data source from [Where the data comes from](#where-the-data-comes-from) | 1, 2, 4 |
| Like-with-like agent | Compares rates inside each job, department, or location | A stratum column on the export | 3 |
| Assessment value agent | Estimates whether a test or interview is worth its cost | Validity or score-outcome pairs, salary or value, hires per year, and cost | 5 |
| Pass-mark agent | Suggests a cutoff and shows the group effect. It does not rank people | Validity and selection ratio | 6 |
| Validity study agent | Checks prediction and under-prediction from anonymous pairs | `score_outcomes` with no identifiers | 7 |
| Large-export agent | Runs the local engine on a full export. Identifiers are dropped on read | Code execution on the agent host | 8 |
| ATS review agent | Reads stage outcomes from the applicant system, aggregates to groups, and hands the cards back for a person to review | The applicant system's own MCP server. Strip names before the call if that server returns them | 1, 3 |
| Scheduled quarterly agent | Runs the same review each quarter without anyone asking | A flow with a recurrence trigger, and email or Teams | 4 |

The tools run on request. Scheduling comes from the flow, not from the connector. Do not build an agent that scores, ranks, or selects an individual person.

## At a glance

| Item | Detail |
| --- | --- |
| Tools | `hiring_fairness` (hosted calculation) and `hiring_fairness_get_engine` (local engine) |
| Minimum input | Group counts (`stage`, `group`, `applicants`, `selected`), or anonymised rows with `stage`, `group`, and `selected` |
| Hosted tool size | About 5,000 rows |
| Larger files | Local engine, runs in your agent's code sandbox, needs only `numpy` |
| Writes to your systems | None |
| Unit of analysis | Groups and processes. Not an individual person |
| Pricing | [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment) |

<!-- TODO Saifs AI: confirm this marketplace URL is the pricing page for this offer. -->

## Where the data comes from

HR Hiring Fairness & Selection does not connect to your systems. The agent brings the data to it. Prefer group counts. The connector needs only rows. It has not been certified against each system named below.

| Data source | How the agent gets the data | Best route |
| --- | --- | --- |
| Spreadsheet of stage counts | The user uploads the file | Hosted tool |
| An applicant-tracking export (for example Greenhouse, Lever, Workday) | The agent keeps only stage, group, selected, and stratum. Names, emails, phones, and ids are dropped if present | Hosted tool up to about 5,000 rows. Above that, the local engine |
| An ATS that has its own MCP server | The agent reads outcomes, drops identifiers, and passes group rows | Same size rule |
| Dataverse tables | The Dataverse MCP server or the Dataverse connector, after identifiers are removed | Same size rule |
| A scheduled flow | A flow reads the export on a schedule and calls this connector | Hosted tool |

**Works alongside your applicant system's MCP server.** That server gives the agent the system's records. This connector adds the group-level fairness and value calculation. A person, with their own counsel, decides what to change. This connector does not hire or reject.

**What to pull from the source**

| Data in your system | Field it fills | Why it matters |
| --- | --- | --- |
| Stage counts by voluntary group | `stage_counts`: `stage`, `group`, `applicants`, `selected` | Preferred input. No person-level row is required |
| One row per person, if that is all you have | `candidates`: `stage`, `group`, `selected`, optional `stratum` | Aggregated at once. Send nothing else |
| Job, department, or location | `stratum` | Like-with-like comparison |
| Anonymous score and later outcome | `score_outcomes`: `score`, `outcome`, optional `group` | Validity. Never show an individual's score back |
| Known validity, hire volume, salary, cost | `validity`, `hires_per_year`, `annual_salary`, `cost_per_applicant` | Pass mark and money value |

**Column names are recognised as they are.** Common headers are matched. `stage_name`, `step`, and `round` are read as stage. `gender`, `race`, `ethnicity`, `caste`, `age_band`, and `disability` are read as group. `hired`, `passed`, and `advanced` are read as selected. `job`, `department`, and `location` are read as stratum.

Send voluntary self-identified groups only. Do not infer a group from a name or a photo. Do not send names, emails, phones, candidate ids, addresses, Aadhaar, PAN, SSN, or dates of birth. Those columns are dropped if present.

**File formats for the local engine.** CSV, TSV, and JSON. Default path `hiring.csv`. Set `data_kind` to `candidates`, `stage_counts`, or `score_outcomes`. Identifying columns are dropped on read.

## How it works

The connector offers two routes to the same calculation. The agent picks the route by the size of the data.

| | Hosted tool | Local engine |
| --- | --- | --- |
| Tool name | `hiring_fairness` | `hiring_fairness_get_engine` |
| Use when | About 5,000 rows or fewer | Anything larger, or a file already in the sandbox |
| What the agent sends | The rows and settings | File path and settings only |
| What comes back | The full result | A ready-to-run Python script |
| Where the calculation runs | Saifs AI endpoint | Your agent's code-execution sandbox |
| Where hiring data goes | To the Saifs AI endpoint for calculation | Nowhere. It stays in the sandbox |
| Requires | Nothing extra | An agent host with code execution, and `numpy` |

**Local engine, step by step**

1. The user uploads or points the agent to a hiring file that has already been reduced to group fields.
2. The agent calls `hiring_fairness_get_engine` with the file path and settings in `params`.
3. The connector returns the calculation script.
4. The agent runs the script unchanged in its sandbox. Identifying columns are dropped on read.
5. The script prints the decision cards and writes the group-level result to `hiring_fairness_result.json`.
6. The agent reports the cards. It does not rank people.

### Inside the connector

1. The user asks whether a hiring process treats groups fairly, or whether a test is worth its cost.
2. The agent chooses a tool and calls the connector, which passes the call to the Saifs AI MCP server over HTTPS at `https://hiring-fairness-mcp-microsoft.saifs.ai/mcp`.
3. The server checks the signed-in account.
4. The hosted tool runs the engine on the rows and returns decision cards. The engine tool returns the same engine as a script.
5. The agent explains the cards in plain language. The numbers come from the engine. The wording comes from the agent. The agent must not turn the cards into a ranking of people.

### Inside the calculation

Both routes run the same engine.

| Step | What the engine does | Method |
| --- | --- | --- |
| 1. Prefer group counts | Uses `stage_counts` when sent. If only person rows are sent, it keeps stage, group, selected, and stratum, and drops identifiers | Aggregation to groups |
| 2. Selection rates | Computes the rate at which each group moves forward at each stage | Selected divided by applicants |
| 3. Impact ratio | Compares each group with the highest rate, or with `reference_group` if you set one. A ratio under `four_fifths` (default 0.80) flags the stage for review | Four-fifths rule |
| 4. Chance check | Tests whether the gap could be chance, at `alpha` (default 0.05). Small counts use a small-sample method | Significance test |
| 5. Small groups | A group under `min_group_size` (default 30) is not treated as a stable rate. Counts under `min_cell_display` (default 5) are hidden | Suppression |
| 6. Like with like | Repeats the rates inside each `stratum` when jobs, departments, or locations differ | Stratified comparison |
| 7. Validity | Correlates anonymous scores with later outcomes. `range_restriction_ratio` corrects a correlation computed only on people who were hired | Validity and range-restriction correction |
| 8. Pass mark | Suggests a cutoff from validity and `selection_ratio`, and shows the group effect | Cutoff from validity and selection ratio |
| 9. Money value | Values the method from hires per year, tenure (default 1 year), the value of a performance difference, and cost per applicant. If the value of a performance difference is omitted, 40% of `annual_salary` is used when salary is supplied | Selection utility |

**Why the answer does not change.** Every step is a fixed procedure. The same inputs and settings return the same numbers. The agent's wording may vary.

**What happens when data is missing.** The engine runs the layers it can support. A money value needs hires per year and a value spread (salary or `sd_performance_value`). A pass mark needs validity and a selection ratio. Missing inputs are reported. They are not invented. Currency is a display label, default USD. There is no conversion between currencies. `current_validity` defaults to 0, meaning the method being replaced has no measured validity.

**Where the working is shown.** Each card states the rate, the impact ratio, and whether the group was large enough. Hidden counts stay hidden.

## Data handling and security

| Question | Local engine | Hosted tool |
| --- | --- | --- |
| Does hiring data reach Saifs AI? | No. Identifiers are dropped on read in the sandbox | Yes, the rows passed in the call. Send group fields only |
| Where is it processed? | Your agent host's code sandbox | `hiring-fairness-mcp-microsoft.saifs.ai`, over HTTPS |
| What happens to the data after the call? | It never left your sandbox | <!-- TODO Saifs AI: confirm retention for this server. --> |
| What does Saifs AI record? | Only that the engine script was requested, if your facts sheet confirms that | <!-- TODO Saifs AI: confirm the audit and billing log. --> |

**What "sandbox" means.** The sandbox is the code-execution environment of your own agent host. It belongs to your tenant, not to Saifs AI. Your host's data policies continue to apply.

**Use the local engine only.** If hiring rows must never leave your environment, turn off the hosted tool. In Copilot Studio, open the agent's **Tools** tab, select the HR Hiring Fairness & Selection server, turn off **Allow all**, and switch off `hiring_fairness`. Leave `hiring_fairness_get_engine` on. This setting is per agent.

**Sign-in.** Users sign in with Microsoft single sign-on (OAuth 2.0). No API key is created, shared, or stored.

**No write access.** The connector does not hire, reject, advance, or write to any applicant system or Dataverse.

## Responsible use

This connector analyses groups and processes. It supports a human review. It does not make the hiring decision.

- Do not score, rank, rate, or select an individual person.
- Do not predict one person's performance.
- Do not rank people for termination, layoff, promotion, or surveillance.
- Send voluntary self-identified groups only. Do not infer a group from a name or a photo.
- Send no names, emails, phones, candidate ids, addresses, government ids, or dates of birth.
- A four-fifths flag is a reason to review a stage. It is not a legal finding and not legal advice.
- Hiring records can be sensitive personal data. Use only data you are authorised to process, at group level.

## Security and compliance

This section separates what an independent party has reviewed from what Saifs AI states about itself.

**Reviewed by Microsoft**

| Item | What it covers |
| --- | --- |
| Microsoft connector certification | Microsoft reviewed and tested this connector before listing it as a certified connector. The review covers the connector's function and packaging, not Saifs AI's internal systems. |

**Self-assessed by Saifs AI**

Self-assessments are statements Saifs AI makes about its own practices. They are published so that you can check them, and they are not independent audits.

<!-- PUBLISH EACH ROW ONLY AFTER IT IS COMPLETED. Until then, keep the row inside this comment.

| Self-assessment | What it is | Where to read it |
| --- | --- | --- |
| Microsoft Publisher Attestation | Saifs AI's answers to Microsoft's questionnaire. Microsoft publishes the answers and does not verify them. Renewed every year. | [link] |
| CSA STAR Level 1 | Saifs AI's completed CAIQ on the public STAR Registry. Updated every year. | [link] |
| CSA STAR for AI Level 1 | Saifs AI's completed AI-CAIQ on the same registry. | [link] |

-->

| Design fact | Detail |
| --- | --- |
| Customer data stored | <!-- TODO Saifs AI: confirm hosted-tool retention. The local engine keeps rows in your sandbox and drops identifiers on read. --> |
| Passwords held | None. Sign-in is Microsoft single sign-on. |
| Write access to your systems | None |
| Unit of analysis | Groups. Individual scores are not returned |
| Local engine | Plain Python, one dependency, no network calls, open to review |

**Independent audits.** Saifs AI does not yet hold an independently audited certificate such as SOC 2 or ISO 27001. If your review needs a completed security questionnaire, request it through the [contact page](https://saifs.ai/contact-us).

## Token usage and cost

With the hosted tool, token use grows with every row, because the model sends those rows out and reads a card back. With the local engine, the rows stay in the sandbox, so that row cost does not grow with the file.

| File | Hosted tool: tokens and cost per run | Local engine: first run | Local engine: each later run | Comparison |
| --- | --- | --- | --- | --- |
| 40 stage-count rows | about 2,900 tokens, about $0.02 | Script not measured | about 70 tokens, under $0.01 | Hosted tool. One small call |
| 5,000 stage-count rows | about 356,000 tokens, about $1.99 | Script not measured | about 70 tokens, under $0.01 | Hosted call sends every row |
| 50,000 stage-count rows | Not the hosted route. Sending the rows alone is about 1,760,000 tokens | Script not measured | about 70 tokens, under $0.01 | Local engine only |

**Estimate your own file**

- **Hosted tool, to send:** about 36 tokens per stage-count row.
- **Hosted tool, to read:** about 36 tokens per result card in this sample, one card per row.
- **Local engine, later run:** about 70 tokens for a short decision print. It does not include the raw rows.

**Which route to choose**

- **One funnel of group counts:** the hosted tool. Forty rows on this sample are about $0.02.
- **A file near 5,000 rows:** the hosted call is about $1.99. Use the local engine when the export is large or still contains names. The script drops identifying columns on read.
- **More than about 5,000 rows:** the local engine. It is the only route.
- **Hiring rows must stay in your environment:** the local engine, with the hosted tool switched off.

**Hosts that bill per action.** Some agent hosts charge a fixed amount for each tool call and do not bill by token. On those hosts the measured token saving does not change the bill. Choose the local engine for file size and for keeping the export in your environment.

How these figures were measured: token counts use the `o200k_base` tokenizer on generated sample data, with rows written as JSON, on 16 November 2026. Costs use $1.25 per million input tokens and $10 per million output tokens. Rows the model sends are output tokens. The result it reads is input tokens. Result cards are representative objects from the documented fields, not a captured engine file. The local-engine script is not in this folder, so the first local run has no dollar figure. Do not copy the Inventory script cost onto this connector.

## Prerequisites

- A Saifs AI account, created by signing in with Microsoft single sign-on
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Access to Copilot Studio, Microsoft 365 Copilot, Azure AI Foundry, Power Automate, Power Apps, or Azure Logic Apps
- Group counts, or anonymised stage rows. Voluntary group labels only
- For the local engine: code execution enabled on the agent host

## How to get credentials

No keys or secrets are needed. When you create a connection, you are redirected to sign in with your Microsoft account and approve access. If the connection later expires, sign in again from the connection page.

## Get started with your connector

1. In Copilot Studio, open your agent and go to **Tools**, then **Add a tool**.
2. Search for **HR Hiring Fairness & Selection** and select it.
3. Create a connection and sign in.
4. Add this to the agent's instructions: "For any question about hiring fairness, adverse impact, or whether a test is worth its cost, call the HR Hiring Fairness tool. Do not calculate rates yourself. Never score, rank, or select an individual person."
5. Provide group counts, or rows with only stage, group, selected, and stratum. Do not send names.
6. Ask in plain language, for example "Which hiring stage should we review for fairness?"

Treat results as a review aid. Counsel and the hiring owner decide what to change.

## For administrators

Enabling this connector gives the organisation one approved way to review whether a hiring process treats groups fairly, and whether a test is worth its cost.

**Why this is an administrator decision.** Without a calculation tool, makers ask an agent whether a funnel is fair, or to rank candidates from a spreadsheet. A language model can give a different story on each run, and it can score individual people. A governed connector stays on group outcomes and method value.

- **You choose who can connect it.** Tenant and environment permissions apply.
- **You keep your system of record.** The connector analyses and explains. It does not hire, reject, or write back to any system.
- **You can check the number.** Impact ratios, pass marks, and money values can be rerun on the same file. Small groups are reported. Counts below the display minimum are hidden.
- **You keep the analysis at group level.** Voluntary labels only. Identifiers do not belong in the file.
- **You control where data goes.** Turn off the hosted tool per agent to keep the file in your environment.
- **You set data policy as usual.** Place the connector in the Business, Non-business, or Blocked group.
- **There are no secrets to rotate.** Expired connections are renewed by signing in again.

**Review checklist**

- Confirm the connector will be used for group review only, not individual selection
- Decide whether the hosted tool is allowed, or local engine only
- Assign the connector to the correct data policy group
- Review the privacy policy and terms with counsel if hiring data is in scope
- Read the self-assessments under Security and compliance, once they are published
- Review the local engine script if your policy requires it

## For developers

Prefer `stage_counts`. If you must send person rows, send only `stage`, `group`, `selected`, and `stratum`.

### Stage count fields

| Field | Meaning | Needed for |
| --- | --- | --- |
| `stage` | Hiring stage, in funnel order | Every rate |
| `group` | Voluntary self-identified group | Every comparison |
| `applicants` | How many people in this group reached this stage | The denominator |
| `selected` | How many moved forward | The numerator |
| `stratum` | Job, department, or location | Like-with-like rates |

### Person rows and validity pairs (optional)

`candidates` is an alternative to stage counts: one row per person per stage, with only stage, group, selected, and optional stratum.

`score_outcomes` is for a validity study: `score`, optional `outcome`, optional `group`. No identifiers. Individual scores are not returned.

### Options and defaults

| Option | Default | Purpose |
| --- | --- | --- |
| `four_fifths` | 0.80 | Impact-ratio line. A ratio under this flags a stage for review |
| `alpha` | 0.05 | Significance level for the chance check |
| `min_group_size` | 30 | Smallest group treated as a stable rate |
| `min_cell_display` | 5 | Counts below this are hidden |
| `tenure_years` | 1 | Expected years a hire stays, for the money value |
| `current_validity` | 0 | Validity of the method being replaced |
| `group_label` | `group` | What the group column means, in your words |
| `reference_group` | Highest selection rate | The group every other group is compared with |
| `currency` | USD | Display label only. No conversion |
| Salary fallback | 40% of `annual_salary` | Used when `sd_performance_value` is omitted and salary is supplied |

`validity` ranges from -1 to 1. `selection_ratio` and `base_rate` are fractions between 0 and 1. `hires_per_year` is required for the money value.

### Sample request

```json
{
  "stage_counts": [
    { "stage": "applied", "group": "A", "applicants": 200, "selected": 200 },
    { "stage": "interviewed", "group": "A", "applicants": 200, "selected": 80 },
    { "stage": "applied", "group": "B", "applicants": 180, "selected": 180 },
    { "stage": "interviewed", "group": "B", "applicants": 180, "selected": 40 }
  ],
  "group_label": "group",
  "currency": "USD"
}
```

These group labels are placeholders. Send the voluntary categories your process actually uses.

### Sample response (shortened)

<!-- TODO Saifs AI: replace this note with a shortened response from a real call. Do not publish invented impact ratios. -->

A live shortened response is not included in this draft. Capture one call before you publish the page. Do not add person-level scores to the sample.

### Response sections

| Section | Use it for |
| --- | --- |
| Stage to review | Impact ratio against the four-fifths line, and the chance check |
| Like with like | Whether an overall rate hides a gap inside a stratum |
| Validity | Whether the method predicts success, and whether it underrates a group |
| Pass mark | The suggested cutoff and the group effect of that line |
| Money value | Whether the method is worth its cost, when the value inputs are present |
| Suppressed counts | Groups too small to treat as a stable rate, and counts that stay hidden |

## Known issues and limitations

- **Group level only.** It does not score, rank, rate, or select an individual person, and it does not rank people for termination, layoff, promotion, or surveillance.
- **Not legal advice.** A four-fifths flag is not a legal finding. Employment law varies by jurisdiction.
- **Decision support only.** It does not hire, reject, or write to any system.
- **Voluntary groups only.** Do not infer a group from a name or a photo.
- **Small groups.** Under 30 people, a rate is not treated as stable. Counts under 5 are hidden.
- **Hosted tool size.** About 5,000 rows. Use the local engine above that.
- **Money value.** If the value of a performance difference is omitted, 40% of annual salary is used when salary is supplied. Tenure defaults to 1 year.
- **Currency.** Display label only. No conversion between currencies.
- **Identifiers.** Names, emails, phones, ids, addresses, Aadhaar, PAN, SSN, and dates of birth are dropped if present. Do not send them.
- **Regions.** Not available in US Government (GCC, GCC High, DoD) or China (21Vianet) clouds.

<!-- TODO Saifs AI: confirm the excluded-cloud sentence for this offer. -->

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| The tool cannot run, or asks you to sign in again | The connection has expired | Open the connection and sign in again |
| A group is marked too small | Fewer than 30 people, unless you changed `min_group_size` | Do not treat that rate as stable. Collect a larger group or keep the flag |
| Counts you expected are missing | They are below the display minimum of 5 | Do not try to reconstruct them |
| No money value | Salary or `sd_performance_value`, or hires per year, was not sent | Add those inputs if you need the cost comparison |
| The agent ranks individual people | Instructions do not forbid it | Add the instruction in Get started, and do not send person-level scores for ranking |
| The local engine does not run | Code execution is not enabled | Enable it, or aggregate to stage counts and use the hosted tool |
| Not permitted, or a subscribe link | The account cannot call the tool | Paste the full Marketplace link exactly as returned |

## FAQ

**Do I need a specialist to use this?**
No. A user asks in plain language. The formulas run inside the tool. Counsel still interprets the result for your jurisdiction.

**What is the least data I need?**
Group counts for one stage: group, applicants, and selected. More stages, a stratum, and validity inputs make a fuller review.

**Does my data leave my environment?**
Not with the local engine. With the hosted tool, the rows in the call are sent for calculation. Send group fields only. You can turn the hosted tool off.

**Will it change anything in my systems?**
No. It does not hire, reject, or write to an applicant system or Dataverse.

**Our applicant system already has an MCP server. Why add this?**
That server gives the agent the system's records. This connector adds the group-level calculation. It does not replace the system of record.

**Why do I get the same answer every time?**
The same counts and settings run the same formulas. The agent's wording may vary. The numbers do not.

**What happens if my data is incomplete?**
The tool runs the layers it can support. Small groups are flagged. Counts below the display minimum are hidden. Missing money inputs are not invented.

**Can I check how a result was produced?**
Yes. Each card shows the rate, the impact ratio, and whether the group was large enough.

**How large a file can it handle?**
About 5,000 rows on the hosted tool. Larger files use the local engine.

**Can it run on a schedule?**
Yes, from a scheduled flow. The connector itself runs on request.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment). Microsoft Premium connector licensing applies separately.

## Support

**HR Hiring Fairness & Selection - Saifs AI** is published by Saifs AI.

- Website: [https://saifs.ai](https://saifs.ai)
- Learn more: [https://saifs.ai/hr-hiring-fairness-selection](https://saifs.ai/hr-hiring-fairness-selection)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Privacy: [https://saifs.ai/privacy_policy/hr-hiring-fairness-selection](https://saifs.ai/privacy_policy/hr-hiring-fairness-selection)
- Terms: [https://saifs.ai/eula](https://saifs.ai/eula)
- Sign-in: Microsoft single sign-on (OAuth 2.0). No API keys.
- MCP endpoint: `https://hiring-fairness-mcp-microsoft.saifs.ai/mcp`

<!-- OPEN QUESTIONS
1. Confirm the marketplace offer URL.
2. Confirm hosted-tool retention and the audit log.
3. Capture a shortened live response. Keep it group-level.
4. Hosted send and read tokens are measured on generated JSON. The local-engine script is still not tokenized. A live result file would replace the representative result cards.
5. Confirm excluded clouds.
6. Operation summary is 36 characters: "Hiring Fairness and Selection Value". Confirm it stays within 80 and uses only letters, numbers, and parentheses. The ampersand was removed because the rule allows letters, numbers, and parentheses only.
7. Publisher Attestation and STAR rows stay commented until completed and linked.
-->
