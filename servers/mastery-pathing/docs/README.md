# Learning Path Generator - Saifs AI

**Learning Path Generator - Saifs AI** helps education and training teams decide **what this learner should study next**. You send a topic map and answer rows. It returns each learner's mastery, the topics they are ready for, gaps that block a goal, and a learning path to that goal.

It is built for adaptive learning teams. That includes adaptive-learning platforms, intelligent tutoring systems, test-prep teams, corporate skills programs, and universities. They need personalized learning: a repeatable next topic for each learner, from their own curriculum and their own answers.

**Publisher:** Saifs AI
**Website:** [https://saifs.ai](https://saifs.ai)
**Learn more:** [https://saifs.ai/learning-path-generator](https://saifs.ai/learning-path-generator)
**Pricing:** [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
**Contact and help:** [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
**Privacy:** [https://saifs.ai/privacy_policy/learning-path-generator](https://saifs.ai/privacy_policy/learning-path-generator)
**Terms:** [https://saifs.ai/eula](https://saifs.ai/eula)
**Sign-in:** Microsoft single sign-on (OAuth 2.0). No API keys.

| If you are | Start with |
| --- | --- |
| A business owner, product lead, or L&D lead | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [Where the data comes from](#where-the-data-comes-from) |
| An administrator or security reviewer | [Data handling and security](#data-handling-and-security), [Security and compliance](#security-and-compliance), [Responsible use](#responsible-use), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [Where the data comes from](#where-the-data-comes-from), [For developers](#for-developers) |
| A curriculum lead checking the method | [Inside the calculation](#inside-the-calculation) |

## The problem it solves

Most courses still move every learner down the same list. A fixed order is wrong for part of the class. Learners who missed a prerequisite are pushed ahead. Learners who are ready wait.

Teams often pick the next lesson from a score, or from how many items were wrong. The map and the answers usually sit in a spreadsheet or an export. That is not mastery learning, and it is not personalized learning.

A spreadsheet does not run adaptive learning. Each learner still needs their own next topic.

Learning Path Generator makes that next-topic decision available to any AI agent:

- **What this learner should study next**: the next topic, and why it is next.
- **What they are ready for**: topics whose prerequisites are mastered, and which are not mastered yet. This is the knowledge frontier.
- **How well each topic is known**: a mastery figure for each learner and each topic.
- **What blocks a goal**: the gaps on the way to a target topic, and a learning path that skips what is already mastered.
- **Whether the map is valid**: loops, missing prerequisites, redundant links, and a teaching order that respects the links.
- **When to review first**: a learner who is stuck is sent back before a new topic.

## Why choose it: the same next topic every time

Learning Path Generator is deterministic. The same topic map, the same answers, and the same settings produce the same mastery figures, the same frontier, and the same next topic.

The figures come from established learning models. They are not written afresh by a language model each time you ask.

The same file supports mastery learning. A learner moves on when a topic is known, and stays when it is not. This is adaptive learning from the prerequisite graph, not a new sequence each time.

That matters because a teacher, a coach, or a product team acts on the next lesson. They need a recommendation they can rerun and compare after the next set of answers. The agent's wording around the figures may vary. The figures do not.

| What it does | Established method |
| --- | --- |
| Names topics a learner is ready for, and the next one | Knowledge Space Theory |
| Estimates mastery of each topic from right and wrong answers | Bayesian Knowledge Tracing (BKT) |
| Rates the chance of a correct answer, and question difficulty, on one scale | Elo rating |
| Orders topics so each one follows its prerequisites | Prerequisite graph |
| Checks the map against the answers | Curriculum checks on the answer log |

**It does not guess a fitted model from a thin log.** When a topic has too few answers to fit, the tool uses its standard settings. It can use fitted knowledge tracing, those standard settings, or Elo-only mastery when you set `method` to `rating`.

**It recommends and does not act.** It never enrolls a learner, posts a grade, or writes to an LMS, a gradebook, or Dataverse. It does not rank learners, and it does not label a learner as weak or strong.

## Who should use this

| Role | How it helps |
| --- | --- |
| **Product and curriculum leads** at adaptive-learning platforms | Personalized learning for each learner, from the platform's own topic map and answer log |
| **Academic leads** at test-prep and coaching institutes | Gaps that block a syllabus goal, and a learning path that skips mastered topics |
| **Corporate L&D and skills-training leads** | A path from an employee's answers to a role skill they still need |
| **University course teams** building adaptive courses | A teaching order that respects prerequisites, and who is ready for the next topic |
| **Builders of intelligent tutoring systems** | Mastery and a frontier, without maintaining the models in-house |
| **Assessment and item writers** | Question difficulty on the same scale as the learner |
| **Owners of an education product** | One method for the next topic, the gaps, and the path, across every agent |
| **Agent makers and developers** | A tested calculation they can call, instead of asking a language model to sequence the course |

**Best fit:** adaptive learning and mastery learning teams. That includes adaptive-learning platforms, intelligent tutoring systems, test-prep teams, corporate skills programs, and universities. A topic list and an answer export are enough to start.

**Not required:** a learning-science team, a custom pathing module, or an LMS planning feature. A topic list is enough to check the map. Answer rows make the next topic specific to a learner.

## Use cases

Each use case below can be built as an agent. The agent types are listed in [Agents you can build](#agents-you-can-build).

**1. The next topic for one learner**
Scenario: a platform knows which questions a learner got right, and which topics depend on which.
Outcome: personalized learning for that learner. You get the next topic, why it is next, and the other topics that are ready now.
Ask: "What should this learner study next?"

**2. A path to a goal**
Scenario: the learner needs one target topic, such as a chapter or a job skill.
Outcome: a mastery learning path from what is already known to that goal, with time when topics include minutes.
Ask: "Path to master fractions."

**3. Gaps that block a goal**
Scenario: a test-prep syllabus has a target topic, and earlier topics are uneven.
Outcome: the missing prerequisites, so remediation starts at the block rather than at chapter 1.
Ask: "Find gaps blocking algebra."

**4. Check the topic map**
Scenario: a curriculum team authored a prerequisite graph and is not sure the map is valid.
Outcome: loops with their path, undefined prerequisites, redundant links, levels, and a valid teaching order.
Ask: "Does this prerequisite graph have any loops?"

**5. A learner who is stuck**
Scenario: a learner keeps answering the same topic and is not moving on.
Outcome: review first, before a new topic. `stuck_attempts` defaults to 10.
Ask: "This learner is stuck. What should they review?"

**6. Question difficulty**
Scenario: item writers need to know which questions are hard, on the same scale as the learners.
Outcome: an Elo rating for the chance of a correct answer, and a difficulty for each question.
Ask: "How hard are these questions for this group?"

**7. Curriculum checks from the answers**
Scenario: the map and the answers disagree, or a topic behaves like two skills.
Outcome: topics that split, practice with no learning gain, and prerequisite links the data contradicts or suggests. `model_check` defaults to on.
Ask: "Which prerequisite links do the answers contradict?"

**8. A named group of learners**
Scenario: a coach wants a plan for one class, while the wider log should still set the topic parameters.
Outcome: plans for the learner codes you list. Other learners still inform the topic settings.
Ask: "Plan next topics for these learner codes only."

**9. Large answer logs**
Scenario: more than 5,000 answers, or more than 2,000 topics.
Outcome: the same calculation runs in the agent's own sandbox through the local engine. The rows stay there.
Ask: "Plan next topics for this full answer export."

## Agents you can build

Each agent is one agent in your agent host with this connector added as a tool. The last column shows what else to add.

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Next-topic agent | Answers what this learner should study next, as personalized learning, and why | A topic file and an answer file, or a source from [Where the data comes from](#where-the-data-comes-from) | 1 |
| Gap and path agent | Lists gaps that block a goal, and a learning path to it | The same data, plus a goal topic | 2, 3 |
| Map-check agent | Reports loops, missing links, and a valid teaching order | The topic map | 4 |
| Review-first agent | Sends a stuck learner back to review before a new topic | The answer log | 5 |
| Item-difficulty agent | Rates questions and the chance of a correct answer | Answer rows that include a question id | 6 |
| Curriculum-check agent | Flags topics that behave like two skills, and links the data contradicts | The topic map and the answer log | 7 |
| Class-plan agent | Plans only the learner codes you name | A list of learner codes | 8 |
| Large-log agent | Runs the local engine on a full export | Code execution on the agent host | 9 |
| LMS path agent | Reads topics and answers from the learning system, then returns a path for a person to approve | The learning system's own MCP server, or a file export | 1, 2 |
| Scheduled path agent | Sends a daily next-topic list without anyone asking | A Power Automate flow with a recurrence trigger, and email or Teams | 1 |

The tools run on request. Scheduling comes from the flow, not from the connector.

## At a glance

| Item | Detail |
| --- | --- |
| Tools | `mastery_pathing` (hosted calculation) and `mastery_pathing_get_engine` (local engine) |
| Minimum input | No field is required by the schema. A useful call sends `topics` and `responses`. |
| Hosted tool size | Up to 2,000 topics, 10,000 prerequisite links, 5,000 answers, and 500 learner codes in `learners` |
| Larger files | Local engine. It runs in your agent's code sandbox and needs only `numpy`. |
| Writes to your systems | None |
| Engine version | Returned with the result. The exact field name is not confirmed on this page. |

<!-- TODO Saifs AI: confirm the response field that carries the engine version, and paste it here. -->

## Where the data comes from

Learning Path Generator does not connect to your learning system. The agent brings the rows to it. Any source that can supply a topic map and answer rows can feed it.

| Data source | How the agent gets the data | Best route |
| --- | --- | --- |
| Spreadsheet or CSV export from an LMS, quiz tool, or assessment platform (for example Canvas, Moodle, Blackboard, Google Classroom) | The user uploads the file to the agent | Local engine for large files, hosted tool for small ones |
| A learning system that has its own MCP server | The agent reads topics and answer rows, then passes them on | Hosted tool up to 5,000 answers and 2,000 topics. Above that, export a file and use the local engine. |
| Dataverse tables | The Dataverse MCP server or the Dataverse connector | Same as above |
| SQL database, Excel workbook online, or SharePoint list | A Power Platform connector on the same agent or flow | Same as above |
| A scheduled flow | A Power Automate flow reads the source on a schedule and calls this connector | Hosted tool, within the size limits |
| An earlier run of this connector | Pass stored topic settings in `topic_parameters` | Either |

The connector needs only rows. It has not been certified against each named system. Another tool that returns topic rows and right-or-wrong answer rows can feed it in the same way.

**Works alongside your learning system's MCP server.** That server gives the agent the system's records. This connector adds the next-topic calculation. No planning module is required. The learning system supplies the rows. Learning Path Generator returns the next topic and the path. A person, or the learning system, assigns the work.

**What to pull from the source**

| Data in your system | Field it fills | Why it matters |
| --- | --- | --- |
| Topic or skill list | `topic_id`, `title`, `minutes` | Names each topic and supplies time for the path |
| Prerequisite links | `prerequisites` on the topic, or the `prerequisites` array | Builds the map the frontier and the path depend on |
| Answer log | `learner_id`, `topic`, `question_id`, `correct`, `timestamp` | Updates mastery, difficulty, and the next topic |
| Goal | `goal` | Turns the result into gaps and a path to one target |
| Learner codes to plan for | `learners` | Limits the plans, while other rows can still set topic parameters |
| Stored topic settings | `topic_parameters` | Reuses `start_known`, `learn_rate`, `slip`, and `guess` from an earlier run |

**Field names to send.** The schema names these fields. This page does not list other headers the engine may accept.

<!-- TODO Saifs AI: confirm which export column names the engine matches without renaming, and add them here. -->

| Field | Name to send |
| --- | --- |
| Topic | `topic_id`, `title`, `minutes`, `prerequisites` (`A; B` or a list) |
| Link, if not on the topic | `{from, to}` or `[from, to]` |
| Answer | `learner_id` (a code), `topic`, `question_id`, `correct` (0 or 1), `timestamp` |
| Stored settings | `topic`, `start_known`, `learn_rate`, `slip`, `guess` |

**File formats for the local engine.** CSV, TSV, and JSON. Topics, links, answers, and stored topic settings can be separate files. The tool arguments are `topics_path`, `prerequisites_path`, `responses_path`, and `topic_parameters_path`.

## How it works

The connector offers two routes to the same calculation. The agent picks the route by the size of the data.

| | Hosted tool | Local engine |
| --- | --- | --- |
| Tool name | `mastery_pathing` | `mastery_pathing_get_engine` |
| Use when | Up to 2,000 topics and 5,000 answers | Anything larger, or when learner rows must stay in your environment |
| What the agent sends | Topics, links, answers, and settings | File paths and settings only |
| What comes back | The full result, decision cards first | A ready-to-run Python script |
| Where the calculation runs | Saifs AI endpoint | Your agent's code-execution sandbox |
| Where learner rows go | To the Saifs AI endpoint for calculation | Nowhere. They stay in the sandbox. |
| Requires | Nothing extra | An agent host with code execution, and `numpy` |

**Local engine, step by step**

1. The user uploads or points the agent to a topics file and an answers file.
2. The agent calls `mastery_pathing_get_engine` with the file paths and any settings in `params`.
3. The connector returns the calculation script. Learner rows are not sent on this call.
4. The agent runs the script in its sandbox against the files.
5. The script prints the decision cards and each learner's next topic. It writes the full result to `mastery_plan.json` in the sandbox.
6. The agent reports those decisions to the user.

### Inside the connector

1. The user asks what a learner should study next, or asks for gaps, a path, or a map check.
2. The agent chooses a tool and calls the connector. The call goes to the Saifs AI MCP server over HTTPS at `https://mastery-pathing-mcp-microsoft.saifs.ai/mcp`.
3. The server checks the signed-in account.
4. The hosted tool runs the engine on the rows and returns the result. The engine tool returns the same engine as a script, for the agent to run in its sandbox.
5. The agent reads the result and explains it in plain language. The figures come from the engine. The wording comes from the agent.

### Inside the calculation

Both routes run the same engine. The checks below are the ones the live tool reports. Formula constants are not published on this page, because a signed-in sample of the engine script was not available while this page was written.

<!-- TODO Saifs AI: after one signed-in call to mastery_pathing_get_engine, confirm this order against the script and add any formula the team is willing to publish. -->

| Check | What you get | Method |
| --- | --- | --- |
| 1. Graph | Loops with their path, undefined prerequisites, redundant links, levels, and a valid teaching order | Prerequisite graph |
| 2. Mastery | Each learner's mastery of each topic. Fitted per topic when the log supports it. Standard settings at cold start. | Bayesian Knowledge Tracing (BKT). Set `method` to `rating` for Elo-only mastery. |
| 3. Difficulty | Chance of a correct answer, and question difficulty | Elo rating |
| 4. Frontier | Topics ready to learn: not mastered, and every prerequisite mastered. The next topic, and why. | Knowledge Space Theory |
| 5. Stuck | Review first when a learner is stuck | `stuck_attempts` defaults to 10 |
| 6. Goal | Gaps that block the goal, and a path to it with time | The map plus mastery. Send `goal`. Set `goal_only` to limit recommendations to that path. |
| 7. Foundations | Shaky foundations | Named by the tool. The exact rule is not stated on this page. |
| 8. Curriculum | Topics that behave like two skills, practice with no learning gain, and links the data contradicts or suggests | Answer-log checks. `model_check` defaults to on. `infer_prerequisites` defaults to on. |
| 9. Decisions | Decision cards, returned first. Learners are not ranked. | Wording filled from the calculated figures |

**Why the answer does not change.** The calculation uses fixed methods. The same topics, answers, and settings return the same mastery figures and the same next topic. No random step is documented for this engine. The agent's sentences around the figures can still differ from one reply to the next.

**What happens when data is missing.** `method` defaults to `auto`, which is knowledge tracing, fitted or on standard settings. A topic with a thin log stays on standard settings rather than a fitted model. `mastery_threshold` defaults to 0.95 (allowed 0.5 to 0.999). `rating_mastery_p` defaults to 0.85 (allowed 0.5 to 0.99). `rating_min_attempts` defaults to 3. Curriculum checks use `min_learners_topic` 10 and `min_answers_topic` 50 unless you change them. Without `goal`, you still get mastery and the frontier. You do not get a path to a target.

**Where the working is shown.** The result leads with decision cards. The next topic includes why it was chosen. The exact JSON paths for that working are not confirmed on this page.

<!-- TODO Saifs AI: name the response fields for decision cards, mastery, frontier, gaps, and the path, from a real call. -->

## Data handling and security

| Question | Local engine | Hosted tool |
| --- | --- | --- |
| Does learner data reach Saifs AI? | No. File paths and settings are sent. The rows stay in the sandbox. | Yes. The topics and answers passed in the call are sent. |
| Where is it processed? | Your agent host's code sandbox | `mastery-pathing-mcp-microsoft.saifs.ai`, over HTTPS |
| What happens to the rows after the call? | They never left your sandbox | They are sent for the calculation. A retention period is not stated on this page. |
| What does Saifs AI record? | Not stated on this page, beyond the script request itself | Not stated on this page |

<!-- TODO Saifs AI: confirm retention for the hosted tool (discarded, or kept, and for how long), the hosting region, and what is logged for audit or billing. Do not describe the service as stateless until that is confirmed. -->

**What "sandbox" means.** The sandbox is the code-execution environment of your own agent host, such as Copilot Studio or Azure AI Foundry. It belongs to your tenant, not to Saifs AI. Your host's data policies still apply to it.

**Use the local engine only.** If your policy requires that learner rows never leave your environment, turn off the hosted tool. In Copilot Studio, open the agent's **Tools** tab, select the Learning Path Generator server, turn off **Allow all**, and switch off `mastery_pathing`. Leave `mastery_pathing_get_engine` on. The agent can then use only the local engine. Any tool added to the server later stays off by default. This setting is per agent.

**Sign-in.** Users sign in with Microsoft single sign-on. Saifs AI holds only the account details needed for sign-in and billing. No API key is created, shared, or stored.

**No write access.** The connector has no action that creates, changes, or deletes records in any LMS, gradebook, or Dataverse.

**The script is open to review.** The local engine is a numpy-only Python script. Learner rows are not sent to Saifs AI on this path. Whether the script makes network calls is not stated on this page. Your security team can read the script before allowing it to run.

<!-- TODO Saifs AI: confirm the script makes no network calls, then say so here. -->

## Security and compliance

This section separates what an independent party reviews from what Saifs AI states about itself.

**Reviewed by Microsoft**

| Item | What it covers |
| --- | --- |
| Microsoft connector certification | Microsoft reviews this connector before listing it as a certified connector. The review covers the connector's function and packaging, not Saifs AI's internal systems. |

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
| Learner rows on the local engine | Not sent to Saifs AI. They stay in your sandbox. |
| Learner rows on the hosted tool | Sent for the calculation. Retention is not stated on this page. |
| Learner identity in the payload | A pseudonymous code in `learner_id`. Do not send a name, email, age, or school. |
| Passwords held | None. Sign-in is Microsoft single sign-on. |
| Write access to your systems | None |
| Local engine | Numpy-only Python script, returned for you to run and read |

**Independent audits.** Saifs AI does not yet hold an independently audited certificate such as SOC 2 or ISO 27001. If your review needs a completed security questionnaire, request it through the [contact page](https://saifs.ai/contact-us).

## Responsible use

This connector recommends a next topic for a learner. Learners may be children. Treat the output as study support for a person to act on.

- Send pseudonymous learner codes only. The tool does not need a name, an age, a school, or a roll number.
- Use the result to choose what to study next, or what to review. Do not use it to rank learners, to label a learner, or to build a leaderboard.
- The result does not guarantee a score, a grade, or a time to mastery.
- A person decides what the learner does next. The connector recommends. It does not enroll, assign, or grade.
- Process answer logs only where you are allowed to. A minor's answers are personal data.

## Token usage and cost

With the hosted tool, token use grows with every row the agent sends and every row it reads back. With the local engine, the rows stay in the sandbox. The agent handles a script and the printed decisions, not the full answer log. That is why the local-engine cost does not grow with the file in the same way.

Measured token counts for this connector are not on this page yet. Do not plan a budget from another product's figures.

<!-- TODO Saifs AI: measure with a named tokenizer on three files (a small log, the hosted limit of 5,000 answers, and a file above that limit). Record tokens and a dated reference price for input and output. State the file size where the hosted tool uses fewer tokens. Include the script cost on the first local run and the later-run cost. -->

**Which route to choose, from the size limits**

- **Within 2,000 topics and 5,000 answers:** the hosted tool can run the calculation in one call.
- **Above 2,000 topics or 5,000 answers:** the local engine. It is the only route.
- **Learner rows must stay in your environment:** the local engine, with the hosted tool switched off.

**Hosts that bill per action.** Some agent hosts charge a fixed amount for each tool call and do not bill by token. On those hosts, a smaller token count does not reduce that charge. The local engine may use one extra action to run the script. There, choose the local engine for file size and for keeping learner rows in your environment, not for a lower token bill.

## Prerequisites

- A Saifs AI account, created by signing in with Microsoft single sign-on
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Access to Copilot Studio, Microsoft 365 Copilot, Azure AI Foundry, Power Automate, Power Apps, or Azure Logic Apps
- A topic list. Answer rows, a goal topic, and minutes per topic give a fuller learning path.
- For the local engine: code execution enabled on the agent host

## How to get credentials

No keys or secrets are needed. When you create a connection, you are redirected to sign in with your Microsoft account and approve access. If the connection later expires, sign in again from the connection page.

## Get started with your connector

1. In Copilot Studio, open your agent and go to **Tools**, then **Add a tool**.
2. Search for **Learning Path Generator** and select it.
3. Create a connection and sign in.
4. Add this to the agent's instructions: "For the next topic, mastery, gaps, or a learning path, call Learning Path Generator. Do not sequence topics yourself. Send learner codes, not names."
5. Provide a topic map and, when you have it, an answer log. Use pseudonymous learner codes.
6. Ask in plain language, for example "What should this learner study next?" or "Find gaps blocking algebra."

Treat results as study support. A person chooses the next assignment. The connector does not enroll the learner or post a grade.

## For administrators

Enabling this connector gives the organization one approved way to run adaptive learning. Makers can answer what this learner should study next inside the agents they already build.

**Why this is an administrator decision.** Without a calculation tool, makers ask an agent to pick the next topic from a spreadsheet. A language model can give a different sequence on each run, and the result cannot be rerun or compared after new answers. A governed connector replaces that with one method for every agent and team.

- **You choose who can connect it.** Tenant and environment permissions apply. Only users who can create a connection can call the tools.
- **You keep your system of record.** The connector recommends and explains. It does not enroll learners, post grades, or write back to any system.
- **You can check the result.** The same file and settings can be rerun. The next topic includes why it was chosen. Thin logs stay on standard settings rather than a fitted model.
- **You control where learner rows go.** Turn off the hosted tool per agent to keep those rows in your environment.
- **You keep identity out of the request.** Learner ids must be codes. Names, emails, ages, and school details do not belong in the payload.
- **You set data policy as usual.** Place the connector in the Business, Non-business, or Blocked group with Power Platform data loss prevention policies.
- **There are no secrets to rotate.** Expired connections are renewed by signing in again.

**Review checklist**

- Decide whether the hosted tool is allowed, or local engine only
- Confirm that makers will send pseudonymous learner codes
- Assign the connector to the correct data policy group
- Review the privacy policy and terms
- Read the self-assessments listed under Security and compliance, once they are published
- Review the local engine script if your policy requires it

## For developers

The schema marks no field as required. Send the columns the business has. `topics` and `responses` are what make a learning path specific to a learner.

### Topic fields

| Field | Meaning | Needed for |
| --- | --- | --- |
| `topics` | Up to 2,000 topics. Each item is an object or a string. | The map, the frontier, and the path |
| `topic_id` | Topic code, inside a topic object | Every later reference |
| `title` | Label | Readable output |
| `minutes` | Time for the topic | Time on the path to a goal |
| `prerequisites` | On a topic: `A; B` or a list of topic codes | The frontier and the teaching order |
| `prerequisites` (array) | Optional links instead of a column. Each item is `{from, to}` or `[from, to]`. Up to 10,000. | The same map, when links live in their own file |

### Answer fields

| Field | Meaning | Needed for |
| --- | --- | --- |
| `responses` | One object per answer. Up to 5,000 on the hosted tool. | Mastery, difficulty, frontier, and curriculum checks |
| `learner_id` | Pseudonymous code. Not a name. | Per-learner mastery and the next topic |
| `topic` | Topic code the answer belongs to | Mastery of that topic |
| `question_id` | Question code | Difficulty and the chance of a correct answer |
| `correct` | 0 or 1 | Mastery and Elo updates |
| `timestamp` | When the answer happened | Order of evidence |

### Options and defaults

| Option | Default | Purpose |
| --- | --- | --- |
| `goal` | None | Target topic for gaps and the path |
| `learners` | None. Up to 500 codes. | Plan these codes only. Other rows can still set topic parameters. |
| `method` | `auto` | `auto` or `bkt`: knowledge tracing, fitted or standard. `rating`: Elo-only mastery. |
| `mastery_threshold` | 0.95 | Allowed 0.5 to 0.999 |
| `rating_mastery_p` | 0.85 | Allowed 0.5 to 0.99. Used when mastery is read from the Elo rating. |
| `rating_min_attempts` | 3 | Minimum attempts setting for the rating route |
| `min_learners_topic` | 10 | Minimum learners before a topic-level check |
| `min_answers_topic` | 50 | Minimum answers before a topic-level check |
| `slip_guess_cap` | 0.5 | Allowed 0.05 to 1 |
| `infer_prerequisites` | true | Suggest prerequisite links from the answers |
| `goal_only` | false | When true, recommend only topics on the way to the goal |
| `model_check` | true | Run the curriculum checks |
| `max_split_tests` | 10 | Cap on split tests in the curriculum check |
| `split_bic_margin` | 10 | Margin for a topic-split check |
| `min_learners_edge` | 10 | Minimum learners before a link is judged |
| `edge_violation_rate` | 0.3 | Allowed 0.05 to 0.9 |
| `max_suggested_links` | 10 | Cap on suggested prerequisite links |
| `max_learners_listed` | 200 | Cap on learners listed in the result |
| `max_ready_listed` | 5 | Cap on ready topics listed |
| `max_questions_listed` | 200 | Cap on questions listed |
| `stuck_attempts` | 10 | Attempts before review-first |
| `topic_parameters` | None. Up to 2,000. | Stored settings from an earlier run: `topic`, `start_known`, `learn_rate`, `slip`, `guess` |

### Local engine arguments

| Field | Meaning |
| --- | --- |
| `topics_path` | Topics file in the sandbox (CSV, TSV, or JSON) |
| `prerequisites_path` | Optional links file (`from`, `to`) |
| `responses_path` | Answers file |
| `topic_parameters_path` | Optional stored topic settings |
| `params` | The same options as `mastery_pathing`, including `goal` and `learners` |

### Sample request

```json
{
  "topics": [
    {"topic_id": "frac_basics", "title": "Fraction basics", "minutes": 40},
    {"topic_id": "frac_add", "title": "Adding fractions", "minutes": 45, "prerequisites": "frac_basics"}
  ],
  "responses": [
    {"learner_id": "L-014", "topic": "frac_basics", "question_id": "q12", "correct": 1, "timestamp": "2026-11-16T10:02:00Z"},
    {"learner_id": "L-014", "topic": "frac_basics", "question_id": "q15", "correct": 0, "timestamp": "2026-11-16T10:05:00Z"}
  ],
  "goal": "frac_add",
  "learners": ["L-014"],
  "mastery_threshold": 0.95
}
```

### Sample response (shortened)

A signed-in call is required to capture a response. An unauthenticated call to the endpoint returns HTTP 401. This page does not invent response fields.

<!-- TODO Saifs AI: paste a shortened real response from one call to mastery_pathing with the sample request above. Keep the field names exactly as returned. -->

### Response sections

These are the parts the live tool says it returns. JSON field names are not confirmed.

| Section | Use it for |
| --- | --- |
| Decision cards | The actions to read first |
| Graph checks | Loops and their path, undefined prerequisites, redundant links, levels, and a valid teaching order |
| Mastery | Each learner's mastery of each topic, from BKT or from Elo when `method` is `rating` |
| Frontier and next topic | Topics ready to learn, the next topic, and why |
| Review first | The recommendation when a learner is stuck |
| Gaps and path | What blocks the goal, and the path to it with time |
| Shaky foundations | Foundations the tool flags |
| Curriculum checks | Topics that behave like two skills, practice with no gain, and links the data contradicts or suggests |
| `mastery_plan.json` | The full result when the local engine runs. The hosted tool returns the result in the call. |

## Known issues and limitations

- **Study support only.** It does not enroll a learner, post a grade, or update any system.
- **No ranking.** Learners are not ranked, and the result is not a leaderboard.
- **No outcome promise.** Mastery learning support is not a guarantee of a score or of time to mastery.
- **Identity.** Send codes only. Do not send names, emails, ages, schools, or roll numbers.
- **Hosted tool size.** 2,000 topics, 10,000 prerequisite links, and 5,000 answers per call. Use the local engine for more.
- **Listing caps.** Defaults list up to 200 learners, 5 ready topics, and 200 questions. Raise `max_learners_listed`, `max_ready_listed`, or `max_questions_listed` to list more.
- **Thin history.** Cold start uses standard settings. A fitted model is used when the topic has enough answers.
- **Goal.** Without `goal`, there is no path to a target topic.
- **Curriculum IP.** The topic map is yours. The connector calculates over the rows you send. It is not a content library.
- **Retention.** How long the hosted tool keeps rows after the call is not stated on this page. Use the local engine if rows must stay in your environment.
- **Regions.** Not available in US Government (GCC, GCC High, DoD) or China (21Vianet) clouds.

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| The tool cannot run, or asks you to sign in again | The connection is missing or has expired | Open the connection and sign in again |
| The hosted call is rejected for size | More than 5,000 answers, 2,000 topics, or 10,000 links | Call `mastery_pathing_get_engine` and run the script in the sandbox |
| The agent names a next topic without calling the tool | Agent instructions do not require the tool | Add the instruction in Get started |
| The local engine does not run | Code execution is off, or `numpy` is missing | Enable code execution. The script needs `numpy`. |
| There is mastery, but no path to a goal | `goal` was not sent | Send the target topic id |
| A learner appears under a real name | The source export used a name as the id | Replace it with a code before the call |

## FAQ

**Do I need a learning scientist to use this?**
No. A user asks in plain language. The models run inside the tool. A topic list and an answer export are enough to start.

**What is the least data I need?**
A topic list, to check the prerequisite graph. Add answer rows to get a learner's mastery and what they should study next. Add `goal` to get gaps and a learning path.

**Does my data leave my environment?**
Not with the local engine. With the hosted tool, the rows in the call are sent to Saifs AI for the calculation. You can turn the hosted tool off.

**Will it change anything in my learning system?**
No. It recommends a next topic and a path. It does not enroll, grade, or write back.

**Our LMS already has an MCP server. Why add this?**
That server gives the agent your learning system's records. This connector gives the agent the next-topic calculation, and it works on rows from any source.

**What happens if my data is incomplete?**
The tool still runs. Thin topics stay on standard settings. Without a goal, you get mastery and the frontier, not a path to a target.

**Can I check how a result was produced?**
The next topic includes why it was chosen, and decision cards come first. The same input and settings return the same figures.

**How large a file can it handle?**
The hosted tool accepts up to 5,000 answers and 2,000 topics. Larger files use the local engine.

**Can it run on a schedule?**
Yes, by calling it from a scheduled Power Automate flow. The connector itself does not run on a timer.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment). Microsoft Premium connector licensing applies separately.

## Support

**Learning Path Generator - Saifs AI** is published by Saifs AI.

- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Learn more: [https://saifs.ai/learning-path-generator](https://saifs.ai/learning-path-generator)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Privacy: [https://saifs.ai/privacy_policy/learning-path-generator](https://saifs.ai/privacy_policy/learning-path-generator)
- Terms: [https://saifs.ai/eula](https://saifs.ai/eula)
