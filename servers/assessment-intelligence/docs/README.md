<!-- DRAFT for review. Do not replace intro.md until this file is accepted.
FIELD BLOCK (Microsoft validated fields). Confirm before submission.

Title (30 characters): Test Analytics & Psychometrics
Description (437 characters):
Helps education teams decide whether a test measures ability, how reliable it is, and who needs practice. Send response rows with a learner code, an item code, and whether the answer was right. Returns item flags, reliability, each learner's ability with a standard error, mastery by skill, the next question, and a check-in list. The same rows return the same numbers. It does not rank learners or write scores back. Learner codes only.

Operation summary (38 characters): Item quality ability and next question
Operation description:
Returns item flags, test reliability, learner ability with a standard error, mastery, and the next question. It does not rank learners.
-->

# Test Analytics & Psychometrics - Saifs AI

**Test Analytics & Psychometrics - Saifs AI** helps education, coaching, university, and corporate learning teams decide **whether a test measures ability, how reliable it is, and who needs practice**. You send response rows, and a learning history or an item bank when you have them. It returns decision cards: items that are not measuring ability, test reliability, each learner's ability with a standard error, mastery by skill, the next question, and a check-in list for the teacher.

It is built for teams that still judge a test by the average percent correct, and who need a read they can rerun.

| If you are | Start with |
| --- | --- |
| An academic lead, teacher, or L&D manager | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [Where the data comes from](#where-the-data-comes-from) |
| An administrator or security reviewer | [Responsible use](#responsible-use), [Data handling and security](#data-handling-and-security), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [For developers](#for-developers) |
| An assessment reviewer checking the method | [Inside the calculation](#inside-the-calculation) |

## The problem it solves

Most teams still score a test as a percent correct. That hides questions that do not measure ability. It mixes a learner's ability with how hard the form was. It cannot say which skill is mastered, or which question to ask next. A public ranking of learners adds a harm the score does not justify.

Test Analytics & Psychometrics makes that read available to any AI agent:

- **Which questions are not measuring ability**: items that are too easy, too hard, or that do not separate higher ability from lower ability.
- **Whether the test is reliable**: Cronbach's alpha for internal consistency.
- **Each learner's ability, apart from the form**: a Rasch ability, always with a standard error. A thin sample stays a proportion correct.
- **Which question to ask next**: the unused item that adds the most information at this learner's ability.
- **What has been mastered**: Bayesian mastery by skill, from a skill column or a learning history.
- **Who needs a check-in**: a support list for the teacher. Not a rank, not a scoreboard, and not a public comparison.

## Why choose it: the same read every time

Test Analytics & Psychometrics is deterministic. The same response rows, learning history, item bank, and settings produce the same item flags, the same reliability, the same ability with the same standard error, the same mastery, the same next question, and the same check-in list. Results are calculated from established psychometric models. They are not written afresh by a language model each time you ask.

That matters because academic leads and item editors act on which question to drop and who needs a check-in. They need a result they can rerun. The agent's wording around the numbers may vary. The numbers do not.

| What it does | Established method |
| --- | --- |
| Flags items that do not separate higher ability from lower ability | Classical item analysis (difficulty and discrimination) |
| Estimates each learner's ability apart from how hard the questions were, with a standard error | Rasch model |
| Reports whether the test is internally consistent | Cronbach's alpha |
| Picks the next question that tells you the most about this learner | Item choice by Fisher information. Items already asked are not offered again |
| Updates the chance a skill is mastered after each attempt | Bayesian knowledge tracing |

**It does not guess.** A Rasch fit needs at least `min_responses` scored answers (default 20), at least 5 learners, and at least 3 items that are not all right or all wrong. Below that, ability stays a proportion correct and the card says to add data. Quote each ability with its standard error.

**It recommends and does not act.** It never writes scores, item flags, or mastery back to an LMS, a quiz tool, or Dataverse. Educators remain responsible for what they drop, reteach, or record. The check-in list is not a ranking and not a scoreboard. Partial credit is not scored. Each answer is right or wrong.

## Who should use this

| Role | How it helps |
| --- | --- |
| **Test-prep and coaching academic leads** | See which questions are not measuring ability, and whether the form is reliable, before the next batch sits it |
| **Ed-tech product leads** | Choose the next question from an item bank using the learner's current ability |
| **University, school, and exam-office teams** | Separate ability from form difficulty, and read each estimate with its standard error |
| **Corporate L&D and certification trainers** | See which skills are mastered and who still needs practice |
| **Assessment publishers and item editors** | A repeatable item review: difficulty, discrimination, and a reason to keep or drop a question |
| **Teachers and cohort leads** | A check-in list of learners to support. Not a ranking and not a scoreboard |
| **Agent makers and developers** | Add a tested item and ability read to an agent |

**Best fit:** ed-tech platforms, coaching and test-prep institutes, universities and schools, corporate L&D and certification bodies, and assessment publishers that score quizzes and need to know whether the questions work.

**Not required:** a psychometrician on staff, or an enterprise testing suite. A response export with a learner code, an item code, and right or wrong is enough to start.

This is a check-in and an item review. It is not a tool for publishing a leaderboard or shaming a learner.

## Use cases

Each use case below can be built as an agent. The agent types are listed in [Agents you can build](#agents-you-can-build).

**1. Which questions to drop or rewrite**
Scenario: a test-prep lead uploads one sitting: learner code, question code, and right or wrong.
Outcome: cards flag items that are not measuring ability, so editors can drop or rewrite them before the next batch.
Ask: "Which questions are not measuring ability?"

**2. Is this assessment reliable?**
Scenario: a course team wants to know whether the quiz hangs together, not only the class average.
Outcome: Cronbach's alpha for that set of responses, alongside the item flags.
Ask: "How reliable is this test?"

**3. Ability, separate from how hard the form was**
Scenario: two cohorts sat forms that were not equally difficult, and percent correct makes them look different.
Outcome: a Rasch ability for each learner, with a standard error. Below the minimum data, ability stays a proportion correct and the card says to add data.
Ask: "Give each learner's ability with its standard error."

**4. The next question for one learner**
Scenario: a product flow should ask the question that tells you the most, and should not repeat a question already given.
Outcome: pass an item bank with `student_ability`, or pass `focus_learner` when the responses already contain that learner. Put already-given codes in `asked_items`. If no bank is sent, the next item is chosen from the questions just analysed.
Ask: "Which question should we ask this learner next?"

**5. Mastery by skill**
Scenario: a trainer needs to know whether a skill is mastered, not only whether one item was right.
Outcome: Bayesian mastery for that skill. A `learning_history` is used when you send one. Otherwise a `skill` column on the response rows feeds the same estimate.
Ask: "Which skills has this learner mastered?"

**6. Who needs a check-in**
Scenario: a teacher wants a short list of learners to sit with, without publishing a rank.
Outcome: a check-in list for the teacher. Do not turn it into a scoreboard.
Ask: "Who needs more practice? Give me a check-in list, not a ranking."

**7. A large response file**
Scenario: more than about 5,000 response rows, or the same cap on a learning history or an item bank.
Outcome: the same calculation runs in the agent's sandbox. The script needs only the Python standard library. Response data stays there.
Ask: "Analyse this large response file in the sandbox."

## Agents you can build

Each agent is one agent in your agent host with this connector added as a tool. The last column shows what else to add.

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Item review agent | Flags questions that are not measuring ability, and reports reliability | A response export with learner codes | 1, 2 |
| Ability agent | Returns each learner's ability with a standard error | Enough scored answers for a Rasch fit, or it stays a proportion correct | 3 |
| Next-question agent | Picks the unused item that adds the most information | An item bank, or the questions just analysed, plus `asked_items` | 4 |
| Mastery agent | Reports Bayesian mastery by skill | A skill column or a learning history in attempt order | 5 |
| Check-in agent | Names learners a teacher should look in on. It does not rank them | The same response file | 6 |
| Large-file agent | Runs the local engine above about 5,000 rows | Code execution on the agent host | 7 |
| Quiz-system agent | Reads response rows from the quiz or LMS, calculates the cards, and hands them back for a person to act on | That system's own MCP server. Pass learner codes, not names | 1, 6 |
| Scheduled item-review agent | Sends an item-quality note after each sitting | A flow with a recurrence trigger, and email or Teams | 1, 2 |

The tools run on request. Scheduling comes from the flow, not from the connector. Do not build an agent that publishes a scoreboard.

## At a glance

| Item | Detail |
| --- | --- |
| Tools | `assessment_intelligence` (hosted calculation) and `assessment_intelligence_get_engine` (local engine) |
| Minimum input | A learner code, an item code, and whether the answer was right |
| Hosted tool size | About 5,000 response rows, and the same cap on a learning history or an item bank |
| Rasch minimum | Default 20 scored answers, at least 5 learners, and at least 3 items that are not all right or all wrong |
| Larger files | Local engine. The script needs only the Python standard library, not `numpy` |
| Writes to your systems | None |
| Pricing | [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment) |

<!-- TODO Saifs AI: confirm this marketplace URL is the pricing page for this offer. -->

## Where the data comes from

Test Analytics & Psychometrics does not connect to your systems. The agent brings the rows. The connector needs only rows. It has not been certified against each system named below.

| Data source | How the agent gets the data | Best route |
| --- | --- | --- |
| Quiz, LMS, or spreadsheet export | The user uploads the file. Learner values must be codes | Local engine above about 5,000 rows, hosted tool below that |
| A quiz or LMS that has its own MCP server | The agent reads responses, drops names, emails, and phones, and passes codes | Same size rule |
| Dataverse tables | The Dataverse MCP server or the Dataverse connector | Same size rule |
| A scheduled flow | A flow reads a sitting and calls this connector | Hosted tool |
| An item bank maintained by editors | Passed as `item_bank` with difficulty, discrimination, and guessing | Either route, within the row cap |

**Works alongside your quiz system's MCP server.** That server gives the agent the response records. This connector adds the item, reliability, ability, mastery, and next-question calculation. A teacher still decides what to drop or reteach.

**What to pull from the source**

| Data in your system | Field it fills | Why it matters |
| --- | --- | --- |
| Learner code | `learner_id` | The only identity to send |
| Question code | `item_id` | Item analysis and the next question |
| Right or wrong | `correct` | Every calculation. Partial credit is not scored |
| Skill or topic | `skill` | Mastery, when you are not sending a separate learning history |
| Attempt order | `learning_history` | Mastery is traced from this log when it is sent |
| Unused questions | `item_bank` | The next-question choice. Difficulty defaults to 0, discrimination to 1, guessing to 0.2 |

**Column names are recognised as they are.** `student_id`, `user_id`, `learner`, and `student` are the same field as `learner_id`. `question_id`, `item`, and `question` are the same field as `item_id`. `score`, `is_correct`, and `response` are the same field as `correct`. `skill_id`, `topic`, and `topic_id` are the same field as `skill`.

Do not send names, emails, or phone numbers. Those fields are dropped, and the card says so.

**File formats for the local engine.** CSV with a header row. Default path `responses.csv`. Set `data_kind` to `responses` or `learning_history`. A learning-history file must include a skill column, in attempt order. The script writes `assessment_result.json`.

## How it works

The connector offers two routes to the same calculation. The agent picks the route by the size of the data.

| | Hosted tool | Local engine |
| --- | --- | --- |
| Tool name | `assessment_intelligence` | `assessment_intelligence_get_engine` |
| Use when | About 5,000 response rows, and the same cap on a learning history or an item bank | Anything larger |
| What the agent sends | The rows and settings | A file path and settings only |
| What comes back | The decision cards | A ready-to-run Python script |
| Where the calculation runs | Saifs AI endpoint | Your agent's code-execution sandbox |
| Where response data goes | To the Saifs AI endpoint for calculation | Nowhere. It stays in the sandbox |
| Requires | Nothing extra | An agent host with code execution. The script uses only the Python standard library |

**Local engine, step by step**

1. The user uploads or points the agent to a response file.
2. The agent calls `assessment_intelligence_get_engine` with `data_path` and any settings in `params`.
3. The connector returns the calculation script.
4. The agent runs the script unchanged in its sandbox.
5. The script prints the decision cards and writes `assessment_result.json`.
6. The agent reports the cards, with each ability quoted beside its standard error. It does not rank learners.

### Inside the connector

1. The user asks whether a test measures ability, how reliable it is, or who needs practice.
2. The agent chooses a tool and calls the connector, which passes the call to the Saifs AI MCP server over HTTPS at `https://assessment-intelligence-mcp-microsoft.saifs.ai/mcp`.
3. The server checks the signed-in account.
4. The hosted tool runs the engine and returns the cards. The engine tool returns the same engine as a script.
5. The agent explains the cards in plain language. The numbers come from the engine. The wording comes from the agent.

### Inside the calculation

Both routes run the same engine.

| Step | What the engine does | Method |
| --- | --- | --- |
| 1. Read responses | Keeps a learner code, an item code, and right or wrong. Drops names, emails, and phones | Header matching |
| 2. Item quality | Flags items that are too easy, too hard, or that do not separate abilities | Classical item analysis |
| 3. Reliability | Computes internal consistency for the set of responses | Cronbach's alpha |
| 4. Ability | Fits a Rasch ability and a standard error when the minimums are met. Otherwise leaves ability as a proportion correct and says to add data | Rasch model |
| 5. Next question | Chooses the unused item with the most information at this ability. `asked_items` are excluded. If no bank is sent, the choice is from the questions just analysed | Fisher information |
| 6. Mastery | Updates mastery by skill. On the local engine, `prior`, `learn`, and `slip` default to 0.1, and `guess` defaults to 0.2 | Bayesian knowledge tracing |

**Why the answer does not change.** The same rows and settings return the same cards. The agent's wording may vary.

**What happens when data is missing.** The engine reports the gap. It does not block a call that has a learner code, an item code, and a right-or-wrong mark. Mastery needs a skill column or a learning history. The next question needs a bank or the items just analysed. `min_responses` defaults to 20.

**Where the working is shown.** Each ability is returned with its standard error. Thin samples say to add data. Dropped identity fields are named on the card.

## Data handling and security

| Question | Local engine | Hosted tool |
| --- | --- | --- |
| Does response data reach Saifs AI? | No | Yes, the rows passed in the call |
| Where is it processed? | Your agent host's code sandbox | `assessment-intelligence-mcp-microsoft.saifs.ai`, over HTTPS |
| What happens to the data after the call? | It never left your sandbox | <!-- TODO Saifs AI: confirm retention for this server. --> |
| What does Saifs AI record? | Only that the engine script was requested, if your facts sheet confirms that | <!-- TODO Saifs AI: confirm the audit and billing log. --> |

**What "sandbox" means.** The sandbox is the code-execution environment of your own agent host. It belongs to your tenant, not to Saifs AI. Your host's data policies continue to apply.

**Use the local engine only.** If response rows must never leave your environment, turn off the hosted tool. In Copilot Studio, open the agent's **Tools** tab, select the Test Analytics & Psychometrics server, turn off **Allow all**, and switch off `assessment_intelligence`. Leave `assessment_intelligence_get_engine` on. This setting is per agent.

**Sign-in.** Users sign in with Microsoft single sign-on (OAuth 2.0). No API key is created, shared, or stored.

**No write access.** The connector does not write scores, item flags, or mastery to an LMS, a quiz tool, or Dataverse.

Response rows can be personal data, and learners may be minors. Use only data you are authorised to process.

## Responsible use

This connector works on learner codes. It supports teaching. It does not judge the learner in public.

- Send learner codes only. Do not send names, emails, or phone numbers.
- Do not rank learners, publish a scoreboard, or shame a learner.
- The check-in list is for the teacher.
- Read each ability with its standard error. A small sample is a weak estimate.
- The cards do not guarantee a grade, a placement, or mastery.
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
| Identity | Learner codes. Names, emails, and phones are dropped |
| Local engine | Plain Python, standard library only, no network calls, open to review |

**Independent audits.** Saifs AI does not yet hold an independently audited certificate such as SOC 2 or ISO 27001. If your review needs a completed security questionnaire, request it through the [contact page](https://saifs.ai/contact-us).

## Token usage and cost

With the hosted tool, token use grows with every response row, because the model sends those rows out and reads abilities and item flags back. With the local engine, the rows stay in the sandbox, so that row cost does not grow with the file.

| Response file | Hosted tool: tokens and cost per run | Local engine: first run | Local engine: each later run | Comparison |
| --- | --- | --- | --- | --- |
| 10 learners, 10 items (100 rows) | about 3,800 tokens, about $0.03 | Script not measured | about 80 tokens, under $0.01 | Hosted tool. One sitting |
| 250 learners, 20 items (5,000 rows) | about 165,000 tokens, about $1.56 | Script not measured | about 80 tokens, under $0.01 | Hosted call sends every row |
| 2,500 learners, 20 items (50,000 rows) | Not the hosted route. Sending the rows alone is about 1,550,000 tokens | Script not measured | about 80 tokens, under $0.01 | Local engine only |

**Estimate your own file**

- **Hosted tool, to send:** about 31 tokens per response row on this sample (learner code, item code, right or wrong, and skill).
- **Hosted tool, to read:** about 40 tokens per learner ability and about 28 tokens per item flag in this sample.
- **Local engine, later run:** about 80 tokens for a short decision print.

**Which route to choose**

- **One sitting of a few hundred rows:** the hosted tool. One hundred rows on this sample are about $0.03.
- **A file near 5,000 rows:** the hosted call is about $1.56. Use the local engine when the export is larger or must stay in your environment.
- **More than about 5,000 response rows, or the same cap on a learning history or an item bank:** the local engine. It is the only route.

**Hosts that bill per action.** Some agent hosts charge a fixed amount for each tool call and do not bill by token. On those hosts the measured token saving does not change the bill. Choose the local engine for file size and for keeping response rows in your environment.

How these figures were measured: token counts use the `o200k_base` tokenizer on generated sample data, with rows written as JSON, on 16 November 2026. Costs use $1.25 per million input tokens and $10 per million output tokens. Rows the model sends are output tokens. The cards it reads are input tokens. Ability and item records are representative objects from the documented fields, not a captured engine file. The local-engine script is not in this folder, so the first local run has no dollar figure. Do not copy the Inventory script cost onto this connector. The local script for this connector uses only the Python standard library.

## Prerequisites

- A Saifs AI account, created by signing in with Microsoft single sign-on
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Access to Copilot Studio, Microsoft 365 Copilot, Azure AI Foundry, Power Automate, Power Apps, or Azure Logic Apps
- Response rows with a learner code, an item code, and right or wrong
- For the local engine: code execution enabled on the agent host

## How to get credentials

No keys or secrets are needed. When you create a connection, you are redirected to sign in with your Microsoft account and approve access. If the connection later expires, sign in again from the connection page.

## Get started with your connector

1. In Copilot Studio, open your agent and go to **Tools**, then **Add a tool**.
2. Search for **Test Analytics & Psychometrics** and select it.
3. Create a connection and sign in.
4. Add this to the agent's instructions: "For any question about item quality, test reliability, learner ability, the next question, or who needs practice, call the Test Analytics tool. Do not calculate scores yourself. Use learner codes only. Do not rank learners or publish a scoreboard. Quote each ability with its standard error."
5. Provide response rows. Add a skill column or an item bank when you need mastery or the next question.
6. Ask in plain language, for example "Which questions are not measuring ability?"

Treat results as decision support. Educators decide what to drop, reteach, or record.

## For administrators

Enabling this connector gives the organisation one approved way to read whether a test measures ability, how reliable it is, and who needs practice.

**Why this is an administrator decision.** Without a calculation tool, makers ask an agent to score a test from a spreadsheet. A language model can give a different figure on each run, and it can turn learners into a ranked list. A governed connector uses one method and keeps the support list as a check-in.

- **You choose who can connect it.** Tenant and environment permissions apply.
- **You keep your system of record.** The connector recommends and explains. It does not write scores back to any system.
- **You can check the number.** Cards can be rerun on the same file. Each ability includes a standard error. Thin samples are reported.
- **You keep identity out of the request.** Learner codes only. Names, emails, and phones are dropped if present.
- **You do not publish a ranking.** The support list is for the teacher.
- **You control where data goes.** Turn off the hosted tool per agent to keep response rows in your environment.
- **You set data policy as usual.** Place the connector in the Business, Non-business, or Blocked group.
- **There are no secrets to rotate.** Expired connections are renewed by signing in again.

**Review checklist**

- Confirm exports use learner codes, not names
- Decide whether the hosted tool is allowed, or local engine only
- Assign the connector to the correct data policy group
- Review the privacy policy and terms, including use with minors where that applies
- Read the self-assessments under Security and compliance, once they are published
- Review the local engine script if your policy requires it

## For developers

A usable row has a learner code, an item code, and whether the answer was right. The schema does not mark those properties required, so a thin file still calls. The card reports what was missing.

### Response fields

| Field | Meaning | Needed for |
| --- | --- | --- |
| `learner_id` | Learner code. Aliases: `student_id`, `user_id`, `learner`, `student` | Every person-level card |
| `item_id` | Question code. Aliases: `question_id`, `item`, `question` | Item analysis and the next question |
| `correct` | Right or wrong. `1`/`0`, true/false, yes/no, or correct/incorrect. Aliases: `score`, `is_correct`, `response` | Every calculation. Partial credit is not scored |
| `skill` | Skill or topic. Aliases: `skill_id`, `topic`, `topic_id` | Mastery, when `learning_history` is not sent |

### Other inputs

| Input | Meaning |
| --- | --- |
| `learning_history` | Attempt log for mastery, in order. When sent, mastery is traced from here instead of from `responses`. Up to 5,000 rows |
| `item_bank` | Unused questions. `difficulty` default 0, `discrimination` default 1, `guessing` default 0.2 and must be from 0 to 0.5 |
| `student_ability` | Ability to use for the next item when you are not taking it from the file |
| `focus_learner` | Learner code whose ability should drive the next item |
| `asked_items` | Item codes already given. Those items are not offered again |
| `min_responses` | Fewest scored answers before a Rasch fit. Default 20 |

On the local engine only, mastery defaults are `prior` 0.1, `learn` 0.1, `slip` 0.1, and `guess` 0.2. `data_path` defaults to `responses.csv`. `data_kind` is `responses` or `learning_history`.

### Sample request

```json
{
  "responses": [
    { "learner_id": "L-01", "item_id": "Q1", "correct": 1, "skill": "algebra" },
    { "learner_id": "L-01", "item_id": "Q2", "correct": 0, "skill": "algebra" },
    { "learner_id": "L-02", "item_id": "Q1", "correct": 1, "skill": "algebra" },
    { "learner_id": "L-02", "item_id": "Q2", "correct": 1, "skill": "algebra" }
  ],
  "min_responses": 20
}
```

This sample is below the Rasch minimum. Ability should stay a proportion correct, and the card should say to add data.

### Sample response (shortened)

<!-- TODO Saifs AI: replace this note with a shortened response from a real call. Include a thin-data case so the refusal is visible. Do not publish invented abilities. -->

A live shortened response is not included in this draft. Capture one call before you publish the page. Quote ability only with its standard error.

### Response sections

| Section | Use it for |
| --- | --- |
| Item flags | Questions that are not measuring ability |
| Reliability | Cronbach's alpha |
| Ability | Each learner's estimate and its standard error, or a proportion correct when the fit is refused |
| Mastery | Bayesian mastery by skill |
| Next question | The item to ask next, excluding `asked_items` |
| Check-in list | Learners a teacher should look in on. Not a rank |
| Data notes | Dropped identity fields and missing inputs |

## Known issues and limitations

- **Decision support only.** It does not write scores, flags, or mastery to any system.
- **Not a ranking.** Do not publish a scoreboard or shame a learner.
- **Learner codes only.** Names, emails, and phones are dropped.
- **Partial credit is not scored.** Each answer is right or wrong.
- **Thin samples.** Below 20 scored answers, 5 learners, and 3 non-extreme items, ability stays a proportion correct.
- **Read the standard error.** A small sample is a weak estimate.
- **Next question.** Needs a bank or the questions just analysed. Guessing on a bank item must be from 0 to 0.5.
- **Hosted tool size.** About 5,000 rows for responses, learning history, and the item bank.
- **Local engine.** Standard library only. Run the script unchanged.
- **Regions.** Not available in US Government (GCC, GCC High, DoD) or China (21Vianet) clouds.

<!-- TODO Saifs AI: confirm the excluded-cloud sentence for this offer. -->

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| The tool cannot run, or asks you to sign in again | The connection has expired | Open the connection and sign in again |
| Ability is a proportion correct | The Rasch minimum was not met | Add scored answers, learners, or items that are not all right or all wrong |
| No mastery | No skill column and no learning history | Add one of them |
| The next question repeats an item | That code was not in `asked_items` | Pass the codes already given |
| Names appear to have been removed | Identity fields were in the file | Use codes only. The card says what was dropped |
| The agent ranks learners | Instructions do not forbid it | Add the instruction in Get started |
| The local engine does not run | Code execution is not enabled | Enable it, or use the hosted tool under 5,000 rows |
| Not permitted, or a subscribe link | The account cannot call the tool | Paste the full Marketplace link exactly as returned |

## FAQ

**Do I need a specialist to use this?**
No. A user asks in plain language. The models run inside the tool. Educators still decide what to change.

**What is the least data I need?**
A learner code, an item code, and whether the answer was right.

**Does my data leave my environment?**
Not with the local engine. With the hosted tool, the rows in the call are sent for calculation. You can turn the hosted tool off.

**Will it change anything in my systems?**
No. It does not write scores, flags, or mastery to an LMS, a quiz tool, or Dataverse.

**Our quiz system already has an MCP server. Why add this?**
That server gives the agent the response records. This connector adds the item, reliability, ability, and next-question calculation.

**Why do I get the same answer every time?**
The same rows and settings return the same cards. The agent's wording may vary.

**What happens if my data is incomplete?**
The tool still runs. A Rasch ability is withheld until the minimums are met, and the card says to add data.

**Can I check how a result was produced?**
Yes. Each ability carries a standard error, and thin-data refusals are stated on the card.

**How large a file can it handle?**
About 5,000 rows on the hosted tool. Larger files use the local engine.

**Can it run on a schedule?**
Yes, from a scheduled flow. The connector itself runs on request.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment). Microsoft Premium connector licensing applies separately.

## Support

**Test Analytics & Psychometrics - Saifs AI** is published by Saifs AI.

- Website: [https://saifs.ai](https://saifs.ai)
- Learn more: [https://saifs.ai/test-analytics-psychometrics](https://saifs.ai/test-analytics-psychometrics)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Privacy: [https://saifs.ai/privacy_policy/test-analytics-&-psychometrics](https://saifs.ai/privacy_policy/test-analytics-&-psychometrics)
- Terms: [https://saifs.ai/eula](https://saifs.ai/eula)
- Sign-in: Microsoft single sign-on (OAuth 2.0). No API keys.
- MCP endpoint: `https://assessment-intelligence-mcp-microsoft.saifs.ai/mcp`

<!-- OPEN QUESTIONS
1. Confirm the marketplace offer URL.
2. Confirm hosted-tool retention and the audit log.
3. Capture a shortened live response, including the thin-data refusal.
4. Hosted send and read tokens are measured on generated JSON. The local-engine script is still not tokenized. A live result file would replace the representative ability and item records.
5. Confirm excluded clouds.
6. Confirm whether mastery defaults (prior, learn, slip, guess) apply only on the local engine, as the schema places them on assessment_intelligence_get_engine.
7. Publisher Attestation and STAR rows stay commented until completed and linked.
-->
