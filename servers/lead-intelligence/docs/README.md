<!-- DRAFT for review. Do not replace intro.md until this file is accepted.
Tool list: lead_scoring_mcptools.json.
lead_pipeline_get_engine is not a local-only score. The tool description says the script scores through the Lead Pipeline API and needs network access. Stdlib only, not numpy. The older intro says numpy and implies the file stays local. This draft follows the tool description.

FIELD BLOCK
Title (11 characters): Lead Scorer
Description (224 characters):
Scores an open lead list into a daily call, nurture, or verify queue, with money at risk shown as a range. The same file and settings return the same queue. It does not update your CRM. A person still decides who to contact.

Operation summary (34 characters): Daily call nurture or verify queue
Operation description:
Scores open leads into a call, nurture, or verify queue and shows money at risk as a range. The same file and settings return the same queue.
-->

# Lead Scorer - Saifs AI

**Lead Scorer - Saifs AI** helps sales managers **decide who to contact today, and how much open pipeline is stalling**. You send lead rows. You can add a touch log and a stage history. It returns a call, nurture, or verify queue, and money figures as a range. The same file and the same settings return the same queue.

It is built for teams that keep an open lead book in a CRM or a spreadsheet and want a daily list ranked by money, not by whoever was touched last.

| If you are | Start with |
| --- | --- |
| A sales manager or revenue lead | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [Where the data comes from](#where-the-data-comes-from) |
| An administrator or security reviewer | [Data handling and security](#data-handling-and-security), [Responsible use](#responsible-use), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [Where the data comes from](#where-the-data-comes-from), [For developers](#for-developers) |
| A sales-ops or audit reviewer checking the method | [Inside the calculation](#inside-the-calculation) |

## The problem it solves

Most teams still work the book from the last touch, the stage name, or the deal size. That misses a quiet late-stage deal, and it spends the morning on noisy early leads. Asking an agent to "rank these leads" produces a new list each time.

Lead Scorer makes that work available to any AI agent:

- **Who to call**: open leads that have gone quiet, ranked by value at risk. The default silence is 21 days.
- **Who to nudge**: leads that are still moving, ranked by the value of one more contact.
- **Who to check first**: thin records, marked verify, so a person confirms them before anyone treats the score as sure.
- **How much is open**: pipeline value and value at risk, as a range.
- **Which channels show up on won deals**: relative credit when the touch log is dense enough. A pattern in the file, not proof that a channel caused the win.

## Why choose it: the same book, the same queue

The reason to use this connector is that the queue is fitted to your leads. Ask again with the same rows and the same settings and you get the same call, nurture, and verify list. The agent does not invent a hot lead.

**Thin history does not get a precise model.** Heavier layers need enough closed leads. The current intro says that is typically about 30 closed leads and about 8 wins. Below that, those layers refuse with a reason, and a simpler queue can still come back. Do not fill the gap with a made-up probability.

**It does not update the CRM.** A person still decides who to contact, and still writes the outcome back.

**Money is a range.** Do not publish a single point as the pipeline.

## Who should use this

| Role | How it helps |
| --- | --- |
| **Sales executives** | A stalling-pipeline figure, with a range, before the forecast meeting |
| **Sales managers** | A ranked list for the morning |
| **SDRs and inside sales** | Call, nurture, or verify, instead of oldest-first only |
| **Account executives** | Which moving deals are still worth a contact |
| **RevOps** | One method on a CRM or spreadsheet export |
| **Demand-gen leads** | Which channels appear on won deals, when a touch log is attached |
| **Agent makers** | Call a tool instead of asking the agent to invent a ranking |

**Best fit:** teams with an open lead or opportunity book, a lead id, and a created date. Stage, deal size, touches, and stage history make the money figures sharper.

**Not required:** a data-science team or an API key. A Saifs AI account and OAuth 2.0 sign-in are required.

## Use cases

**1. Monday call list**
Scenario: this week's open leads are in a sheet.
Outcome: a ranked call, nurture, and verify queue. The default queue length is 10.
Ask: "Who should the team call today?"

**2. Pipeline that has gone quiet**
Scenario: the forecast meeting needs a stalling figure.
Outcome: value at risk as a range, plus silent late-stage leads.
Ask: "How much pipeline is stalling, and which deals have been quiet for 21 days?"

**3. A queue from a CRM export**
Scenario: leads have an id, a created date, a stage, and a deal size.
Outcome: reps work a money-ranked list. The CRM is not updated.
Ask: "Score this lead export into a daily queue."

**4. Advance deals that are still moving**
Scenario: recent touches are in the file.
Outcome: nurture items ranked by the value of one more contact.
Ask: "Which active deals are still worth a follow-up?"

**5. Thin records**
Scenario: some leads have no activity.
Outcome: they land in verify. They are not auto-called.
Ask: "Which leads should we qualify before we trust the score?"

**6. Channels on won deals**
Scenario: a touch log has lead id, date, and channel, and enough deals have closed.
Outcome: relative credit. Say it is a pattern in the file.
Ask: "Which channels show up on won deals in this touch log?"

## Agents you can build

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Daily queue agent | Returns call, nurture, and verify | A lead export. Optional touches and stage history | 1, 3 |
| Forecast briefing agent | States pipeline at risk as a range | The open book, including won and lost rows if you have them | 2 |
| Nurture agent | Ranks moving deals by contact value | A touch log | 4 |
| Qualify agent | Lists thin records for a person to check | Nothing extra. Low-confidence rows should not be auto-dialled | 5 |
| Channel-pattern agent | Shows which channels appear on won deals | A dense touch log. Do not call it proof | 6 |
| Large-book agent | Scores more than 500 leads without pasting every row into the chat | A sandbox with network access. The script calls the scoring API. The file still leaves the sandbox | Any |
| Scheduled queue agent | Builds the list on a cadence | A flow with a recurrence trigger. A person still decides who to contact | 1 |

The tools run on request. This connector does not dial, email, or write the CRM.

## At a glance

| Item | Detail |
| --- | --- |
| Tools | `lead_pipeline` (500 lead rows or fewer). `lead_pipeline_get_engine` (more than 500) |
| Minimum input | `lead_id` and `created_date` on each lead |
| Defaults | Horizon 90 days. Silence 21 days. Queue length 10. Currency `USD`. Diagnostics off |
| Writes to your systems | None |
| Same file, next run | Same queue, if the settings match |
| Pricing | [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment) |

## Where the data comes from

You export the rows. This connector has not been certified against each CRM named below. Salesforce, HubSpot, Dynamics, and Excel are examples of exports that can contain the fields. They are not integrations.

| File | Required | Useful when you have it |
| --- | --- | --- |
| Leads | `lead_id`, `created_date` | `stage`, `status`, `closed_date`, `deal_size`, `source` |
| Touches | Not required | `lead_id`, `date`, optional `channel` |
| Stage history | Not required | `lead_id`, `date`, `stage` |

Deal size must be zero or positive. Missing sizes are not a reason to invent a value in the agent. Send the export as it is and read the data-quality notes in the result.

Currency is a label. Sending `INR` does not convert amounts. Send deal sizes already in the currency you want to read.

## How it works

Both routes score through Saifs AI. The hosted tool takes the rows in the call. The engine tool returns a script that loads the file in a sandbox and scores it through the Lead Pipeline API. The rows are not pasted into the chat on that second route. They do still leave the sandbox, because the script calls the API. The tool description says the script uses the Python standard library and needs network access. It does not say numpy.

### Inside the connector

1. The user asks who to contact, or how much pipeline is stalling, and provides a lead export.
2. At 500 lead rows or fewer, the agent calls `lead_pipeline` at `https://lead-scoring-mcp-microsoft.saifs.ai/mcp`.
3. Over 500 leads, the agent calls `lead_pipeline_get_engine` with the file path, saves the script, and runs it as given.
4. The agent leads with the queue and the money range. A person decides who to contact.

Turning off `lead_pipeline` and leaving `lead_pipeline_get_engine` on stops the chat-sized call. It does not keep the file inside the sandbox. The script still sends the file to the scoring API. Open the agent's **Tools** tab, select this server, turn off **Allow all**, and switch the hosted tool only if you want to force the script path for large books.

### Inside the calculation

The live tool description names these outputs: conversion outlook, money at risk, value of contact, funnel outlook, multi-touch credit (Shapley) when touches are dense enough, and a daily call, nurture, or verify queue. Stage history is what the description calls a Markov-style funnel. Without it, the funnel falls back to observed rates by stage.

| Step | What happens | What to know |
| --- | --- | --- |
| 1. Clean | Rows without an id or a created date are dropped and counted | The tool does not invent the missing id |
| 2. Queue | Call, nurture, or verify | Call means quiet. Nurture means still moving. Verify means thin. Default silence is 21 days. Default list length is 10 |
| 3. Money | Value and value at risk as a range | Do not quote a bare point as the forecast |
| 4. Heavier layers | Conversion timing and channel credit | The current intro says these want about 30 closed leads and about 8 wins. Below that they refuse with a reason |
| 5. Channel credit | Relative credit on closed deals | A pattern in the touch log. Not proof a channel caused the win |

**What happens when an input is missing.** Leads alone can still produce a queue. Without touches, engagement and channel credit are weak or skipped. Without stage history, the funnel uses observed rates by stage. The tool does not invent the missing log.

## Data handling and security

| Question | Hosted `lead_pipeline` | Engine script |
| --- | --- | --- |
| Do lead rows reach Saifs AI? | Yes. They are in the tool call | Yes. The script calls the scoring API. They are not pasted into the chat |
| Where is the work done? | `lead-scoring-mcp-microsoft.saifs.ai`, over HTTPS | The script runs in the sandbox and calls that API. Standard library. Network required |
| Does this keep the file inside the tenant? | No | No |
| Where is the result stored in your tenant? | In the agent thread, unless you save it | Where the script writes it. Confirm the file name from the script you receive |
| How long does Saifs AI keep an upload? | <!-- TODO Saifs AI: confirm retention and the audit log for both routes. --> | Same open question. This route still uploads |
| Write access to the CRM | None | None |

**Sign-in.** OAuth 2.0 through Saifs AI. No API key is created for the maker to paste into the agent.

Lead names, ids, and activity can be personal data. Send only rows you are allowed to process.

## Responsible use

The queue is decision support for a person.

- A verify row is not a person to dial automatically.
- Do not treat a conversion figure as a quota, a commission, or a credit decision.
- Channel credit is not a reason to cut a channel on its own. It is a pattern in closed deals.
- Do not add fields the tool does not ask for, such as age, gender, or other traits that are not in the schema.
- The connector does not contact the lead. Your team does, under your own outreach rules.

## Token usage and cost

Prices below use the rates published on 16 November 2026: **$1.25 per million tokens the model reads**, and **$10 per million tokens the model sends**. Rows the model sends are the expensive direction. Counts use the `o200k_base` tokenizer on spaced JSON. The queue cards are a representative shape, not a captured live result. A longer live ledger raises the read cost only. The script is not in this folder, so its first-run token cost is not stated. Do not copy another connector's script cost onto this one.

One lead row is about 49 tokens to send. One touch is about 27. One stage-history row is about 27.

| File | Hosted tokens and cost | Script first run | Later chat print | Comparison |
| --- | --- | --- | --- | --- |
| 40 leads, 40 touches, 40 stage rows | About 3,900 sent and 1,000 read. About $0.04 | Script not measured | About 25 tokens, under $0.01 | Fine on the hosted tool |
| 500 of each | About 48,500 sent and 1,000 read. About $0.49 | Script not measured | Under $0.01 for the printed queue | Hosted cap. The read stays small only if the agent reads the queue, not every lead card |
| 5,000 lead rows, no touches | Not the hosted tool. Sending the leads alone is about 245,000 tokens | Script not measured | The print is small. The file still goes to the scoring API | Use the script so the rows are not pasted into the chat. This does not keep them on your machine |

Hosts that charge a fixed amount per tool call are not changed by these token figures. The script path is for size. It is not a way to keep lead data inside the tenant.

## Security and compliance

**Reviewed by Microsoft**

| Item | What it covers |
| --- | --- |
| Microsoft connector certification | Microsoft reviewed and tested this connector before listing it as a certified connector. The review covers the connector's function and packaging, not Saifs AI's internal systems. |

**Self-assessed by Saifs AI**

<!-- PUBLISH EACH ROW ONLY AFTER IT IS COMPLETED.

| Self-assessment | What it is | Where to read it |
| --- | --- | --- |
| Microsoft Publisher Attestation | Saifs AI's answers to Microsoft's questionnaire. Microsoft publishes the answers and does not verify them. | [link] |
| CSA STAR Level 1 | Saifs AI's completed CAIQ on the public STAR Registry. | [link] |
| CSA STAR for AI Level 1 | Saifs AI's completed AI-CAIQ on the same registry. | [link] |

-->

| Design fact | Detail |
| --- | --- |
| CRM writes | None |
| Lead data on the script path | Still sent to the scoring API |
| Passwords held | None. Sign-in is OAuth 2.0 |
| Low-confidence leads | Verify. Not an automatic call |

**Independent audits.** Saifs AI does not yet hold SOC 2 or ISO 27001. For a security questionnaire, use the [contact page](https://saifs.ai/contact-us).

## Prerequisites

- A Saifs AI account with access to Lead Scorer
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- OAuth 2.0 sign-in when you create the connection
- A lead export with `lead_id` and `created_date`
- For more than 500 leads: a sandbox that can run a Python standard-library script and reach the scoring API
- Access to Copilot Studio, Microsoft 365 Copilot, Azure AI Foundry, Power Automate, Power Apps, or Azure Logic Apps

## How to get credentials

No API key is created for you to copy. Create a connection and sign in with OAuth 2.0. If the connection expires, sign in again from the connection page.

## Get started with your connector

1. In Copilot Studio, open your agent and go to **Tools**, then **Add a tool**.
2. Search for **Lead Scorer** and select it.
3. Create a connection and complete OAuth 2.0 sign-in.
4. Add this to the agent's instructions: "When the user asks who to contact or how much pipeline is stalling, call the lead tool. At 500 leads or fewer, call lead_pipeline. Above that, call lead_pipeline_get_engine and run the script as given. Lead with call, nurture, and verify, and with the money range. Do not invent amounts. Do not tell anyone to auto-dial a verify row. Do not update the CRM."
5. Test with a small export that includes a few open leads and, if you have them, a few closed leads.

## For administrators

Enabling this connector gives the organisation one approved way to turn a lead export into a rerunnable daily queue.

- **You choose who can connect it.** Tenant and environment permissions apply.
- **You keep the CRM as the system of record.** This connector does not write back.
- **You can rerun the file.** The same rows and settings return the same queue.
- **You do not get a local-only mode.** Both routes send lead rows to Saifs AI. The script route only keeps them out of the chat.
- **You set a human check.** Verify rows need a person. The connector does not dial.
- **You set data policy as usual.** Place the connector in the Business, Non-business, or Blocked group.
- **There is no API key to rotate.** Expired connections are renewed by signing in again.

**Review checklist**

- Confirm the team may upload lead and activity exports
- Confirm nobody will treat the queue as an automatic dialler
- Assign the connector to the correct data policy group
- Review the privacy policy and terms
- Read the self-assessments once they are published

## For developers

| Tool | When | Required |
| --- | --- | --- |
| `lead_pipeline` | 500 lead rows or fewer | `leads` |
| `lead_pipeline_get_engine` | More than 500 lead rows | `file_path` |

| Field | Meaning | Default |
| --- | --- | --- |
| `leads[].lead_id`, `leads[].created_date` | Required on each lead | None |
| `stage`, `status`, `closed_date`, `deal_size`, `source` | Optional. `deal_size` minimum 0 | Omitted |
| `touches` | `lead_id`, `date`, optional `channel` | Omitted |
| `stage_history` | `lead_id`, `date`, `stage` | Omitted |
| `as_of` | Analysis date | Latest date in the data |
| `horizon_days` | Conversion window | 90 |
| `cold_threshold_days` | Silence before a lead counts as quiet | 21 |
| `call_queue_size` | Maximum daily list | 10 |
| `currency` | Display label. No conversion | `USD` |
| `include_diagnostics` | Extra fit detail | false |

Engine paths: `file_path` required. `touches_path` and `stage_history_path` optional. `currency` defaults to `USD`.

### Sample request

```json
{
  "leads": [
    {
      "lead_id": "L-10001",
      "created_date": "2026-01-05",
      "stage": "Opportunity",
      "status": "open",
      "deal_size": 25000,
      "source": "webinar"
    }
  ],
  "currency": "USD",
  "call_queue_size": 10
}
```

One open lead is a valid call. Heavier layers still need closed history. Do not expect channel credit from this sample.

### Sample response (shortened)

<!-- TODO Saifs AI: replace this note with a shortened response from a real call. Do not publish invented money. -->

A live shortened response is not included in this draft. Capture one small `lead_pipeline` call before you publish the page. The result the tool describes includes decisions, pipeline exposure, a per-lead ledger, and a call queue.

## Known issues and limitations

- **500 lead rows** on the hosted tool. Larger books use the script.
- **The script is not a private score.** It calls the scoring API and needs network. Standard library, not numpy. The older intro says numpy. This page follows the tool file.
- **About 30 closed leads and about 8 wins** for the heavier layers, as the current intro states. Those names are not repeated as a legal minimum in the tool schema. Below that, the intro says the layer refuses and a simpler queue remains.
- **Touches and stage history are optional** and change the result when they are present.
- **Verify is not a call.**
- **Currency is a label.**
- **No CRM write.**
- **Personal data.** Send only what you may process.
- **Not proof.** A touch pattern is not a lab result.
- **Script.** Run it as given. Its token cost is unmeasured because the script is not in this folder.

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| The tool asks you to sign in again | The connection expired | Sign in again with OAuth 2.0 |
| Rows dropped | Missing `lead_id` or `created_date`, or a bad date | Fix the export. Do not invent ids |
| No channel credit | No touch log, or too few closed deals | Say the layer was skipped. Do not invent credit |
| A verify lead was dialled | The agent treated every row as a call | Follow the instruction in Get started |
| A large file fails in chat | More than 500 leads were pasted into `lead_pipeline` | Use `lead_pipeline_get_engine` |
| The script cannot score | No network, or the script was rewritten | Run it as given, with network access to the scoring API |

## FAQ

**Do I need an analyst?**
No. Ask who to call and attach the export. A person still makes the call.

**What is the least I need to send?**
`lead_id` and `created_date` on each lead.

**Does my data leave my environment?**
Yes, on both routes. The script keeps the rows out of the chat and still sends them to the scoring API.

**Will it change anything in the CRM?**
No.

**We already sort by last activity. Why add this?**
The same file returns the same queue, with quiet deals ranked by money and thin deals marked verify.

**Why might two runs differ?**
They should match when the rows and the settings match. A different silence window or a new touch log is a different run.

**What happens if stage history is missing?**
The queue can still be built. The funnel uses observed rates by stage.

**Can I check how a name got on the list?**
The tool returns a short reason with the card. A live trace is not pasted into this draft.

**How large a file can I send?**
500 lead rows on the hosted tool. Above that, use the script.

**Can it run on a schedule?**
Yes, from a scheduled flow. A person still decides who to contact.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment). A hosted book of 500 leads with a touch and a stage row each is about $0.49 in model tokens at the 16 November 2026 rates. Microsoft Premium connector licensing applies separately.

## Support

**Lead Scorer - Saifs AI** is published by Saifs AI.

- Website: [https://saifs.ai](https://saifs.ai)
- Learn more: [https://saifs.ai/lead-scorer](https://saifs.ai/lead-scorer)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Privacy: [https://saifs.ai/privacy_policy/lead-scorer](https://saifs.ai/privacy_policy/lead-scorer)
- Terms: [https://saifs.ai/eula](https://saifs.ai/eula)
- Sign-in: OAuth 2.0. No API key for the maker to paste.
- MCP endpoint: `https://lead-scoring-mcp-microsoft.saifs.ai/mcp`

<!-- OPEN QUESTIONS
1. intro.md lists https://lead-scoring-mcp.saifs.ai/mcp. manifest.json lists https://lead-scoring-mcp-microsoft.saifs.ai/mcp. This draft uses the manifest URL.
2. The engine tool says the script calls the scoring API (stdlib, network). The older intro says a numpy sandbox. Confirm there is no local-only score.
3. Confirm the closed-lead minimum (intro says about 30 closed and about 8 wins). It is not in the tool schema.
4. Confirm hosted retention and the audit log.
5. Capture a shortened live response, including the engine result file name.
6. Confirm excluded government and China clouds.
7. Publisher Attestation and STAR rows stay commented until completed and linked.
8. Pricing uses the same Marketplace URL as the other Saifs AI connector pages.
-->
