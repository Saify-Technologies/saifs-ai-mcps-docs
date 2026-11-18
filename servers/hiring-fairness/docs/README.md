# Hiring Fairness & Selection ROI

[SAIFS AI](https://saifs.ai) (`saifs.ai`) publishes this as a remote MCP server under the registry name `ai.saifs/hiring-fairness`. It measures two things about a hiring process, at group level: whether a stage selects one group at a lower rate than another, and whether a test or interview is worth what it costs.

It does not score, rank, or select a person. It does not rank people for termination, and it does not monitor productivity. That boundary is the product. The output is analysis for a talent, compliance, or DEI review. It is not legal advice. Employment law is not the same everywhere.

People search for this as adverse-impact analysis, the four-fifths rule, the 80% rule, hiring bias, selection rate, assessment validity, or DEI analytics. The questions are “is our hiring biased” and “is this test worth it.” The short install page is the [server README](../README.md). This page is what a ratio means, why a funnel needs a like-for-like check, and how to send counts.

| | |
|---|---|
| **Registry name** | `ai.saifs/hiring-fairness` |
| **Endpoint** | `https://hiring-fairness-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## What it is

You send counts by stage and group, or anonymous score and outcome pairs with no names. One call to `hiring_fairness` covers up to 5,000 rows. A larger ATS export uses `hiring_fairness_get_engine`: a Python script that turns candidate rows into group counts on your machine, then runs the same analysis. Candidate-level rows do not need to reach the server.

The reply is a set of decision cards.

| Card | What it is saying |
|---|---|
| **REVIEW THIS STAGE** | A stage’s impact ratio is under 0.80 and the gap is larger than chance. Open that stage. |
| **COMPARE LIKE WITH LIKE** | The company-wide rate and the within-job rate disagree. Simpson’s paradox. Read the job-level rates. |
| **TEST PREDICTS** | Anonymous scores track outcomes, or they do not. Includes whether the test underrates a group. |
| **SET CUTOFF** | A pass mark implied by validity and the selection ratio. A policy number, not a list of people. |
| **WORTH THE COST** | The Brogden-Cronbach-Gleser utility of the method, net of its cost. |

The four-fifths arithmetic, the significance test, the funnel, the like-for-like check, Taylor-Russell, and the utility figure all live inside `hiring_fairness`. They are not separate tools.

The call is stateless. Counts are processed and discarded. A quarterly review is you sending the next cycle’s counts. The server does not keep the funnel.

Sending group counts, and keeping names off the request, is the data practice this server is built for. Fairness statistics are about groups. An individual’s row is not a result.

## Why the funnel is measured in rates

Companies hire without measuring two facts. One is whether a stage selects protected or monitored groups at different rates. The other is whether the assessment or the interview actually improves who gets hired, enough to justify its cost. Adverse-impact analysis has been part of US federal guidance since the EEOC’s 1978 Uniform Guidelines. Utility analysis is the industrial-psychology account of what a valid method is worth. Both are usually a consulting engagement. A language model can divide two rates once. It will not pair the four-fifths rule with a significance test, refuse a ratio built on a handful of people, check the same rates inside each job, and keep the result as analysis rather than a ranking of candidates.

## How to use it

Connect the endpoint in the [install README](../README.md) and sign in when the client asks. Export stage counts from Greenhouse, Lever, Workday, or any ATS. Prefer a table of counts. If the file is one row per candidate and it is large, run the local script so only the counts are analysed.

```json
{
  "stages": [
    {"stage": "technical_screen", "job": "backend", "group": "A", "applicants": 200, "selected": 80},
    {"stage": "technical_screen", "job": "backend", "group": "B", "applicants": 150, "selected": 41}
  ]
}
```

Group labels are whatever categories your review is allowed to use. Do not send names, email addresses, or a national identifier. For a validity study, send anonymous pairs: a predictor score and an outcome, plus the group label if the question is whether the test underrates a group. No candidate id.

### The four-fifths arithmetic

At each stage, the selection rate is the number selected divided by the number who reached that stage. The impact ratio is that rate divided by the rate of the group with the highest rate.

On the counts above, group A’s screen rate is 80/200 = 0.40. Group B’s is 41/150 ≈ 0.27. The impact ratio is about 0.68, which is under 0.80. The card then asks whether that gap is larger than chance: a two-proportion z-test, or Fisher’s exact test when the counts are small. A ratio under 0.80 on a tiny cell is not reported as if it were precise. A minimum-n gate holds the card back and says the cell is too small.

A ratio under 0.80 is a reason to review the stage. It is not a finding that the stage is unlawful, and it is not a finding about any person who passed or failed it.

### Like for like

Pooled across every job, a company can look inside the four-fifths line while one job is far outside it, because the mix of jobs differs by group. The reverse happens too: a pooled gap that disappears once each job is compared with itself. **COMPARE LIKE WITH LIKE** is that check. Read the within-job ratios before you brief anyone on the company-wide number.

### Whether the test earns its cost

`TEST PREDICTS` reports the correlation between the anonymous predictor and the outcome, then the Taylor-Russell (1939) share of hires expected to succeed, given that validity, the selection ratio, and the base rate of success in the pool. If the same scores track outcomes differently by group, the card says the test underrates that group. That is a property of the test, not a score for a candidate.

`SET CUTOFF` turns validity and the selection ratio into a pass mark to consider. It does not return the people who would fall under it.

`WORTH THE COST` is the Brogden-Cronbach-Gleser utility:

```text
ΔU = N · T · r_xy · SD_y · Z̄_x − cost
```

N is the number of hires, T is how long they stay in the role, r_xy is the validity, SD_y is the money value of a standard deviation of performance, Z̄_x is the average standardised score of the people hired, and cost is what the method costs. The figure is only as solid as those inputs. It is an estimate of selection value. It is not a guaranteed saving, and it is not a reason to drop a stage that the fairness cards have flagged.

### Prompts

- “Is our interview stage fair to women?”
- “Run the 4/5ths rule on our funnel.”
- “Is this coding test worth it?”
- “Here are our applicant and hire counts by stage and gender. Is any stage showing adverse impact?”
- “Is our coding assessment worth what it costs, and what pass mark is fair?”
- “Compare selection rates across departments like for like.”

## Tools

| Tool | What you send | What comes back | Do not use it when |
|---|---|---|---|
| `hiring_fairness` | Stage counts by group, or up to 5,000 stage/group rows, or anonymous score/outcome pairs | Decision cards: review a stage, compare like with like, whether a test predicts, a cutoff to consider, and utility net of cost | You want a person scored, ranked, hired, or fired. The tool will not do that. A handful of people in a cell is reported as too small, not as a precise ratio |
| `hiring_fairness_get_engine` | A request for an ATS export larger than 5,000 rows | A numpy-only Python script that aggregates candidate rows to group counts on your machine and runs the same analysis | The file is already group counts under 5,000 rows. Use `hiring_fairness` |

## How to read a result

- **REVIEW THIS STAGE** means the impact ratio is under four-fifths and the significance test agrees the gap is large relative to the counts. Take the stage apart: the exercise, the rubric, the panel. Do not treat the card as a determination that anyone discriminated.
- **A ratio of 0.80 or above** does not certify that the process is fair or lawful. It means this particular screen did not light up.
- **COMPARE LIKE WITH LIKE** outranks the pooled slide. Brief the within-job rates.
- **Small cells** stay unpublished as a hard ratio. Add a later cycle’s counts, or pool only where the like-for-like check says the jobs really are the same decision.
- **TEST PREDICTS** is about the method. A weak correlation means the test is not carrying the decision. A group gap in prediction means the test is a poor yardstick for that group.
- **SET CUTOFF** is a number to discuss. It is not a reject list.
- **WORTH THE COST** moves when you change SD_y or the validity. Quote the inputs with the rupee or dollar figure.
- **The cards are analysis.** Counsel, in the jurisdiction where you hire, decides what the law requires. The EEOC four-fifths rule is the reference frame these ratios use. It is not a verdict.

## Data and what this will not do

Hiring data is sensitive personal data. Send group counts, or anonymous score and outcome pairs. The local script exists so a large candidate file can be reduced to counts before any of it is analysed as a rate. The server processes the request and does not store it. Each user signs in with OAuth. Your own duties under India’s Digital Personal Data Protection Act, the GDPR, or any other law that applies to the export, including consent, stay with you.

This server will not score an employee, rank a person, recommend who to hire or fire, watch how someone works, or turn a group ratio into a label on an individual. It will not present itself as legal advice.

Privacy policy: https://saifs.ai/privacy_policy

## Plans

| Tier | What it covers |
|---|---|
| **Free** | A single adverse-impact check |
| **Pro** | ₹4,100 / $49 per month — the full funnel, significance tests, and utility analysis |
| **Business** | ₹16,500 / $199 per month — fairness across cycles you send, validity studies, ATS export pulls, and an audit export |

The free tier is enough to run one stage’s four-fifths check with no sales call. Current terms are on [saifs.ai](https://saifs.ai).

## Support

- Install and client setup: [server README](../README.md)
- Website: https://saifs.ai
- Registry: https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
