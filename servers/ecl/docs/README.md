# ECL Calculator: IFRS 9/Ind AS - Saifs AI

**ECL Calculator: IFRS 9/Ind AS - Saifs AI** helps credit, finance, and controllership teams decide **what the expected credit loss is this quarter, and why it changed**. You send a loan book or a trade-receivable ageing, plus history and your own assumptions. It returns a stage per exposure, 12-month and lifetime expected credit loss, any floors you supplied, and a bridge from the previous quarter.

It is built for lenders and Ind AS companies that still build the quarterly provision in spreadsheets, and that need the same ECL calculation when they run the file again.

**In plain language:** you give it the book, or the receivables, and the rules you already use. It returns the provision for the quarter and the reason it moved.

**For technical readers:** the ECL calculation uses the IFRS 9 and Ind AS 109 three-stage impairment model, Markov roll-rate probability of default, workout loss given default, a credit conversion factor, probability-weighted scenarios, and a provision matrix. Lifetime loss is the CECL-style lifetime loss named on the tool.

It recommends a provision. It does not post one. It does not decide an individual loan. It does not compute interest or an effective interest rate. It is not an accounting opinion, a regulatory sign-off, or financial or legal advice.

**Publisher:** Saifs AI
**Website:** [https://saifs.ai](https://saifs.ai)
**Pricing:** [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
**Contact and help:** [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
**Privacy:** [https://saifs.ai/privacy_policy/ecl-calculator-ifrs-9-ind-as](https://saifs.ai/privacy_policy/ecl-calculator-ifrs-9-ind-as)
**Terms:** [https://saifs.ai/eula](https://saifs.ai/eula)
**Sign-in:** Microsoft single sign-on (OAuth 2.0). No API keys.
**MCP endpoint:** [https://ecl-mcp-microsoft.saifs.ai/mcp](https://ecl-mcp-microsoft.saifs.ai/mcp)

<!-- TODO Saifs AI: publish the product page, then add a Learn more line here and in Support. https://saifs.ai/ecl-calculator, https://saifs.ai/ecl-calculator-ifrs-9-ind-as, and https://saifs.ai/ecl-calculator-ifrs-9-ind-as-109 each returned HTTP 404 on 24 November 2026. Do not link them until one opens. -->

| If you are | Start with |
| --- | --- |
| A business owner, CFO, or credit and finance lead | [The problem it solves](#the-problem-it-solves), [Who should use this](#who-should-use-this), [Use cases](#use-cases) |
| An agent maker deciding what to build | [Agents you can build](#agents-you-can-build), [Where the data comes from](#where-the-data-comes-from) |
| An administrator or security reviewer | [Data handling and security](#data-handling-and-security), [Security and compliance](#security-and-compliance), [For administrators](#for-administrators) |
| A maker or developer | [How it works](#how-it-works), [Where the data comes from](#where-the-data-comes-from), [For developers](#for-developers) |
| A finance or audit reviewer checking the method | [Inside the calculation](#inside-the-calculation), [Responsible use](#responsible-use) |

## The problem it solves

Lenders and Ind AS companies must provision for expected credit loss, not only for losses already booked. The work is quarterly. An auditor will ask why the number moved. Many teams still do that IFRS 9 impairment work in a workbook that changes owner each quarter.

A fixed provision rate is wrong for part of the book. Stage rules, roll rates, and scenario weights sit in different sheets. A language model asked for the quarter's loan loss provisioning can return a different figure on the next run. That figure is not something an auditor can rerun.

ECL Calculator: IFRS 9/Ind AS makes that ECL calculation available to any AI agent:

- **What the expected credit loss is this quarter:** 12-month and lifetime expected credit loss by exposure, segment, and stage.
- **Why it changed:** a bridge from the previous run you send, including write-offs you list.
- **Which stage, and why:** stage 1, stage 2, or stage 3, with the trigger that put the exposure there.
- **What the floor does:** the dated floor table you supply, applied on top of the model figure.
- **What the trade receivables need:** a provision matrix from ageing and loss history, for the simplified IFRS 9 impairment approach.

## Why choose it: the same provision every time

The same book and the same settings are the same request. The hosted tool is marked idempotent. The ECL calculation comes from fixed credit methods. It is not written afresh by a language model each time you ask.

That matters because finance, the auditor, and the board act on the provision. They need a figure they can rerun and compare with last quarter. The agent's wording around the numbers may vary. The numbers do not.

| What it does | Established method |
| --- | --- |
| Puts each exposure in stage 1, stage 2, or stage 3, and names the trigger | IFRS 9 and Ind AS 109 three-stage impairment model, using the staging rules you send |
| Builds a probability of default term structure from delinquency movement | Markov roll-rate transition matrices, or the PD curves you send |
| Estimates loss given default from recovery cash flows | Workout LGD. Discount factors only if you send them |
| Turns an undrawn limit into exposure at default | Credit conversion factor, from history or from the factor you send |
| Weights base, upside, and downside cases | Probability-weighted scenarios, with the weights you send |
| Applies a minimum provision | A dated floor table you supply. The tool does not embed a regulation |
| Measures trade receivables | Provision matrix: loss rates by ageing bucket, with your forward-looking multiplier |
| Reports lifetime loss as well as 12-month loss | CECL-style lifetime loss, alongside the IFRS 9 and Ind AS 109 stages |

**It does not guess.** If you send no discount factors, the result is undiscounted and labelled. No interest rate is derived. On a regulated run, missing floors are flagged at high priority. `min_history_months` defaults to 24. `min_defaults` defaults to 30. The exact sentence the tool uses when history is thinner than that is not confirmed on this page. See the note under [Inside the calculation](#inside-the-calculation).

**It recommends and does not act.** It never posts a journal, never approves a provision, and never writes to a core-banking system, a loan system, an accounting system, or Dataverse.

## Who should use this

| Role | How it helps |
| --- | --- |
| **CFO and finance teams at NBFCs** | A quarterly loan loss provisioning figure, with the stage of each exposure and a bridge for the board note |
| **CROs and credit-risk teams** | Stage 1, stage 2, and stage 3 from the rules they set, plus PD, LGD, and the scenario weights |
| **Small finance banks, co-operative banks, and mid-size banks** | The same ECL calculation on an export, with a floor table the team supplies and dates |
| **Finance controllers at Ind AS companies** | A provision matrix for trade receivables, by ageing bucket, from the company's own loss history |
| **Statutory auditors and CA firms** | A rerunnable pack: assumptions, stages, and hashes, for the file the client already holds |
| **Audit firms and risk consultants** | One method across several client books, without rebuilding the workbook each quarter |
| **Business owners** | What the expected credit loss is this quarter, and why it changed, without maintaining the formulas |
| **Agent makers and developers** | A tested ECL calculation inside an agent, so the agent does not invent the provision |

**Best fit:** NBFCs, small finance banks, co-operative banks, mid-size banks, and Ind AS companies that measure expected credit loss on loans or on trade receivables each quarter.

**Not required:** an enterprise impairment suite, a model-validation team, or a planning module. An exposure export is enough to start. Roll-rate history, recoveries, and a previous snapshot make the ECL calculation sharper.

## Use cases

Each use case below can be built as an agent. The agent types are listed in [Agents you can build](#agents-you-can-build).

**1. This quarter's loan book**
Scenario: the credit and finance team closes the quarter on a loan export.
Outcome: stage and trigger per exposure, 12-month and lifetime expected credit loss, allowance and provision, and the floors you supplied.
Ask: "Calculate our ECL for this quarter and show the stage transfers."

**2. Why the provision moved**
Scenario: the board asks why loan loss provisioning is higher than last quarter.
Outcome: a bridge from the snapshot you kept, plus the write-offs you list.
Ask: "Why did our ECL increase from last quarter?"

**3. Stage transfers**
Scenario: accounts crossed 30 days past due, or a watch-list flag was set.
Outcome: stage 1, stage 2, or stage 3, with the trigger, using the staging rules you send.
Ask: "Which loans moved from stage 1 to stage 2, and what triggered it?"

**4. Trade receivables**
Scenario: an Ind AS company needs the simplified approach on open invoices.
Outcome: a provision matrix by ageing bucket, from loss history and the forward-looking multiplier you set.
Ask: "Build the IFRS 9 provision matrix for our trade receivables."

**5. Probability of default from history**
Scenario: the team has monthly delinquency movement, not a bought PD curve.
Outcome: 12-month and lifetime PD by segment from roll rates, or from days-past-due snapshots.
Ask: "Fit the PD term structure from our roll-rate history."

**6. Loss given default from workouts**
Scenario: recoveries exist, and the team does not want a single assumed LGD.
Outcome: workout LGD by segment from the cash flows, discounted only with factors you send.
Ask: "Estimate loss given default from these recovery cash flows."

**7. One exposure, step by step**
Scenario: an auditor asks how one facility was measured.
Outcome: the explanation for the `exposure_ref` you name, with the inputs used for that facility.
Ask: "Explain the ECL calculation on exposure L-1001."

**8. The pack for the file**
Scenario: the quarter is done and the working papers need the assumptions.
Outcome: an audit pack with hashes, and a snapshot to keep for the next ECL calculation.
Ask: "Prepare the audit pack for this quarter's expected credit loss."

**9. A full book export**
Scenario: the export is larger than the hosted caps, or it must stay inside the lender's environment.
Outcome: the same engine runs in the agent's sandbox. The loan rows are not sent to Saifs AI on that path.
Ask: "Run the ECL calculation on this full loan-book export."

## Agents you can build

Each agent is one agent in your agent host with this connector added as a tool. The last column shows what else to add.

| Agent type | What it does | Add alongside this connector | Use case |
| --- | --- | --- | --- |
| Quarterly provision agent | Returns this quarter's stages, 12-month and lifetime expected credit loss, and floors | A loan export, or a source from [Where the data comes from](#where-the-data-comes-from) | 1 |
| Movement agent | Explains why the provision changed since the snapshot you kept | Last quarter's snapshot as `previous_run`, and `write_offs` | 2 |
| Staging agent | Assigns stage 1, stage 2, or stage 3 and names the trigger | `staging_rules` if the defaults are not your policy | 3 |
| Trade receivable agent | Builds the provision matrix for trade receivables | Ageing and loss history, or the loss rates you set | 4 |
| PD and LGD agent | Fits probability of default and workout loss given default | Roll-rate or days-past-due history, and recovery cash flows | 5, 6 |
| Single-exposure agent | Explains one facility for a reviewer | `exposure_ref`, plus the book inputs that facility needs | 7 |
| Audit-pack agent | Returns the assumptions, hashes, and the snapshot to store | The same inputs as the quarterly run. A place the caller stores the snapshot | 8 |
| Large-book agent | Runs the local engine on a full export | Code execution on the agent host | 9 |
| Core-banking agent | Reads the book from the system of record, calculates, and hands the figure to a person | That system's own MCP server | 1, 3 |
| Scheduled close agent | Runs the ECL calculation on a recurrence, without anyone asking | A Power Automate flow with a recurrence trigger, and email or Teams. The flow must pass the previous snapshot | 1, 2 |

The tools run on request. Scheduling comes from the flow, not from the connector. The connector does not post the provision.

## At a glance

| Item | Detail |
| --- | --- |
| Tools | `ecl_calculator` (hosted calculation) and `ecl_get_engine` (local engine) |
| Jobs on the hosted tool | `assign_stages`, `audit_pack`, `compute_ecl`, `ecl_movement`, `explain_exposure`, `fit_lgd`, `fit_pd_curves`, `provision_matrix`. Omit `job` and the tool runs what the inputs allow. |
| Minimum input | Nothing is marked required. A loan run needs exposures. A trade-receivable run needs ageing. |
| Hosted tool size | Up to 20,001 exposures. Up to 100,001 days-past-due history rows. Up to 50,001 roll-rate rows. Up to 10,001 recovery rows. Up to 10,001 credit-conversion rows. Up to 20 scenarios. Up to 1,000 floor rows. Up to 20,001 receivable rows. Up to 5,001 loss-history rows. |
| Listed rows | `max_listed` defaults to 200. The range is 1 to 100,000. |
| Larger files | Local engine, for books over 20,000 exposures or 100,000 days-past-due history rows, or when the rows must stay in your environment. It needs `numpy`. |
| Writes to your systems | None. The local engine writes `ecl_result.json` and `ecl_snapshot.json` in the sandbox. Keep the snapshot for the next quarter. |
| Out of scope | Posting a provision. Deciding an individual loan. Interest and effective interest rate. |

<!-- TODO Saifs AI: if a live response includes an engine version field, name it in At a glance. Do not copy metadata.model_version from Inventory Intelligence. -->

## Where the data comes from

ECL Calculator: IFRS 9/Ind AS does not connect to your systems. The agent brings the rows to it, from any source that can supply a loan book or a receivable ageing.

| Data source | How the agent gets the data | Route that suits |
| --- | --- | --- |
| A spreadsheet or CSV export from a core-banking system, a loan origination system, or accounting software (for example Tally) | The user uploads the file to the agent | Local engine for large files, hosted tool inside the caps |
| An ERP or core system that has its own MCP server (for example Dynamics 365 or SAP) | The agent reads facilities, days past due, and limits, then passes the rows on | Hosted tool inside the caps. Above them, export a file and use the local engine. |
| Dataverse tables | The Dataverse MCP server or the Dataverse connector | Same as above |
| A SQL database, an Excel workbook online, or a SharePoint list | A connector added to the same agent or flow | Same as above |
| A scheduled flow | A Power Automate flow reads the source on a schedule and calls this connector | Hosted tool, if the book is inside the caps. Pass the previous snapshot from the flow's own store. |

The connector needs only rows. It has not been certified against each system named above. Any other tool that returns those rows can feed it the same way.

**Works alongside your core system's MCP server.** That server gives the agent the system's records and functions. This connector adds the ECL calculation. The system supplies the book. This connector calculates the provision. A person approves it.

**What to pull from the source**

| Data in your system | Field it fills | Why it matters |
| --- | --- | --- |
| One row per facility: a loan or account reference, a borrower reference, segment, drawn balance, undrawn or limit, days past due, remaining term | `exposures` | The book being measured. References should be pseudonymous. |
| Days past due, SMA, NPA or asset classification, restructured, watch list, rating then and rating now, previous stage, months since cure | Fields on `exposures`, and `staging_rules` | Stage 1, stage 2, or stage 3, and the trigger |
| Monthly transitions, or account-month days-past-due snapshots | `roll_rate_history` or `dpd_history` | Probability of default. The tool counts transitions from consecutive months when you send snapshots. |
| Recovery cash flows after default | `recoveries` | Workout loss given default |
| Limit, drawn before default, and drawn at default | `ccf_history`, or a caller `ccf` | Exposure at default on undrawn limits |
| Your scenarios and a dated floor table | `scenarios`, `floors` | Weights, and the minimum provision. Include `source` and `valid_from` on each floor. |
| Last quarter's snapshot, and write-offs since | `previous_run`, `write_offs` | Why the expected credit loss changed |
| Ageing buckets and historical write-offs | `receivables_ageing`, `loss_history` | The provision matrix for trade receivables |

**Headers can stay as exported.** The tool accepts loan-origination and core-banking headers as they are. `mapping` overrides a header when it was read as the wrong field. This page does not list the header aliases, because the schema does not list them.

<!-- TODO Saifs AI: if the engine maps named headers without renaming, list those headers and the response field that reports the mapping. Until a signed-in call or the engine script shows them, do not publish an alias table. -->

**File formats for the local engine.** CSV, TSV, or JSON. The engine reads the files named in the call: exposures, days-past-due history, roll rates, recoveries, credit-conversion history, receivables, loss history, and the previous run. Save an Excel sheet as CSV first, or ask the agent to convert it in the sandbox. The script prints the decision cards and writes `ecl_result.json` and `ecl_snapshot.json`.

## How it works

The connector offers two routes to the same ECL calculation. The agent picks the route by the size of the data.

| | Hosted tool | Local engine |
| --- | --- | --- |
| Tool name | `ecl_calculator` | `ecl_get_engine` |
| Use when | Inside the caps in [At a glance](#at-a-glance) | Over 20,000 exposures or 100,000 days-past-due history rows, or when the rows must stay in your environment |
| What the agent sends | The rows and the settings | File paths, and other inputs in `params` |
| What comes back | The full result | A ready-to-run Python script. The tool says it is numpy-only. |
| Where the calculation runs | Saifs AI endpoint | Your agent's code-execution sandbox |
| Where your loan or receivable rows go | To the Saifs AI endpoint for the calculation | Nowhere. They stay in the sandbox. The tool states that no data reaches the server on this path. |
| Requires | Nothing extra | An agent host with code execution, and `numpy` |

**Local engine, step by step**

1. The user uploads or points the agent to the exposure file, and to history files when they exist.
2. The agent calls `ecl_get_engine` with those paths and any settings in `params`.
3. The connector returns the calculation script. Save it exactly as given. Do not shorten it or rewrite it.
4. The agent runs the script in its sandbox against the files.
5. The script prints the decision cards and writes `ecl_result.json` and `ecl_snapshot.json` in the sandbox.
6. The agent reports the cards to the user. A person approves the provision. The caller keeps the snapshot for next quarter.

### Inside the connector

1. The user asks what the expected credit loss is this quarter, or why it changed.
2. The agent chooses a tool and calls the connector, which passes the call to the Saifs AI MCP server over HTTPS at `https://ecl-mcp-microsoft.saifs.ai/mcp`.
3. The server checks the signed-in account.
4. The hosted tool runs the ECL calculation on the rows and returns the result. The engine tool returns the same calculation as a script, for the agent to run in its sandbox.
5. The agent reads the result and explains it in plain language. The numbers come from the engine. The wording comes from the agent.

### Inside the calculation

Both routes are the same engine. The tool states what that engine does. A signed-in response was not available, so this page does not name the JSON keys inside the result.

| Step | What the engine does | Method |
| --- | --- | --- |
| 1. Read the book | Takes exposures, history, and receivables as exported. `mapping` can correct a header. `as_of` is the reporting date. | The call |
| 2. Assign the stage | Stage 3 at the days-past-due and NPA rules you set. Stage 2 at the significant-increase rules you set. Stage 1 is the rest. Cure months, if you set them, govern a move back down. | The tool description names `stage3_dpd` 90, `stage2_dpd` 30, `sma_stage2_from` 1, `restructured_stage` 2, `watchlist_stage` 2, and `borrower_contagion` true. Send `staging_rules` when your policy differs. |
| 3. Probability of default | Builds a monthly roll-rate matrix by segment from counts, or from consecutive days-past-due months. Cumulative PD follows from that matrix. Small segments are pooled. Or it uses the `pd_curves` you send. | Markov roll rates. `credibility_k` defaults to 50. Ranges use `pd_draws` (default 300) and `seed` (default 7). |
| 4. Loss given default | From workout cash flows: one minus recoveries divided by exposure at default. Present value uses only discount factors you send. If you send none, the result is undiscounted and labelled. Or it uses the `lgd` you send. | Workout LGD. No rate is derived. |
| 5. Exposure at default | Drawn balance plus a credit conversion factor times the undrawn limit. The factor comes from `ccf_history` or from `ccf`. | `ead_profile` defaults to `constant`. `amortising`, or your own monthly fractions, are the other choices. |
| 6. Expected credit loss | Multiplies marginal probability of default, loss given default, exposure at default, and the discount factor through time. Returns both 12-month and lifetime figures, by exposure, segment, and stage. | IFRS 9 and Ind AS 109. Stage 1 is the 12-month figure. Stage 2 uses remaining life. Stage 3 uses a probability of default of 1. `default_life_months` defaults to 12. |
| 7. Scenarios | Applies the probability weights you send, as a PD multiplier or a matrix shift. | Up to 20 scenarios |
| 8. Floors | Applies the dated floor table you send. `floor_base` defaults to `ead`, as a percent of exposure at default. `drawn` is the other base. | Caller-supplied `min_pct`, `source`, and `valid_from`. On a regulated run, missing floors are flagged at high priority. `regulated` defaults to false. |
| 9. Provision matrix | For trade receivables: a loss rate per ageing bucket, your forward-looking multiplier, and the ECL on the balances. `loss_rates` overrides history when you send them. | Simplified IFRS 9 impairment approach |
| 10. Bridge and pack | Compares the previous snapshot you send with this run, includes write-offs you list, and returns an audit pack with hashes plus a snapshot. | `ecl_movement` and `audit_pack` |
| 11. Decision cards | Puts the plain-language actions first. | Fixed output of every call. `explain_exposure` is the job for one `exposure_ref`. |

**Why the answer does not change.** The hosted tool is marked idempotent. The steps above are a fixed method. No language model writes the provision. The step that draws ranges uses `seed`, which defaults to 7, so the same seed repeats.

**What happens when an input is missing.** Nothing on the schema is required. If `job` is omitted, the tool runs what the inputs allow. No discount factors means undiscounted, and the result is labelled. No floor table means the model figure only. A regulated run flags that gap at high priority. `min_history_months` defaults to 24. `min_defaults` defaults to 30. This page does not quote the refusal text for thin history until a signed-in call shows it.

**Where the working is shown.** Decision cards come first. Then the stage and trigger, 12-month and lifetime expected credit loss, allowance and provision, the scenario table, floors applied, the quarter-on-quarter bridge, explanations, and an audit pack with hashes. This page does not name the JSON keys for those fields. See the sample response note in [For developers](#for-developers).

<!-- TODO Saifs AI: confirm from a signed-in call or the engine script: the stage-1 / stage-2 / stage-3 horizon split stated above; the exact floor comparison; the bridge line names; the refusal text when history is under min_history_months (default 24) or defaults are under min_defaults (default 30); and what default_life_months (default 12) and period_months (default 1) apply to. The schema names the options. It does not define those two. Do not add bridge categories from the research note until the script shows them. -->

## Data handling and security

| Question | Local engine | Hosted tool |
| --- | --- | --- |
| Do loan, recovery, or receivable rows reach Saifs AI? | No. The tool states that no data reaches the server on this path. | Yes. The rows passed in the call are sent for the ECL calculation. |
| Where are they processed? | Your agent host's code sandbox | `ecl-mcp-microsoft.saifs.ai`, over HTTPS |
| What happens to the rows after the call? | They stayed in your sandbox. The script writes `ecl_result.json` and `ecl_snapshot.json` there. You keep the snapshot. | Not stated on the tool. See the note below. |
| What about the snapshot? | The caller keeps `ecl_snapshot.json` for the next quarter. | You pass the previous snapshot back in as `previous_run`. This page does not say the server stores it. |
| What can the script do? | It reads the local files named in the call and writes the two JSON files above. The tool says the script is numpy-only. | Not used on this path. |

<!-- TODO Saifs AI: confirm hosted-tool retention in writing before this sentence is made stronger. Say whether exposure rows, roll rates, recoveries, and receivables are discarded after the request, and whether any billing or audit log keeps a hash, the account, or the rows. Also confirm the hosting region, rate limits, and request timeout. Do not copy the audit-hash sentence from the Inventory Intelligence listing unless this server does the same. The privacy page at https://saifs.ai/privacy_policy/ecl-calculator-ifrs-9-ind-as returned HTTP 200 on 24 November 2026 and does not state portfolio-row retention. It is a general template. It mentions Facebook and Google sign-in, Stripe, and a contact at support@multiplewords.in. The connector package uses Microsoft single sign-on and taher@saifytech.com. Reconcile the page with the package before certification. -->

**What "sandbox" means.** The sandbox is the code-execution environment of your own agent host, such as Copilot Studio or Azure AI Foundry. It belongs to your tenant, not to Saifs AI. Your host's data policies still apply to it.

**Use the local engine only.** If the book must not leave your environment, turn off the hosted tool. In Copilot Studio, open the agent's **Tools** tab, select the ECL Calculator: IFRS 9/Ind AS server, turn off **Allow all**, and switch off `ecl_calculator`. Leave `ecl_get_engine` on. The agent can then use only the local engine. A tool added to the server later stays off until you enable it. This setting is per agent.

**Sign-in.** Users sign in with Microsoft single sign-on. Saifs AI holds only the account details needed for sign-in and billing. No API key is created, shared, or stored.

**No write access.** The connector has no action that creates, changes, or deletes a loan, a receivable, a journal, or a payment. Both tools are marked read-only and not destructive. The local engine is marked open-world because it reads files in the caller's sandbox and writes two JSON files there. It still does not write to a core-banking system.

**The script is returned in full.** Your security team can read it before allowing it to run. It is Python and needs `numpy`. Do not shorten it or rewrite it.

<!-- TODO Saifs AI: confirm whether the local script makes network calls. The tool says no data reaches the server on that path. This page does not claim the script itself makes no network calls. -->

## Security and compliance

This section separates what an independent party has reviewed from what Saifs AI states about itself.

**Reviewed by Microsoft**

| Item | What it covers |
| --- | --- |
| Microsoft connector certification | Microsoft reviews and tests this connector before listing it as a certified connector. The review covers the connector's function and packaging, not Saifs AI's internal systems. |

**Self-assessed by Saifs AI**

Self-assessments are statements Saifs AI makes about its own practices. They are not independent audits.

<!-- PUBLISH EACH ROW ONLY AFTER IT IS COMPLETED. Until then, keep the row inside this comment. Add the public link when you have it.

| Self-assessment | What it is | Where to read it |
| --- | --- | --- |
| Microsoft Publisher Attestation | Saifs AI's answers to Microsoft's questionnaire on security, data handling, and compliance. Microsoft publishes the answers and does not verify them. Renewed every year. | [link to the Microsoft app compliance page for this offer] |
| CSA STAR Level 1 | Saifs AI's completed Cloud Security Alliance questionnaire (CAIQ), published on the public STAR Registry. Updated every year. | [link to the STAR Registry entry] |
| CSA STAR for AI Level 1 | Saifs AI's completed AI questionnaire (AI-CAIQ) on the same registry. | [link to the STAR Registry entry] |

-->

| Design fact | Detail |
| --- | --- |
| Customer loan and receivable rows stored by the hosted tool | Not confirmed here. The local engine keeps rows in your sandbox. |
| Passwords held | None. Sign-in is Microsoft single sign-on. |
| Write access to your systems | None |
| Local engine | The script is returned in full for review before it runs. It needs `numpy`. |

**Independent audits.** Saifs AI does not yet hold an independently audited certificate such as SOC 2 or ISO 27001. If your review needs a completed security questionnaire, request it through the [contact page](https://saifs.ai/contact-us).

## Responsible use

This connector is an expected credit loss calculation for a portfolio, and a provision matrix for trade receivables. It supports a human decision. It does not make one. Management, the auditor, and the board approve the provision.

Say "calculated under these assumptions." Do not say the provision was approved, posted, or accepted by a regulator. A result is not an accounting opinion, a regulatory sign-off, or financial or legal advice.

It does not decide an individual loan application, a limit, or a restructuring. Do not use it that way. It does not compute interest or an effective interest rate.

Do not describe the result as approved by a supervisor. Floors and staging rules are yours. They are dated when you send `source` and `valid_from`. The tool does not embed the RBI ECL framework or any other direction. A team preparing for that framework passes its own dated floor table.

Send pseudonymous loan and borrower references. The calculation does not need a name, PAN, Aadhaar, or an account number. Do not send protected personal attributes. Days past due, balances, and ratings are enough.

There is no promise that the provision will be accepted. A clear pack means the ECL calculation can be rerun on these inputs. It does not mean the assumptions were the right ones.

## Token usage and cost

With the hosted tool, the rows are part of the tool call, so token use grows with the file. With the local engine, the agent receives a script and the rows stay in the sandbox, so token use does not grow with the file.

<!-- TODO Saifs AI: measure tokens before publishing figures. Use one named tokenizer on three sizes: a small file, the hosted limit (20,001 exposures or 100,001 days-past-due history rows), and a larger file. Count both directions. Include the script, the write into the sandbox, and the printed decision cards. Record input and output tokens, a dated reference price per million, the first-run and later-run multipliers, and a monthly example. State where the hosted tool uses fewer tokens. Do not copy the Inventory Intelligence token table. -->

Until those figures are measured, this page does not state a token count or a dollar cost.

**Which route to choose, before the token table exists**

- **Inside the hosted limits, and a single question:** the hosted tool. It is one call.
- **Over 20,000 exposures or 100,000 days-past-due history rows:** the local engine. It is the only route.
- **The rows must stay in your environment:** the local engine. Turn the hosted tool off.
- **The same large file, many times in one session:** the local engine, if the sandbox keeps the script.

**Hosts that bill per action.** Some hosts charge a fixed amount per tool call and do not bill by token. On those hosts, a smaller token count may not reduce the bill. The local engine may also use one extra action to run the script. Choose it there for file size and for where the data stays, not for a lower token bill.

## Prerequisites

- A Saifs AI account, created by signing in with Microsoft single sign-on
- A plan from the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Access to Copilot Studio, Microsoft 365 Copilot, Azure AI Foundry, Power Automate, Power Apps, or Azure Logic Apps
- A loan export or a trade-receivable ageing. History, scenarios, floors, and last quarter's snapshot make the ECL calculation more complete.
- For the local engine: code execution enabled on the agent host, and `numpy` available there

## How to get credentials

No keys or secrets are needed. When you create a connection, you are redirected to sign in with your Microsoft account and approve access. If the connection later expires, sign in again from the connection page.

## Get started with your connector

1. In Copilot Studio, open your agent and go to **Tools**, then **Add a tool**.
2. Search for **ECL Calculator: IFRS 9/Ind AS** and select it.
3. Create a connection and sign in.
4. Add this to the agent's instructions: "For any question about expected credit loss, IFRS 9 impairment, Ind AS 109 staging, a provision matrix, or why the provision changed, call ECL Calculator: IFRS 9/Ind AS. Do not calculate the provision yourself. Do not post a journal or decide an individual loan."
5. Provide the rows by pasting them, uploading them, or pointing the agent at a file. Send pseudonymous references. Include the previous snapshot when you want the bridge.
6. Ask in plain language. Two examples are "Calculate our ECL for this quarter" and "Why did our ECL increase?"

Treat the result as a calculation under the assumptions you sent. A person approves the provision before it is booked.

## For administrators

Enabling this connector gives the organisation one approved way to decide what the expected credit loss is this quarter, and why it changed. It does that inside the agents your makers already build.

**Why this is an administrator decision.** Without a calculation tool, makers ask an agent to compute loan loss provisioning from a spreadsheet. A language model can give a different figure on the next run, and the result cannot be compared with last quarter. A governed connector uses one ECL calculation for every agent and team.

- **You choose who can connect it.** Tenant and environment permissions apply. Only users who can create a connection can call the tools.
- **You keep your system of record.** The connector calculates and explains. It does not post a provision or write back to any system.
- **You can check the number.** The same file and settings can be run again. The tool is marked idempotent. A missing discount rate is not invented. The result is labelled undiscounted.
- **You control where data goes.** Turn off the hosted tool per agent to keep loan and receivable rows in your environment.
- **You set data policy as usual.** Place the connector in the Business, Non-business, or Blocked group with Power Platform data loss prevention policies.
- **There are no secrets to rotate.** Expired connections are renewed by signing in again.

**Review checklist**

- Decide whether the hosted tool is allowed, or local engine only
- Assign the connector to the correct data policy group
- Confirm that makers will send pseudonymous references, and will not send names, PAN, Aadhaar, or account numbers
- Confirm that the caller, not the server, stores `ecl_snapshot.json` between quarters
- Review the privacy policy and terms
- Read the self-assessments listed under Security and compliance, once they are published
- Review the local engine script if your policy requires it
- Confirm the hosted-tool retention note in Data handling and security before a wide rollout

## For developers

Nothing on the schema is marked required. Send the arrays the job needs. The response reports decision cards first. This page does not name the other JSON keys until a signed-in call is pasted below.

<!-- TODO Saifs AI: confirm the minimum arrays each job actually requires, from a signed-in call. The schema marks no field as required. The job names and input descriptions are what this table uses. -->

### Exposure fields

One object per facility, in `exposures`. Up to 20,001 items.

| Field | Meaning | Needed for |
| --- | --- | --- |
| Loan or account reference, borrower reference | Identifiers. Send pseudonymous values. | The stage list, and `explain_exposure` |
| Segment or product | The pool the facility sits in | PD, LGD, floors, and segment totals |
| Drawn, undrawn or sanctioned limit | Balances | Exposure at default and the provision |
| Days past due, SMA, NPA or asset classification | Delinquency and status | Stage 3 and stage 2 |
| Restructured, watch list | Flags you already hold | Stage, when your rules say so |
| Rating at origination, rating now | The migration you measured | A downgrade rule, if you set `rating_downgrade_notches` |
| Remaining term or maturity | Life left, at `as_of` | Lifetime expected credit loss |
| Previous stage, months since cure | Where it was, and how long it has cured | Cure and probation, when you set `cure_months_stage2` and `cure_months_stage3` |

### History, recoveries, and caller parameters

| Field | Meaning | Needed for |
| --- | --- | --- |
| `roll_rate_history` | `{segment, month, from_state, to_state, count}`. Up to 50,001 rows. | `fit_pd_curves` |
| `dpd_history` | Account-month snapshots. Transitions are counted from consecutive months. Up to 100,001 rows. | PD when you do not send a matrix |
| `pd_curves` | Caller PD, as a curve or a 12-month PD by segment | Skips the fitted curve for that segment |
| `recoveries` | `{default_ref, segment, ead_at_default, cashflows, complete?}`. Up to 10,001 rows. | `fit_lgd` |
| `lgd` | A number, a map by segment, or a list. `0.45` or `45` are both accepted. | When you are not fitting LGD |
| `ccf` | A number, a map, or a list. Applied to undrawn limits. | Exposure at default |
| `ccf_history` | `{segment, limit, drawn_before, drawn_at_default}`. Up to 10,001 rows. | A CCF from your own defaults |
| `scenarios` | `{name, weight, pd_multiplier or matrix_shift}`. Up to 20. | Probability-weighted totals |
| `floors` | `{stage, segment, min_pct, source, valid_from}`. Segment `*` means all. Up to 1,000. | A minimum provision. `min_pct` is a percent of the floor base. |
| `discount_factors` | One factor per year, or `{month or year, factor}` | Present value. Omit them and the result is undiscounted and labelled. |
| `previous_run` | The snapshot, or the full result, from last time | `ecl_movement` |
| `write_offs` | `{exposure_ref, amount}`, or references, since the previous run | The bridge |
| `exposure_ref` | One facility to explain | `explain_exposure` |

### Trade receivables

| Field | Meaning | Needed for |
| --- | --- | --- |
| `receivables_ageing` | `{bucket, balance}`, or invoices with customer, due date or days overdue, and balance. Up to 20,001 rows. | `provision_matrix` |
| `loss_history` | `{period, bucket, balance, written_off}`. Up to 5,001 rows. | Historical loss rates |
| `loss_rates` | Caller rate per bucket, such as `"1.6%"`. Overrides history. | When you are not fitting rates |
| `forward_looking` | A multiplier on the historical rates, or one per bucket | The overlay you choose |

### Options and defaults

| Option | Default | Purpose |
| --- | --- | --- |
| `job` | Omit, and the tool runs what the inputs allow | One of the eight jobs |
| `as_of` | None stated | Reporting date for floors in force, and for remaining term |
| `regulated` | false | A regulated run flags missing floors at high priority |
| `period_months` | 1 (1 to 12) | On the schema. What it changes is not stated. See the note above. |
| `min_history_months` | 24 (1 to 600) | History length the PD step can test |
| `min_defaults` | 30 (1 to 100,000) | Default count the LGD step can test |
| `default_life_months` | 12 (1 to 600) | On the schema. What it changes is not stated. |
| `ead_profile` | `constant` | Or `amortising`, or balance fractions by month |
| `floor_base` | `ead` | Or `drawn` |
| `dpd_buckets` | 30, 60, 90 | Bucket edges for days past due |
| `credibility_k` | 50 | Pooling weight for small segments. Minimum 0. |
| `pd_draws` | 300 (50 to 5,000) | Draws used for the PD range |
| `seed` | 7 | Fixes those draws so the same call repeats |
| `date_order` | None stated | `DMY` or `MDY` |
| `mapping` | None | Header to field overrides |
| `currency_symbol` | Empty | Display label only. `Rs` or `$`. No conversion. |
| `max_listed` | 200 (1 to 100,000) | How many rows the hosted result lists |

`staging_rules` is an object. The tool description names these starting values: `stage3_dpd` 90, `stage2_dpd` 30, `sma_stage2_from` 1, `restructured_stage` 2, `watchlist_stage` 2, and `borrower_contagion` true. Also named, with no number in the schema: `npa_stage3`, `rating_downgrade_notches`, `rating_scale`, `cure_months_stage2`, and `cure_months_stage3`.

<!-- TODO Saifs AI: confirm from a live call whether mixed currencies in one file are kept separate, converted, or refused. The schema describes a display symbol only. It does not describe a conversion. -->

### Sample request

```json
{
  "job": "compute_ecl",
  "as_of": "2026-09-30",
  "currency_symbol": "Rs",
  "exposures": [
    {
      "exposure_ref": "L-1001",
      "borrower_ref": "B-22",
      "segment": "two-wheeler",
      "drawn": 180000,
      "undrawn": 20000,
      "dpd": 12,
      "remaining_term_months": 18
    }
  ],
  "lgd": { "two-wheeler": 0.45 },
  "ccf": 0.5,
  "pd_curves": [
    { "segment": "two-wheeler", "pd_12m": 0.03 }
  ],
  "scenarios": [
    { "name": "base", "weight": 0.5, "pd_multiplier": 1 },
    { "name": "downside", "weight": 0.3, "pd_multiplier": 1.3 },
    { "name": "upside", "weight": 0.2, "pd_multiplier": 0.8 }
  ]
}
```

The rows above are made up. They show the shape of a small call. They are not a customer book.

### Sample response (shortened)

A signed-in call was not available. On 24 November 2026 the endpoint `https://ecl-mcp-microsoft.saifs.ai/mcp` returned HTTP 401 without a token. This page does not invent a response or its field names.

<!-- TODO Saifs AI: replace this note with a shortened real response from ecl_calculator. Include one exposure's stage and trigger, the 12-month and lifetime figures, one floor line, and the field that explains the number. Do not invent field names. Do not paste a response from another product. -->

### Response sections

The tool states what comes back. The names in the first column are the words on the tool, not confirmed JSON keys.

| What comes back | Use it for |
| --- | --- |
| Decision cards | The plain-language action, first |
| Stage and trigger per exposure | Why the facility is in stage 1, stage 2, or stage 3 |
| 12-month and lifetime ECL | The provision by exposure, segment, and stage |
| Allowance and provision | Drawn allowance and undrawn provision, as the tool separates them |
| Scenario table | The weighted cases you sent |
| Floors applied | Where the dated floor table changed the model figure |
| Quarter-on-quarter bridge | Why it changed, when you sent `previous_run` |
| Explanations | The working for a facility, from `explain_exposure` |
| Audit pack with hashes, and a snapshot | The file for reviewers. Keep the snapshot for the next quarter. |

<!-- TODO Saifs AI: replace the first column with the JSON keys from a live response. -->

## Known issues and limitations

- **A calculation aid only.** It does not post a provision, approve a number, or update any system.
- **Not an opinion.** It is not an accounting opinion, a regulatory sign-off, or financial or legal advice. It does not decide an individual loan.
- **No interest.** It does not compute interest or an effective interest rate.
- **Your rules, not a buried regulation.** Staging rules and floors are whatever you send. The tool does not embed the RBI ECL framework. A CECL-style lifetime loss figure is not a CECL regulatory filing.
- **Hosted tool size.** The caps are in [At a glance](#at-a-glance). Above 20,000 exposures or 100,000 days-past-due history rows, use the local engine.
- **Thin history.** `min_history_months` defaults to 24 and `min_defaults` defaults to 30. The refusal wording is not confirmed here.
- **No discount factors.** The result is undiscounted and labelled. No rate is derived.
- **No previous snapshot.** Without `previous_run`, this page does not claim a bridge.
- **Currency.** `currency_symbol` is a display label. There is no conversion between currencies.
- **References.** Send pseudonymous loan and borrower references. Do not send names, PAN, Aadhaar, or account numbers.
- **Regions.** Not available in US Government (GCC, GCC High, DoD) or China (21Vianet) clouds.

<!-- TODO Saifs AI: rate limits, timeouts, and the service region are not in the tool schema. Add them here when confirmed. Also add the refusal text, the stage horizon, and the floor comparison once the engine script confirms them. -->

## Common errors and remedies

| What you see | Cause | Remedy |
| --- | --- | --- |
| HTTP 401, or the tool asks you to sign in again | The call had no token, or the connection has expired. A call with no token returned 401 on 24 November 2026. | Open the connection and sign in again |
| A stage list with no quarter bridge | `previous_run` was not sent | Pass the snapshot you kept from the last ECL calculation |
| The result says floors were not applied, on a regulated run | `regulated` is true and `floors` was empty. The tool flags that at high priority. | Send the dated floor table, or set `regulated` to false if this run is model-only |
| An undiscounted label | `discount_factors` was omitted | Send factors if you want present value. The tool will not derive a rate. |
| The agent gives a provision without calling the tool | The instructions do not require the tool | Add the instruction in [Get started with your connector](#get-started-with-your-connector) |
| The local engine does not run | Code execution is off, or `numpy` is missing | Enable code execution, or use the hosted tool inside the caps |
| A header landed in the wrong field | The export name was not the one the tool expected | Send `mapping` for that header |

## FAQ

**Do I need a credit-risk modeller to use this?**
No. A user asks in plain language and the agent calls the tool. The ECL calculation runs inside the tool. Someone who owns the assumptions should still review them.

**What is the least data I need?**
For loans, an exposure list. For trade receivables, an ageing. PD, LGD, a credit conversion factor, scenarios, floors, and last quarter's snapshot make the answer more complete.

**Does my data leave my environment?**
Not with the local engine. The tool states that no data reaches the server on that path. With the hosted tool, the rows in the call are sent to Saifs AI for the calculation. You can turn the hosted tool off.

**Will it change anything in my systems?**
No. It does not post a provision, and it does not write to a core-banking, loan, or accounting system. You keep the snapshot yourself.

**Our core-banking system already has an MCP server. Why add this?**
That server gives the agent the system's records and functions. This connector gives the agent the ECL calculation, and it works on rows from any export.

**Why do I get the same answer every time?**
The tool is marked idempotent, and the method is fixed. The same inputs return the same numbers. `seed` defaults to 7. The agent's sentences around the numbers may change.

**What happens if my data is incomplete?**
The tool still runs on what you sent. Missing discount factors stay undiscounted and labelled. A regulated run with no floors is flagged. A bridge needs the previous snapshot.

**Can I check how a result was produced?**
Yes. Use `explain_exposure` with the `exposure_ref`. The audit pack includes hashes. The pack shows the calculation under your assumptions. It is not an opinion on those assumptions.

**How large a file can it handle?**
The hosted tool stops at the limits in [At a glance](#at-a-glance). Larger files use the local engine. `max_listed` (default 200) caps how many rows the hosted result lists.

**Can it run on a schedule?**
Yes. The tools run on request. Call the connector from a Power Automate flow with a recurrence trigger, and store the snapshot between runs. The connector does not book the entry itself.

**What does it cost?**
Plans and prices are on the [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment). Microsoft Premium connector licensing applies separately.

**Is the result an accounting opinion?**
No. It is an IFRS 9 impairment calculation under the assumptions you sent. The loan loss provisioning total is for a person to approve. It is not an accounting opinion, a regulatory sign-off, or a decision on an individual loan.

## Support

**ECL Calculator: IFRS 9/Ind AS - Saifs AI** is published by Saifs AI.

- Contact and help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Pricing: [Microsoft Marketplace listing](https://marketplace.microsoft.com/en-us/product/saifeesoftwaresolutions1773311173044.saifsai-remote-mcps-payment)
- Privacy: [https://saifs.ai/privacy_policy/ecl-calculator-ifrs-9-ind-as](https://saifs.ai/privacy_policy/ecl-calculator-ifrs-9-ind-as)
- Terms: [https://saifs.ai/eula](https://saifs.ai/eula)
- Sign-in: Microsoft single sign-on (OAuth 2.0). No API keys.
- MCP endpoint: `https://ecl-mcp-microsoft.saifs.ai/mcp`

<!-- TODO Saifs AI: add the Learn more link here after the product page is published. See the note at the top of this file. -->

<!-- OPEN QUESTIONS
1. Hosted-tool retention, any audit or billing hash, hosting region, rate limits, and timeout. Not stated on the live tool schema. The privacy page returned HTTP 200 on 24 November 2026 and does not state portfolio-row retention. That page is a general template: it mentions Facebook and Google sign-in, Stripe, and support@multiplewords.in. The package uses Microsoft single sign-on and taher@saifytech.com. Reconcile them.
2. Whether the local engine script makes network calls. The tool states that no data reaches the server on that path.
3. A shortened real response, including JSON field names, from a signed-in call. The endpoint returned HTTP 401 without a token on 24 November 2026.
4. Engine version field name, if one exists. Do not copy metadata.model_version from Inventory Intelligence.
5. The stage horizon split, the floor comparison, the bridge line names, and the thin-history refusal text. Options exist (min_history_months 24, min_defaults 30). The exact behaviour is not on the schema.
6. What default_life_months (default 12) and period_months (default 1) change.
7. The minimum arrays each job requires. The schema marks nothing as required.
8. Named header aliases, and the response field that reports a mapping. Not listed in the schema.
9. Whether mixed currencies in one file are kept separate, converted, or refused. No conversion is claimed.
10. Token counts. Not measured. Do not copy another product's table.
11. Product page URL. The three candidate paths returned HTTP 404 on 24 November 2026.
12. Whether the staging-rule numbers in the tool description (90, 30, 1, 2, 2, true) are applied when staging_rules is omitted, or only documented examples. The schema does not set them as JSON defaults.
-->
