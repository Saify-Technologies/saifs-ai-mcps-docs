# Assessment Intelligence

[SAIFS AI](https://saifs.ai) (`saifs.ai`) publishes this as a remote MCP server under the registry name `ai.saifs/assessment-intelligence`. It runs psychometric analysis on assessment data you send in the request: item quality, true ability, test reliability, the next adaptive question, and skill mastery.

People search for this work as item response theory (IRT), test analytics, psychometrics, skill diagnostics, adaptive testing, Bayesian knowledge tracing (BKT), and computerized adaptive testing (CAT). Those are the methods below. The short install page is the [server README](../README.md). This page is what the server is, why a raw percentage is not enough, and how to use each tool.

| | |
|---|---|
| **Registry name** | `ai.saifs/assessment-intelligence` |
| **Endpoint** | `https://assessment-intelligence-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## What it is

A language model can talk about a test. It cannot reliably fit an IRT model or carry mastery across a term. Those fits are numerical: they have to converge, stay identified, and cope with sparse responses. Assessment Intelligence does that fit on the server and returns the numbers, plus a plain explanation an educator can act on.

You send responses (a pseudonymous learner, an item, and whether the answer was correct). You get back abilities, item statistics, reliability, mastery, or the next item. Nothing in the call is written to a learner record on the server. Each request is processed and dropped. Sign-in is per user, through OAuth.

The same math sits behind large-scale exams such as the SAT, GRE, and GMAT, and behind adaptive tutors. Here it is a tool an ed-tech team, a coaching institute, a university, or a corporate L&D group can call without hiring a psychometrician.

## Why raw percentages fall short

A percent correct mixes the learner and the paper into one number. A hard form and a weak student can land on the same score. A question everyone finds confusing can look “hard” when it is actually measuring the wrong thing. A click-through on a compliance module can look like mastery.

Four questions usually stay hidden:

- Is this question measuring ability, or confusing everyone?
- What is this learner’s true ability (θ), separate from how difficult the test was?
- What has this learner actually mastered, skill by skill?
- Is the assessment reliable enough to trust the total?

IRT answers the first two. Cronbach’s alpha answers the last. Bayesian Knowledge Tracing answers mastery over time. Computerized adaptive testing uses the ability estimate to pick the next question. Teams that only have a spreadsheet of percentages ship tests they have never checked. Fitting these models by prompting a model is not dependable. The server is the fit.

Used on a real item bank, the review (which questions to drop, whether the form is reliable, where each learner stands) is the work that usually takes a psychometrician. Teams using the tools cut that design and analysis time by about 50–80%.

## Who it is for

Ed-tech platforms, coaching and test-prep institutes, universities and schools, corporate learning and certification teams, and assessment publishers.

- **Test-prep institute.** “Questions 14, 22, and 31 have negative discrimination — they are measuring the wrong thing; drop them. This cohort’s ability rose 0.4σ this month.”
- **Ed-tech product lead.** An adaptive-testing loop (current ability in, next item out) without a psychometrician on staff.
- **Corporate L&D.** Mastery tracking that shows who has not learned the compliance module, as distinct from who clicked through it.

## How to use it

Connect the endpoint in the [install README](../README.md), sign in when the client asks, and send the data in the prompt or as a file the client can read. Use pseudonymous learner ids. Do not send names.

### What you send

| Data | What it looks like | Where it comes from |
|---|---|---|
| **Responses** | One row per learner per item: pseudonymous id, item id, correct or not | CSV, quiz-platform export, LMS export (Moodle, Canvas, or similar) |
| **Item bank** | Question metadata and, once fitted, item parameters | The customer’s own question bank |
| **Learning history** | Repeated attempts on named skills, in time order | Course or practice logs |
| **Cohort trajectory** | Mastery or ability over time for a group | The outputs of `track_mastery` and `estimate_ability`, or the same history |

Send the export in the request. The server does not connect to an LMS on its own in the current release.

### Tools

Listed alphabetically, in the order a client shows them.

| Tool | What you send | What comes back | Do not use it when |
|---|---|---|---|
| `adaptive_next_item` | Current ability (θ) and an item bank | The next question that adds the most information at that θ | You are scoring a finished test, or you have no ability estimate and no item parameters |
| `analyze_assessment` | Response data (learner, item, correct) | Per-item difficulty, discrimination, and guessing, plus flags for bad items | You only have the question text, or a single learner’s percent score |
| `assessment_reliability` | Response data | Cronbach’s alpha and item-total correlations | You want a verdict on one question; use `explain_item` for that. Alpha on a handful of items is unstable |
| `estimate_ability` | Student responses | Each learner’s true ability (θ), separate from test difficulty | The items have not been checked, or you want a public ranking |
| `explain_item` | A question id from a fitted test | Why that item is good or bad, with its parameters | You have not run `analyze_assessment` yet |
| `flag_at_risk_learners` | A cohort’s mastery or ability over time | Learners whose trajectory predicts failure, framed as who needs support | You want a league table, a public label, or a reason to withhold help |
| `track_mastery` | A learning history on named skills | P(mastered) per skill after each attempt | The data is one sitting of a test with no skill labels; use `estimate_ability` for that |

### A typical review

1. **`analyze_assessment`** on the response matrix. Drop or rewrite items with negative discrimination. Getting them right does not mean higher ability.
2. **`assessment_reliability`** on the items you intend to keep. Read alpha together with the item-total correlations.
3. **`explain_item`** for anything a teacher or item writer will ask about.
4. **`estimate_ability`** once the item set is defensible. Compare learners on θ, and read each estimate with its uncertainty. A small response set is a reason to pause, not a reason to treat a point estimate as exact. If there are too few responses to identify the model, the fit should stop and say why.
5. **`track_mastery`** when the data is a skill history rather than one test sitting.
6. **`flag_at_risk_learners`** to see who to support. Keep the result with the educator. Do not publish it as a ranking.
7. **`adaptive_next_item`** only while delivering an adaptive test, with a current θ and an item bank.

### Example prompts

- “Score this exam with IRT.”
- “Which questions are too hard?”
- “What has each student mastered?”
- “Here’s this mock’s response matrix. Which questions have negative discrimination and should be dropped?”
- “Estimate each student’s true ability on this test, not just their raw percentage.”
- “What is Cronbach’s alpha for this paper, and which items correlate poorly with the total?”
- “Given this item bank and a learner currently at θ = 0.4, which question should we give next?”
- “Who needs support before this cohort fails, and what has each person actually mastered?”
- “Explain why item 14 was flagged. Show its difficulty, discrimination, and guessing.”

## The methods

These are the classical models the tools fit. Dates are the sources people cite, not a claim that the server reproduces a particular paper’s software.

### Item Response Theory

Lord (1952) and Rasch (1960). The three-parameter logistic model is:

```text
P(correct | θ) = c + (1 − c) / (1 + e^(−a(θ − b)))
```

| Symbol | Name | Reading |
|---|---|---|
| θ | Ability | The learner’s standing on the same scale as item difficulty. |
| a | Discrimination | How sharply the item separates higher ability from lower. Near zero, it barely separates anyone. Negative means lower-ability learners do better on it, so it is working backwards. |
| b | Difficulty | Where the item sits on the ability scale. Higher means only more able learners are likely to get it right. |
| c | Guessing | A floor on the chance of a correct answer at very low ability. It matters for multiple choice, where a guess can be right. |

The server fits item parameters and person abilities numerically (marginal maximum likelihood). That fit is what makes the tool necessary: convergence, identifiability constraints, and sparse response matrices are where a prompted model breaks. The one-parameter Rasch model is the simpler case, used when a full three-parameter fit is more than the data can support.

`analyze_assessment` returns a, b, and c per item, with flags. `estimate_ability` returns θ. `explain_item` restates one item in plain language.

### Cronbach’s alpha

Cronbach (1951). Internal-consistency reliability of a test score:

```text
α = (k / (k − 1)) · (1 − Σσ²ᵢ / σ²_total)
```

k is the number of items, σ²ᵢ is the variance of item i, and σ²_total is the variance of the total score. `assessment_reliability` also returns item-total correlations, which show whether each item moves with the rest of the score. A low alpha, or an item that correlates poorly or negatively with the total, is a reason to open that item in `analyze_assessment` and `explain_item`. A very easy or very hard item can correlate poorly simply because almost everyone scored the same.

### Computerized adaptive testing

CAT is IRT used one item at a time. Given the current θ, `adaptive_next_item` picks the bank item with the most Fisher information at that θ, so the answer tells you something new. After the response, ability is estimated again and the next item is chosen the same way. It needs an item bank with parameters and a current ability. It does not score a test that has already been given.

### Bayesian Knowledge Tracing

Corbett and Anderson (1994). One hidden Markov model per skill:

| Parameter | Meaning |
|---|---|
| P(init) | Probability the learner already knows the skill before the first attempt. |
| P(learn) | Probability of learning the skill on an attempt when it was not yet mastered. |
| P(slip) | Probability a learner who has mastered it still gets the item wrong. |
| P(guess) | Probability a learner who has not mastered it gets the item right. |

After each response, `track_mastery` updates P(mastered). That is mastery of a named skill across attempts. θ from `estimate_ability` is standing on one test. A learner can have a solid θ and still be unmastered on one skill, or the reverse.

## How to read the results

- **Negative discrimination.** The item is measuring the wrong thing. It is a candidate to drop or rewrite.
- **Poor item-total correlation.** The item does not move with the rest of the test. Confirm with the IRT flag before cutting it.
- **Ability versus percent correct.** A low percentage on a hard form can still be a mid or high θ. Compare learners on θ when the forms differ. Read θ with its uncertainty. Do not treat a small-sample estimate as exact.
- **Mastery versus ability.** Use P(mastered) for a named skill over time. Use θ for a test sitting.
- **At-risk flags.** These are support signals for an educator. They are not a rank, a public label, or a reason to punish or withhold help.

## Data and how flags are framed

Response data is personal data. In schools and test prep it is often data of minors. Send pseudonymous ids. The server processes the request and does not store it. Each user signs in with OAuth. Your own duties under India’s Digital Personal Data Protection Act, the GDPR, or any other law that applies to your learners stay with you.

At-risk and ability results are for the educator who can offer support. Do not expose individual scores in a way that shames a learner, and do not publish them as a ranking.

Privacy policy: https://saifs.ai/privacy_policy

## Plans

| Tier | What it covers |
|---|---|
| **Free** | Reliability (alpha) and basic item analysis on one assessment |
| **Pro** | ₹4,100 / $49 per month — full IRT item analysis, ability estimation, multiple assessments |
| **Business** | ₹16,500 / $199 per month — adaptive testing (CAT), knowledge tracing, at-risk flags, LMS pulls |
| **Enterprise** | Assessment-publisher volume and custom item-bank calibration |

The free tier is the way to try a fit without a sales call. Current terms are on [saifs.ai](https://saifs.ai).

## Support

- Install and client setup: [server README](../README.md)
- Website: https://saifs.ai
- Registry: https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
