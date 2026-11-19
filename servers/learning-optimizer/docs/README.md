# Learning Optimizer

[SAIFS AI](https://saifs.ai) (`saifs.ai`) publishes this as a remote MCP server under the registry name `ai.saifs/learning-optimizer`. It schedules when a learner should review each item so the review happens before the memory is gone, and so an exam-day plan peaks on the exam rather than the night before.

People search for this as spaced repetition, SM-2, FSRS, flashcard scheduling, learning retention, or the forgetting curve. The questions are “when to revise” and “the daily revision list.” The short install page is the [server README](../README.md). This page is what a next-review date means, why a fixed interval wastes the session, and how to send a review.

| | |
|---|---|
| **Registry name** | `ai.saifs/learning-optimizer` |
| **Endpoint** | `https://learning-optimizer-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## What it is

An ed-tech app, a test-prep product, a language app, or a corporate learning system calls this server once per learner and item. You send a pseudonymous learner id, the item, the last result, and the memory state you stored last time. You get back the next review time, the predicted probability of recall, and the updated state to store.

Six tools cover that loop. The same inputs and the same state produce the same schedule.

| Tool | What you ask |
|---|---|
| `schedule_review` | When is the next review of this item, and what is P(recall) then? |
| `due_reviews` | What should this learner review today, in order? |
| `predict_retention` | What is P(recall) on a date you name? |
| `optimize_study_plan` | How should this item bank be spaced so retention peaks on exam day? |
| `forgetting_curve` | What decay was fitted, and what are its parameters? |
| `explain_schedule` | Why this interval, which model, and how sure is it? |

The call is stateless. Review history and the per-item state (ease, interval, stability) stay in your app. The server fits the schedule from what you send and discards it. A learner id should not be a name.

For the same study time, spaced reviews of this kind retain on the order of 20–40% more than a single cram or a fixed interval. That range is the research record these models come from. It is not a guarantee for one learner. A missed item schedules an earlier review. It is not a rank, and it is not a reason to shame anyone. Whether the question itself is a sound item is a different server: [Assessment Intelligence](../../assessment-intelligence/README.md). Which topic the learner is ready for next is [Mastery Pathing](../../mastery-pathing/README.md).

## Why a fixed interval forgets

Ebbinghaus measured the forgetting curve in 1885. Retention falls quickly, then more slowly, and a review at the right moment rebuilds it with less work than restudying everything. Most products still wait a fixed number of days, or they wait until the learner opens the app and crams. SM-2, the scheduler Anki made widely used, was published by Woźniak in 1990. Half-life regression, the model Settles and Meeder published from Duolingo review data in 2016, and FSRS, the three-component model from 2022, fit the curve to the learner’s own reviews.

Copying the SM-2 update is the part teams usually stop at. Holding a state per learner per item, fitting a curve once there is history, and explaining the interval is the rest. A language model cannot keep that state between sessions. Your app keeps it. This server is the fit.

On the first review there is nothing to fit. The schedule is SM-2, and `explain_schedule` says so. Half-life regression and FSRS take over as the review history you send grows. The confidence is wider at the start. That ramp is reported, not hidden.

## How to use it

Connect the endpoint in the [install README](../README.md) and sign in when the client asks. Send ids you can join back to the learner on your side.

```json
{
  "learner_id": "L-2041",
  "item_id": "142",
  "last_result": { "quality": 2, "reviewed_at": "2026-11-18" },
  "state": { "ease_factor": 2.5, "interval_days": 6, "repetitions": 2 }
}
```

`quality` is the SM-2 grade from 0 to 5. A 3 or above is a successful recall. Below 3, the interval resets and the item comes back soon. Store the `state` in the response and send it on the next call. If you omit it, the item is treated as new.

`due_reviews` takes the learner, the date, and the states of the items you hold. The queue is ordered by how close recall is to falling through the target, not by which item was added first.

`optimize_study_plan` takes the learner, the exam date, and the item bank, and lays the reviews so predicted retention is highest on that day rather than a week early or a week late. Twenty-eight days out is a normal test-prep call.

`predict_retention` answers “if we do not review until this date, what is P(recall)?” `forgetting_curve` returns the fitted decay and its parameters. `explain_schedule` names the model, the evidence, and the confidence. A cold item is labelled as the SM-2 baseline.

### Prompts

- “What should this student revise today?”
- “Schedule revisions before the exam.”
- “Predict recall for this topic.”
- “This student just missed item 142. When should they review it next, and what’s P(recall) then?”
- “What should this learner review today, in priority order?”
- “Exam is 28 days out. Build a study plan over this item bank so they peak on the day.”

## Tools

| Tool | What you send | What comes back | Do not use it when |
|---|---|---|---|
| `due_reviews` | A pseudonymous learner id, a date, and the item states you have stored | Today’s queue, highest priority first | You only have one item. That is `schedule_review` |
| `explain_schedule` | An item and the schedule just computed | The model, the evidence, and the confidence. A new item is named as the SM-2 baseline | You have not scheduled it yet |
| `forgetting_curve` | Learner id, item id, and the review history or state | The fitted decay and its parameters | You want the calendar. The curve is the reason, not the queue |
| `optimize_study_plan` | Learner, exam date, and an item bank | A schedule aimed at peak retention on that date | There is no exam date, or you only need today’s list |
| `predict_retention` | Learner id, item id, a future date, and the state | P(recall) on that date | You want a class ranking or a grade. This is one learner and one item |
| `schedule_review` | Learner id, item id, the last result, and the stored state | The next review time, P(recall) then, and the state to store | You want the whole day. That is `due_reviews` |

## The methods

### Forgetting curve

Ebbinghaus (1885). With stability S, retention after t days is

```text
R = e^(−t / S)
```

A review that succeeds increases S. A review that fails shortens the next gap. The target is the retention you want, often around the time the curve would otherwise fall through it. The scheduler places the review there, which is fewer sessions than reviewing everything every day.

### SM-2

Woźniak (1990). This is the day-one engine and the Free tier. Each item has an ease factor EF, starting at 2.5, and an interval in days. After a review graded q from 0 to 5:

- If q is 3 or above, the intervals run 1 day, then 6 days, then each further interval is the previous interval times EF.
- EF moves by `0.1 − (5 − q) · (0.08 + (5 − q) · 0.02)`, and it does not fall below 1.3.
- If q is below 3, the interval resets and the item is due again soon.

Leitner’s box system (1972) is the same idea in coarser steps: a success promotes the item to a longer box, a failure sends it back. SM-2 is the continuous form of that.

### Half-life regression

Settles and Meeder (2016). Memory half-life is a function of features of the history you send: past correct and incorrect reviews, and the lag since the last one.

```text
h = 2^(θ · x)
p = 2^(−Δ / h)
```

Δ is the time since the last review. θ is fit from those reviews. With too few reviews the fit is not claimed. The schedule stays on SM-2 and the explanation says the half-life is not identified yet. This model is the Pro tier, along with the exam-day plan and retention at a named date.

### FSRS

The 2022 model used on the Business tier. It keeps three numbers instead of a single ease factor. Difficulty is how hard this item is for this learner. Stability is how slowly retrievability decays. Retrievability is the probability of recall right now. The next interval is the gap that lands retrievability on the target. It needs the review history SM-2 does not. Until that history exists, the reply is the SM-2 schedule, labelled as such.

## How to read a result

- **The date and P(recall) travel together.** A next review without the probability is not the result. The probability is a forecast of memory, not a mark.
- **A miss pulls the item forward.** That is the schedule working. It is not a leaderboard of who forgot.
- **Cold start is labelled.** Ease 2.5 and a first interval of 1 day means there is no personal curve yet. Do not describe that interval as a fitted half-life.
- **Confidence widens on a short history** and narrows as you send more reviews of that item. Quote the confidence with the date.
- **The exam plan is a peak, not a cram.** Reviews are spread so retention is highest on the exam date. Items already stable take fewer slots. Items that failed recently take earlier ones.
- **Store the returned state.** The next call without it starts the item over.

## Data and what this will not do

Review records are personal data, and in test prep they are often data of minors. Send pseudonymous ids. The server processes the request and does not store the learner or the curve. Each user signs in with OAuth. Consent, retention, and deletion under India’s Digital Personal Data Protection Act, the GDPR, or any other law that applies stay with the app that holds the history.

The server will not name a learner, rank a class by who forgets, or turn a low P(recall) into a judgment of ability. Ability and item quality are [Assessment Intelligence](../../assessment-intelligence/README.md). This server will not keep the review corpus for you, and it will not pretend a brand-new item has a personalised FSRS curve.

Privacy policy: https://saifs.ai/privacy_policy

## Plans

| Tier | What it covers |
|---|---|
| **Free** | SM-2 scheduling, up to 1,000 items across the learner base |
| **Pro** | ₹4,100 / $49 per month — half-life regression, the exam-day study plan, and retention on a named date |
| **Business** | ₹16,500 / $199 per month — FSRS, a large learner base, accuracy of the predicted recall against later reviews, and API volume |
| **Platform** | Ed-tech products that embed the scheduler, priced per monthly active user, with a white-label option |

The free tier is enough to schedule SM-2 for a small item bank with no sales call. A new learner is on SM-2 on every tier until the history you send can support a fitted curve. Current terms are on [saifs.ai](https://saifs.ai).

## Support

- Install and client setup: [server README](../README.md)
- Whether the item measures the right thing: [Assessment Intelligence](../../assessment-intelligence/README.md)
- Which topic comes next: [Mastery Pathing](../../mastery-pathing/README.md)
- Website: https://saifs.ai
- Registry: https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
