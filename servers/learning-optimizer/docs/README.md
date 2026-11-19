<!-- DRAFT for review. Do not replace intro.md until this file is accepted.
FIELD BLOCK (Microsoft validated fields). Confirm before submission.

Title (18 characters): Learning Optimizer
Description (417 characters):
Helps education and training teams decide what each learner should review, when, and whether they can reach an exam. Send a review log with an item, a time, and a grade. Returns today's review list, the chance of remembering now, in 7 days, and in 30 days, the next review date, and a day-by-day plan to an exam date. Learner codes only. The same log and settings return the same dates. It does not write grades back.

Operation summary (31 characters): Next review dates and exam plan
Operation description:
Returns next review dates, recall chances, and a day-by-day plan to an exam date from a review log. It does not write grades.
-->

# Learning Optimizer - Saifs AI

**Learning Optimizer - Saifs AI** helps education, L&D, and test-prep teams decide **what each learner should review, when, and whether they can reach an exam**. You send a review log, one row per answer: who, which item, when, and how well. It returns today's review list with the most at-risk items first, the chance of remembering each item now, in 7 days, and in 30 days, the next review date, a day-by-day plan to an exam date, items that keep slipping, questions most learners miss, neglected topics, and how much daily study a retention target costs.

It is built for teams that still review on a fixed interval, or not at all, and who need a schedule they can rerun.

| If you are | Start with |
| --- | --- |
| A teacher, L&D lead, or product owner | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [Where the data comes from](#where-the-data-comes-from) |
| An administrator or security reviewer | [Responsible use](#responsible-use), [Data handling and security](#data-handling-and-security), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [For developers](#for-developers) |
| An academic reviewer checking the method | [Inside the calculation](#inside-the-calculation) |

## The problem it solves

Most teams still schedule reviews on a fixed gap, or not at all. Learners forget and then re-cram. A score-per-student summary cannot say which item is about to be forgotten, or whether a daily time budget can reach an exam date.

Learning Optimizer makes that schedule available to any AI agent:

- **What to review today**: a queue ordered with the most at-risk items first.
- **When to see an item again**: the next review date for the retention target you set. The default is 90%.
- **Whether they will remember it**: chance of recall now, in 7 days, and in 30 days.
- **Whether they can reach the exam**: a day-by-day plan to an exam date, aimed at exam-day retention. The default is 85%.
- **What keeps slipping**: items forgotten often enough to count as slipping. The default is 8 lapses.
- **Where the group is weak**: questions most learners miss, and topics that are being neglected.
- **What a retention target costs in time**: daily study load for 80, 85, 90, and 95 percent retention.

## Why choose it: the same schedule every time

Learning Optimizer is deterministic. The same review log and settings produce the same next review dates, the same chance of remembering, the same daily queue, and the same exam plan. Results are calculated from established memory models. They are not written afresh by a language model each time you ask.

That matters because teachers and L&D leads act on a date and a probability. They need a schedule they can rerun and compare week to week. The agent's wording around the numbers may vary. The numbers do not.

| What it does | Established method |
| --- | --- |
| Sets the next interval from review quality | SM-2 |
| Estimates how long a memory lasts from past answers | Half-life regression |
| Schedules from difficulty, stability, and retrievability | FSRS, including the FSRS-6 standard schedule |
| Uses the schedule measured most accurate on recent answers | Held-out comparison. Another model is used only after enough held-out answers |
| Flags items forgotten again and again | Leech rule, default 8 lapses |
| Sizes a day-by-day plan up to an exam date | Exam plan within the daily review limit you set |

**It does not guess.** Thin history stays on the standard schedule, and the result says so. A learner's own pace needs about 30 reviews (`min_reviews_for_pace`, default 30). Another model can win only after `min_test_reviews` held-out answers (default 100).

**It recommends and does not act.** It never writes grades, intervals, or review state back to an LMS, a flashcard app, or Dataverse. The schedule supports study. It does not guarantee an exam result, and it does not rank or shame a learner.

## Who should use this

| Role | How it helps |
| --- | --- |
| **Test-prep and coaching teams** | Learners review what they are about to forget, with a plan aimed at exam day |
| **Corporate L&D and compliance trainers** | Required knowledge is scheduled for retention, rather than clicked through once |
| **Language-learning products** | Vocabulary and item scheduling from a review export |
| **Medical and professional certification prep** | Per-item recall and a day-by-day plan up to the exam |
| **Ed-tech and curriculum teams** | A review queue and a retention read from an LMS, quiz, or flashcard export |
| **Teachers and academic coaches** | Today's revision list, items that keep slipping, and topics the class is neglecting |
| **Agent makers and developers** | Add a tested review schedule to an agent |

**Best fit:** test-prep, language learning, corporate L&D, medical and professional certification, and ed-tech teams that already have a review or quiz log.

**Not required:** an in-house spaced-repetition team, or a custom scheduler deployment. A review export with an item id, a time, and a grade is enough to start.

## Use cases

Each use case below can be built as an agent. The agent types are listed in [Agents you can build](#agents-you-can-build).

**1. Today's revision list**
Scenario: a teacher uploads a review log for one learner or a class.
Outcome: today's queue, most at risk first, so study time goes to items about to be forgotten.
Ask: "What should this learner review today?"

**2. Exam study plan**
Scenario: a test-prep learner has an exam date and a limited number of reviews per day.
Outcome: a day-by-day plan to that date, including new items when you send an item bank, sized to `daily_review_limit` and `new_items_per_day` (default 20).
Ask: "Build a day-by-day plan to this exam date."

**3. Predict recall for an item**
Scenario: you need the chance a learner still remembers an item later, not only whether they got it right last time.
Outcome: recall probability now, in 7 days, and in 30 days, plus the next review date.
Ask: "What is the chance they remember this item in 7 days and in 30 days?"

**4. Items that keep slipping**
Scenario: the same cards are missed again and again.
Outcome: items that have lapsed enough times (default 8) are flagged, so a teacher can reteach them.
Ask: "Which items keep slipping for this learner?"

**5. Questions most learners miss**
Scenario: a cohort export covers many anonymous learners on the same items.
Outcome: questions most learners miss, and neglected topics.
Ask: "Which questions do most learners miss?"

**6. Fit study time to a retention target**
Scenario: the team wants to know what 80, 85, 90, or 95 percent retention costs in daily study.
Outcome: the daily load for each of those targets. The scheduling target itself defaults to 0.90 and can be set from 0.50 to 0.99.
Ask: "How much daily study does 90 percent retention take?"

**7. New material before the exam**
Scenario: the plan must cover items not yet studied.
Outcome: pass an item bank. The plan can introduce up to `new_items_per_day` new items.
Ask: "Add items we have not studied yet to the exam plan."

**8. A large review log**
Scenario: more than about 2,000 review rows or 2,000 items.
Outcome: the same calculation runs in the agent's sandbox, and learner data stays there.
Ask: "Schedule this large review export in the sandbox."

## Agents you can build

Each agent is one agent in your agent host with this connector added as a tool. The last column shows what else to add.

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Daily review agent | Answers what to review today, most at risk first | A review log with anonymous learner codes | 1 |
| Exam plan agent | Builds a day-by-day plan to an exam date | `exam_date`, and an item bank if new material must be covered | 2, 7 |
| Recall agent | Returns the chance of remembering now, in 7 days, and in 30 days | The full review history, not only today's answers | 3 |
| Slipping-item agent | Flags items a learner keeps missing | The same review log | 4 |
| Cohort weakness agent | Names questions most learners miss, and neglected topics | A multi-learner log with topic labels | 5 |
| Retention-load agent | Shows the daily study cost of 80, 85, 90, and 95 percent retention | Enough history to schedule | 6 |
| Large-log agent | Runs the local engine on a full export | Code execution on the agent host | 8 |
| LMS review agent | Reads anonymous review rows from the learning system and hands back a schedule for a person to assign | The LMS's own MCP server. Strip names before the call | 1, 5 |
| Scheduled revision agent | Sends today's list without anyone asking | A flow with a recurrence trigger, and email or Teams | 1 |

The tools run on request. Scheduling comes from the flow, not from the connector. The agent must not rank or shame learners.

## At a glance

| Item | Detail |
| --- | --- |
| Tools | `learning_optimizer` (hosted calculation) and `learning_get_engine` (local engine) |
| Minimum input | One review row with an item id, a time, and a grade. A learner id is required when there is more than one learner, and it must be an anonymous code |
| Hosted tool size | About 2,000 review rows and about 2,000 items |
| Larger files | Local engine, runs in your agent's code sandbox, needs only `numpy` |
| Writes to your systems | None |
| Pricing | [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment) |

<!-- TODO Saifs AI: confirm this marketplace URL is the pricing page for this offer. -->

## Where the data comes from

Learning Optimizer does not connect to your systems. The agent brings the review log. The connector needs only rows. It has not been certified against each system named below.

| Data source | How the agent gets the data | Best route |
| --- | --- | --- |
| Quiz, LMS, or flashcard export (for example an LMS gradebook of attempts, or an Anki revlog) | The user uploads the file. Learner ids must already be anonymous codes | Local engine for large logs, hosted tool up to about 2,000 rows |
| An LMS that has its own MCP server | The agent reads attempt rows, strips names, and passes anonymous codes | Same size rule |
| Dataverse tables | The Dataverse MCP server or the Dataverse connector | Same size rule |
| A scheduled flow | A flow reads new reviews and calls this connector | Hosted tool |

**Works alongside your learning system's MCP server.** That server gives the agent the attempt records. This connector adds the review schedule. A teacher or product team still assigns the work. This connector does not write it back.

**What to pull from the source**

| Data in your system | Field it fills | Why it matters |
| --- | --- | --- |
| Anonymous learner code | `learner_id` | Required when more than one learner is in the file. Not a name, email, phone, or roll number |
| Card, question, word, or problem | `item_id` | The thing being scheduled |
| When they answered | `timestamp` | The gap since the last review |
| How well they answered | `grade` | Drives the next interval |
| Topic, deck, or chapter | `topic` | Neglected topics and readable plans |
| Time spent | `duration_seconds` or `duration_ms` | Optional. Anki's `time` column is read as milliseconds |
| Items not yet studied | `items` | Needed for an exam plan that must cover new material |
| Exam date | `exam_date` | Turns the schedule into a day-by-day plan |

**Column names are recognised as they are.** `user_id`, `student_id`, and `uid` are read as learner. `card_id`, `question_id`, `cid`, and `word` are read as item. `reviewed_at`, `date`, and Anki epoch milliseconds are read as time. `rating`, `correct`, `score`, `quality`, and Anki `ease` are read as grade.

Send the full history, not only today's answers. A score-per-student summary will not work.

**File formats for the local engine.** The script reads the user's file in the sandbox and writes `learning_result.json`. Pass `exam_date`, `daily_review_limit`, `target_retention`, `grade_scale`, and the other settings in `params`. Learner ids in the file must already be anonymous codes.

## How it works

The connector offers two routes to the same calculation. The agent picks the route by the size of the data.

| | Hosted tool | Local engine |
| --- | --- | --- |
| Tool name | `learning_optimizer` | `learning_get_engine` |
| Use when | About 2,000 review rows and 2,000 items, or fewer | Anything larger, or a file already in the sandbox |
| What the agent sends | The rows and settings | File path and settings only |
| What comes back | The full result | A ready-to-run Python script |
| Where the calculation runs | Saifs AI endpoint | Your agent's code-execution sandbox |
| Where learner data goes | To the Saifs AI endpoint for calculation | Nowhere. It stays in the sandbox |
| Requires | Nothing extra | An agent host with code execution, and `numpy` |

**Local engine, step by step**

1. The user uploads or points the agent to a review file with anonymous codes.
2. The agent calls `learning_get_engine` with the file path and settings.
3. The connector returns the calculation script.
4. The agent runs the script unchanged in its sandbox.
5. The script prints the decision cards and writes lists, exam plans, and item states to `learning_result.json`.
6. The agent reports the schedule. It does not write grades back.

### Inside the connector

1. The user asks what to review, when, or whether an exam date is reachable.
2. The agent chooses a tool and calls the connector, which passes the call to the Saifs AI MCP server over HTTPS at `https://learning-optimizer-mcp-microsoft.saifs.ai/mcp`.
3. The server checks the signed-in account.
4. The hosted tool runs the engine on the rows and returns the schedule. The engine tool returns the same engine as a script.
5. The agent explains the schedule in plain language. The dates and probabilities come from the engine. The wording comes from the agent.

### Inside the calculation

Both routes run the same engine.

| Step | What the engine does | Method |
| --- | --- | --- |
| 1. Read the log | Needs an item id, a time, and a grade on each row. Detects the grade scale unless you set `grade_scale` | Header matching. `grade_scale` default `auto` |
| 2. Choose a scheduler | `scheduler` default `auto` uses the model measured most accurate on recent answers: FSRS-6 standard, a per-learner personalised FSRS, half-life regression, or SM-2. You can force `standard`, `personalised`, `hlr`, or `sm2` | Held-out comparison |
| 3. Thin history | Below `min_test_reviews` (default 100), the standard schedule is used and the card says so. A learner's own pace needs `min_reviews_for_pace` (default 30) | Stated thresholds |
| 4. Next review | Sets the next date for `target_retention` (default 0.90, allowed 0.50 to 0.99). The longest gap is `maximum_interval_days` (default 36500) | The chosen memory model |
| 5. Recall | Reports the chance of remembering now, in 7 days, and in 30 days | The same model |
| 6. Today's queue | Orders due items most at risk first. Names up to `max_items_listed` (default 20) on each card | Ranking by recall risk |
| 7. Slipping items | Flags an item after `leech_lapses` forgotten reviews (default 8) | Leech rule |
| 8. Exam plan | If `exam_date` is set, builds a day-by-day plan aimed at `exam_target` (default 0.85). New items are capped by `new_items_per_day` (default 20). At most `max_learners_planned` learners get a plan in one call (default 50) | Constrained study plan |
| 9. Cohort read | Names questions most learners miss, and neglected topics when a topic column exists | Counts across learners |
| 10. Time cost | Reports the daily study load for 80, 85, 90, and 95 percent retention. `seconds_per_review` overrides the seconds used to turn a time budget into an item limit | Load from the schedule |

**Why the answer does not change.** The same log and settings return the same dates and probabilities. The agent's wording may vary.

**What happens when data is missing.** The engine reports the gap. It does not block a call that has an item, a time, and a grade. An exam plan needs an exam date. Covering unstudied material needs an item bank. `as_of` defaults to the latest review. `include_item_states` defaults to `auto`, which lists learner-item states when there are up to 2,000. `max_learners_listed` defaults to 200.

**Where the working is shown.** The result says which scheduler was used, and it says when thin history kept the standard schedule.

## Data handling and security

| Question | Local engine | Hosted tool |
| --- | --- | --- |
| Does learner data reach Saifs AI? | No | Yes, the rows passed in the call |
| Where is it processed? | Your agent host's code sandbox | `learning-optimizer-mcp-microsoft.saifs.ai`, over HTTPS |
| What happens to the data after the call? | It never left your sandbox | <!-- TODO Saifs AI: confirm retention for this server. --> |
| What does Saifs AI record? | Only that the engine script was requested, if your facts sheet confirms that | <!-- TODO Saifs AI: confirm the audit and billing log. --> |

**What "sandbox" means.** The sandbox is the code-execution environment of your own agent host. It belongs to your tenant, not to Saifs AI. Your host's data policies continue to apply.

**Use the local engine only.** If review logs must never leave your environment, turn off the hosted tool. In Copilot Studio, open the agent's **Tools** tab, select the Learning Optimizer server, turn off **Allow all**, and switch off `learning_optimizer`. Leave `learning_get_engine` on. This setting is per agent.

**Sign-in.** Users sign in with Microsoft single sign-on (OAuth 2.0). No API key is created, shared, or stored.

**No write access.** The connector does not write grades or review state to an LMS, a flashcard app, or Dataverse.

Review logs can be personal data, and learners may be minors. Use only data you are authorised to process.

## Responsible use

This connector works on pseudonymous learner codes. It supports study. It does not judge the learner.

- Send anonymous codes. Do not send names, emails, phone numbers, or roll numbers.
- Do not rank learners against each other, and do not shame a learner for a low recall chance.
- The schedule does not guarantee an exam result, a grade, or mastery.
- Frame the output as what to review next, not as a verdict on the person.
- Learners may be minors. Use only data you are authorised to process.

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
| Customer data stored | <!-- TODO Saifs AI: confirm hosted-tool retention. The local engine keeps rows in your sandbox. --> |
| Passwords held | None. Sign-in is Microsoft single sign-on. |
| Write access to your systems | None |
| Identity | Anonymous learner codes. Names do not belong in the file |
| Local engine | Plain Python, one dependency, no network calls, open to review |

**Independent audits.** Saifs AI does not yet hold an independently audited certificate such as SOC 2 or ISO 27001. If your review needs a completed security questionnaire, request it through the [contact page](https://saifs.ai/contact-us).

## Token usage and cost

With the hosted tool, token use grows with every review row, because the model sends those rows out and reads item states back. With the local engine, the rows stay in the sandbox, so that row cost does not grow with the file.

| Review log | Hosted tool: tokens and cost per run | Local engine: first run | Local engine: each later run | Comparison |
| --- | --- | --- | --- | --- |
| 50 review rows | about 6,000 tokens, about $0.03 | Script not measured | about 80 tokens, under $0.01 | Hosted tool. One small call |
| 2,000 review rows | about 238,000 tokens, about $1.21 | Script not measured | about 80 tokens, under $0.01 | Hosted call sends every row and reads item states |
| 20,000 review rows | Not the hosted route. Sending the rows alone is about 1,040,000 tokens | Script not measured | about 80 tokens, under $0.01 | Local engine only |

On the hosted tool, item states are listed up to about 2,000. The 20,000-row read in a local run is the printed decisions, not every item state, unless you ask for the full file.

**Estimate your own file**

- **Hosted tool, to send:** about 52 tokens per review row on this sample (learner code, item, time, grade, topic, and duration).
- **Hosted tool, to read:** about 67 tokens per item state, up to the 2,000 listed.
- **Local engine, later run:** about 80 tokens for a short decision print.

**Which route to choose**

- **One learner or a small class:** the hosted tool. Fifty rows on this sample are about $0.03.
- **A log near 2,000 rows:** the hosted call is about $1.21. Use the local engine when you do not want the log sent through the model.
- **More than about 2,000 review rows or 2,000 items:** the local engine. It is the only route.
- **Learner data must stay in your environment:** the local engine, with the hosted tool switched off.

**Hosts that bill per action.** Some agent hosts charge a fixed amount for each tool call and do not bill by token. On those hosts the measured token saving does not change the bill. Choose the local engine for file size and for keeping the log in your environment.

How these figures were measured: token counts use the `o200k_base` tokenizer on generated sample data, with rows written as JSON, on 16 November 2026. Costs use $1.25 per million input tokens and $10 per million output tokens. Rows the model sends are output tokens. The result it reads is input tokens. Item states are representative objects from the documented fields, not a captured engine file. The local-engine script is not in this folder, so the first local run has no dollar figure. Do not copy the Inventory script cost onto this connector.

## Prerequisites

- A Saifs AI account, created by signing in with Microsoft single sign-on
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Access to Copilot Studio, Microsoft 365 Copilot, Azure AI Foundry, Power Automate, Power Apps, or Azure Logic Apps
- A review log with an item id, a time, and a grade, using anonymous learner codes
- For the local engine: code execution enabled on the agent host

## How to get credentials

No keys or secrets are needed. When you create a connection, you are redirected to sign in with your Microsoft account and approve access. If the connection later expires, sign in again from the connection page.

## Get started with your connector

1. In Copilot Studio, open your agent and go to **Tools**, then **Add a tool**.
2. Search for **Learning Optimizer** and select it.
3. Create a connection and sign in.
4. Add this to the agent's instructions: "For any question about what to review, when to review, or an exam study plan, call the Learning Optimizer tool. Do not calculate intervals yourself. Use anonymous learner codes only. Do not rank or shame learners."
5. Provide the full review history, not a score-per-student summary. Include an exam date if you want a day-by-day plan.
6. Ask in plain language, for example "What should this learner review today?"

Treat results as a study aid. A teacher still assigns the work.

## For administrators

Enabling this connector gives the organisation one approved way to decide what each learner should review, when, and whether they can reach an exam.

**Why this is an administrator decision.** Without a calculation tool, makers ask an agent what a learner should revise from a spreadsheet. A language model can give a different date on each run, and the result cannot be compared week to week. A governed connector uses one schedule for every agent and course.

- **You choose who can connect it.** Tenant and environment permissions apply.
- **You keep your system of record.** The connector recommends a schedule. It does not write grades or review state back to any system.
- **You can check the date.** The same file and settings can be rerun. The result says which scheduler was used, and when history was too thin for a personal pace.
- **You keep identity out of the request.** Anonymous codes only.
- **You control where data goes.** Turn off the hosted tool per agent to keep review logs in your environment.
- **You set data policy as usual.** Place the connector in the Business, Non-business, or Blocked group.
- **There are no secrets to rotate.** Expired connections are renewed by signing in again.

**Review checklist**

- Confirm learner ids in source files are already anonymous
- Decide whether the hosted tool is allowed, or local engine only
- Assign the connector to the correct data policy group
- Review the privacy policy and terms, including use with minors where that applies
- Read the self-assessments under Security and compliance, once they are published
- Review the local engine script if your policy requires it

## For developers

`reviews` is required. Each row needs an item id, a time, and a grade. A learner id is required when there is more than one learner.

### Review fields

| Field | Meaning | Needed for |
| --- | --- | --- |
| `learner_id` | Anonymous learner code | Multi-learner files |
| `item_id` | Card, question, word, or problem | Every schedule |
| `timestamp` | When they answered. Date, ISO datetime, or epoch seconds or milliseconds | The interval |
| `grade` | true/false, 0/1, again/hard/good/easy, SM-2 quality 0 to 5, or a 0 to 100 score | The next interval |
| `topic` | Topic, deck, chapter, or skill | Neglected topics |
| `duration_seconds` or `duration_ms` | How long the answer took | Optional timing |

### Item bank and options

| Option | Default | Purpose |
| --- | --- | --- |
| `items` | None | Item bank, including items not studied yet |
| `as_of` | Latest review | Analysis date. A date means the end of that day |
| `target_retention` | 0.90 | Chance of remembering to schedule for. Range 0.50 to 0.99 |
| `exam_date` | None | Required for a day-by-day plan |
| `exam_target` | 0.85 | Chance of remembering each item on exam day |
| `daily_review_limit` | None | Most items a learner can do per day. Empty means no limit |
| `new_items_per_day` | 20 | New items a learner may start per day |
| `grade_scale` | `auto` | Also `fsrs_1_4`, `sm2_0_5`, `binary`, `percent` |
| `scheduler` | `auto` | Also `standard`, `personalised`, `hlr`, `sm2` |
| `maximum_interval_days` | 36500 | Longest gap the schedule may give |
| `leech_lapses` | 8 | Lapses before an item counts as slipping |
| `min_test_reviews` | 100 | Held-out answers before a model other than the standard schedule can win |
| `min_reviews_for_pace` | 30 | Reviews before a learner's own pace is used |
| `max_items_listed` | 20 | Due or slipping items named on each card |
| `max_learners_planned` | 50 | Learners who receive an exam plan in one call |
| `max_learners_listed` | 200 | Learners included in the returned list |
| `learners` | Everyone in the file | Restrict the call to these anonymous codes |

### Sample request

```json
{
  "reviews": [
    {
      "learner_id": "S-001",
      "item_id": "Q-14",
      "timestamp": "2026-11-01",
      "grade": "good",
      "topic": "algebra"
    },
    {
      "learner_id": "S-001",
      "item_id": "Q-14",
      "timestamp": "2026-11-08",
      "grade": "again",
      "topic": "algebra"
    }
  ],
  "exam_date": "2026-12-15",
  "target_retention": 0.9,
  "new_items_per_day": 10
}
```

### Sample response (shortened)

<!-- TODO Saifs AI: replace this note with a shortened response from a real call. Do not publish invented recall probabilities. -->

A live shortened response is not included in this draft. Capture one call before you publish the page. Use an anonymous learner code in the sample.

### Response sections

| Section | Use it for |
| --- | --- |
| Today's queue | What to review, most at risk first |
| Recall | Chance now, in 7 days, and in 30 days, plus the next review date |
| Exam plan | The day-by-day plan, when an exam date was sent |
| Slipping items | Items at or above the lapse threshold |
| Cohort | Questions most learners miss, and neglected topics |
| Retention load | Daily study for 80, 85, 90, and 95 percent retention |
| Scheduler used | Which model ran, and whether thin history kept the standard schedule |

## Known issues and limitations

- **Decision support only.** It does not write grades or review state to any system.
- **No exam guarantee.** The plan supports study. It does not promise a result.
- **Anonymous codes only.** Do not send names, emails, phones, or roll numbers.
- **Full history required.** A score-per-student summary will not work.
- **Thin history.** Below 100 held-out answers, the standard schedule is used. A personal pace needs about 30 reviews.
- **Exam plan.** Needs `exam_date`. New material needs an item bank.
- **Hosted tool size.** About 2,000 review rows and 2,000 items.
- **Listing caps.** Default 20 items named per card, 50 learners planned, 200 learners listed.
- **Do not rank or shame.** The queue is a study list, not a leaderboard.
- **Regions.** Not available in US Government (GCC, GCC High, DoD) or China (21Vianet) clouds.

<!-- TODO Saifs AI: confirm the excluded-cloud sentence for this offer. -->

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| The tool cannot run, or asks you to sign in again | The connection has expired | Open the connection and sign in again |
| The standard schedule was used | Fewer than 100 held-out answers, or the learner has fewer than about 30 reviews | Add history, or accept the standard schedule. The card says which |
| No exam plan | `exam_date` was not sent | Send the exam date |
| New items are missing | No item bank was sent | Pass `items` for material not yet studied |
| Grades look wrong | `auto` misread the scale | Set `grade_scale` to `fsrs_1_4`, `sm2_0_5`, `binary`, or `percent` |
| The agent invents an interval | Instructions do not require the tool | Add the instruction in Get started |
| The local engine does not run | Code execution is not enabled | Enable it, or use the hosted tool for a small log |
| Not permitted, or a subscribe link | The account cannot call the tool | Paste the full Marketplace link exactly as returned |

## FAQ

**Do I need a specialist to use this?**
No. A user asks in plain language. The schedule runs inside the tool.

**What is the least data I need?**
An item id, a time, and a grade. Add an anonymous learner id when the file has more than one learner.

**Does my data leave my environment?**
Not with the local engine. With the hosted tool, the rows in the call are sent for calculation. You can turn the hosted tool off.

**Will it change anything in my systems?**
No. It does not write grades, intervals, or review state to an LMS or Dataverse.

**Our LMS already has an MCP server. Why add this?**
That server gives the agent the attempt records. This connector adds the review schedule.

**Why do I get the same answer every time?**
The same log and settings return the same dates and probabilities. The agent's wording may vary.

**What happens if my data is incomplete?**
The tool still runs. Thin history stays on the standard schedule, and the result says so.

**Can I check how a result was produced?**
Yes. The result names the scheduler and whether a threshold kept the standard schedule.

**How large a file can it handle?**
About 2,000 review rows and 2,000 items on the hosted tool. Larger logs use the local engine.

**Can it run on a schedule?**
Yes, from a scheduled flow. The connector itself runs on request.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment). Microsoft Premium connector licensing applies separately.

## Support

**Learning Optimizer - Saifs AI** is published by Saifs AI.

- Website: [https://saifs.ai](https://saifs.ai)
- Learn more: [https://saifs.ai/learning-optimizer](https://saifs.ai/learning-optimizer)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Privacy: [https://saifs.ai/privacy_policy/learning-optimizer](https://saifs.ai/privacy_policy/learning-optimizer)
- Terms: [https://saifs.ai/eula](https://saifs.ai/eula)
- Sign-in: Microsoft single sign-on (OAuth 2.0). No API keys.
- MCP endpoint: `https://learning-optimizer-mcp-microsoft.saifs.ai/mcp`

<!-- OPEN QUESTIONS
1. Confirm the marketplace offer URL.
2. Confirm hosted-tool retention and the audit log.
3. Capture a shortened live response.
4. Hosted send and read tokens are measured on generated JSON. The local-engine script is still not tokenized. A live result file would replace the representative item states.
5. Confirm excluded clouds.
6. Publisher Attestation and STAR rows stay commented until completed and linked.
-->
