# Mastery Pathing

[SAIFS AI](https://saifs.ai) (`saifs.ai`) publishes this as a remote MCP server under the registry name `ai.saifs/mastery-pathing`. It answers what this learner should study next: the prerequisite structure of the subject, their mastery of each topic, and a sequence that respects both.

People search for this as adaptive learning, a learning path, knowledge space theory, a prerequisite graph, personalized learning, or mastery learning. The question is “what should the student learn next.” The short install page is the [server README](../README.md). This page is what the frontier means, why a linear syllabus is not a path, and how to send a graph.

| | |
|---|---|
| **Registry name** | `ai.saifs/mastery-pathing` |
| **Endpoint** | `https://mastery-pathing-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## What it is

You send the curriculum. Topics, the prerequisite links you authored, a pseudonymous learner id, and the responses since last time. You may also send the `mastery_snapshot` this server returned on the previous call. You get back mastery per topic, the knowledge frontier, the next topic, the gaps in front of a goal, a path, and a new snapshot to store.

The curriculum stays yours. The server reasons over the graph you send and does not keep a copy to hand to anyone else.

Seven tools cover that loop.

| Tool | What you ask |
|---|---|
| `build_knowledge_graph` | Is this prerequisite list a valid order, or does it contain a cycle or a hole? |
| `estimate_mastery` | What is P(mastered) on each topic, from these responses? |
| `next_best_topic` | What are they ready for now: prerequisites met, topic not yet mastered? |
| `diagnose_gaps` | Which missing prerequisites block this goal, and in what order should they be filled? |
| `optimal_sequence` | What is a valid path from here to the goal? |
| `rate_difficulty` | Where do this learner and this item sit on one difficulty scale? |
| `explain_recommendation` | Why this topic, and which responses support it? |

The call is stateless. The snapshot is yours. The next call sends it back, and `responses` can be only the new ones. A snapshot from a different `model_version` is refused. Learner ids are pseudonymous. Do not send a name, an age, or a school.

For the same curriculum, a path that follows prerequisites is associated with reaching mastery in on the order of 20–40% less time than working the syllabus in a fixed line. That is a range across courses, not a promise for one learner. The recommendation says what to learn next. It does not say the learner is weak, and it is not a leaderboard.

Two neighbouring servers answer the other two questions. Whether the questions are sound, and what ability they measure, is [Assessment Intelligence](../../assessment-intelligence/README.md). When to review an item so it is not forgotten is [Learning Optimizer](../../learning-optimizer/README.md).

## Why the next chapter is often the wrong one

A subject is a graph. Adding fractions waits on fraction basics. A linear course can still assign adding fractions to someone who has not mastered the basics, and it can hold someone on basics they already have. Knowledge Space Theory (Doignon and Falmagne, 1985) names the learnable states and the outer fringe: the topics just outside what the learner already knows. Bayesian Knowledge Tracing (Corbett and Anderson, 1994) updates P(mastered) after each response. A topological order of the prerequisite graph is a sequence that does not teach an effect before its cause.

Holding that graph and that mastery between sessions is the part a chat model cannot do. Your app holds the snapshot. This server is the update.

## How to use it

Connect the endpoint in the [install README](../README.md) and sign in when the client asks.

```json
{
  "graph": {
    "topics": [
      {"id": "frac_add", "name": "Adding fractions"},
      {"id": "frac_basics", "name": "Fraction basics"}
    ],
    "prerequisites": [
      {"topic": "frac_add", "requires": "frac_basics"}
    ]
  },
  "learner_id": "L-2041",
  "responses": [
    {"topic": "frac_basics", "item": "q12", "correct": true, "ts": "2026-10-17T10:02:00Z"},
    {"topic": "frac_basics", "item": "q15", "correct": false, "ts": "2026-10-17T10:05:00Z"}
  ],
  "mastery_snapshot": null,
  "goal_topic": "frac_add",
  "params": {"mastery_threshold": 0.95}
}
```

On two responses, a result looks like this. Fraction basics is still learning, P(mastered) about 0.71. The frontier is fraction basics. The next topic is fraction basics, because it is a prerequisite of the goal and it is not mastered. The path is fraction basics, then adding fractions. The confidence note says the estimate is Bayesian Knowledge Tracing on two responses and will sharpen. Store `mastery_snapshot` and `model_version`. Send them next time.

`build_knowledge_graph` is the check to run before any learner call. A cycle is named, not silently broken. A topic that requires a topic you did not define is flagged as a gap in the graph, which is an authoring error, not a learner gap.

### Prompts

- “What should this learner study next?”
- “Path to master fractions.”
- “Find gaps blocking algebra.”
- “Here’s our syllabus with prerequisites. What should this student study next?”
- “They want to reach quadratic equations. Which gaps are blocking that, and in what order should we remediate?”
- “Build a learning sequence from their current mastery to the chapter on recursion.”

## Tools

| Tool | What you send | What comes back | Do not use it when |
|---|---|---|---|
| `build_knowledge_graph` | Topics and prerequisite links | A validated directed acyclic graph, or the cycle named, plus links that point at a missing topic | You want a learner’s next topic. The graph has no learner in it |
| `diagnose_gaps` | The graph, a learner snapshot or responses, and a goal topic | The unmastered prerequisites that block the goal, and a remediation order | The goal is already on the frontier. That is `next_best_topic` |
| `estimate_mastery` | The graph, responses, and an optional snapshot | P(mastered) and a status per topic, plus how many responses support each estimate | You want the path. Mastery is the evidence. The path is `optimal_sequence` |
| `explain_recommendation` | The learner and the recommendation just made | Why this topic, with the mastery evidence and the response count | You have not asked for a next topic yet |
| `next_best_topic` | The graph, the learner’s snapshot or responses, and an optional goal | The frontier topic to study now. With a goal, that is the unmastered ancestor with the lowest mastery | You want the full path to a distant goal. That is `optimal_sequence` |
| `optimal_sequence` | The graph, the current mastery, and a goal | A topological path through the unmastered prerequisites to the goal | The graph has a cycle. Fix it with `build_knowledge_graph` first. A cycle has no valid order |
| `rate_difficulty` | Item responses: item id, correct or not, and the learner | An Elo rating for the learner and for the item, on one scale | You want P(mastered) of a topic. Elo is the fast rating. Topic mastery is `estimate_mastery` |

## The methods

### The frontier

A topic is on the knowledge frontier when every prerequisite is mastered and the topic itself is not. That is the outer fringe in Knowledge Space Theory. `next_best_topic` picks from that fringe. If you named a goal, the pick is restricted to topics that stand between the learner and the goal, lowest mastery first, so the work moves toward the goal instead of sideways into an unlocked but unrelated topic.

Mastery uses the threshold you send. The example uses 0.95. An estimate under the threshold, or backed by fewer responses than the minimum, stays “learning.” The server does not declare mastery to make a path look finished. The confidence note carries the response count.

### The graph

Prerequisites must form a directed acyclic graph. A topological sort is a legal teaching order: no topic appears before something it requires. `optimal_sequence` is that order, restricted to the unmastered prerequisites of the goal. `diagnose_gaps` is the same set, named as the blockers.

### Bayesian Knowledge Tracing

Each topic is a small hidden Markov model. The defaults, overridable per topic, are:

| Parameter | Default | Meaning |
|---|---|---|
| P(L0) | 0.30 | Probability the topic was already mastered before the first response |
| P(T) | 0.10 | Probability of learning it on an opportunity where it was not yet mastered |
| P(G) | 0.20 | Probability of a correct answer without mastery |
| P(S) | 0.10 | Probability of a wrong answer despite mastery |

A parameter set with P(G) + P(S) at or above 1 is rejected. Guessing and slipping that large make a correct answer uninformative. After each response the posterior P(mastered) updates. Two responses leave a wide estimate. The note says so. The same tracing, used to read a test rather than to choose the next topic, is what [Assessment Intelligence](../../assessment-intelligence/README.md) applies to an assessment.

### Elo

For a cold start, and for picking an item inside a topic, learner ability and item difficulty share one scale. After a response,

```text
R_new = R + K · (actual − expected)
```

Actual is 1 or 0. Expected is the logistic of the gap between the learner’s rating and the item’s. Both ratings move. Elo settles from the first answers. It is not a substitute for the topic posterior once there are enough responses for Bayesian Knowledge Tracing. `rate_difficulty` is this scale. It does not by itself choose the next topic.

Learning Factors Analysis, on the Business tier, checks whether an item actually exercises the topic you attached it to. A mismatch is a curriculum correction, not a judgment of the learner.

## How to read a result

- **The frontier is permission, not a grade.** Prerequisites are met. The topic is not mastered yet. That is why it is next.
- **Read the response count** before you treat P(mastered) as stable. “BKT on 2 responses” means the number will move.
- **A named cycle is an authoring fix.** Do not ask for a path until the graph is acyclic.
- **Gaps are topics, not people.** The remediation list is what to teach next on the way to the goal.
- **Status stays “learning”** until the threshold and the minimum count are both met. Do not rewrite that as a label on the child.
- **Store the snapshot.** The following call without it refits from whatever responses you send, and a snapshot stamped with another model version is refused.

## Data and what this will not do

Responses are personal data, and in school and test prep they are often data of minors. Send a pseudonymous id and the graph. Do not send names, ages, or school details. The server computes, returns the snapshot, and discards the request. Each user signs in with OAuth. Consent and the decision to hold the snapshot stay with the app. Your duties under India’s Digital Personal Data Protection Act, the GDPR, or any other law that applies stay with you. The graph is your curriculum. It is not republished.

The server will not rank learners, will not call a learner weak, and will not declare mastery from a handful of answers. It will not invent a prerequisite you did not write. It will not break a cycle quietly. It will not tell you when to review. That is the learning optimizer. It will not tell you whether the item is a fair measure of the topic. That is assessment intelligence.

Privacy policy: https://saifs.ai/privacy_policy

## Plans

| Tier | What it covers |
|---|---|
| **Free** | A knowledge graph of up to 50 topics, and basic mastery |
| **Pro** | ₹8,200 / $99 per month — Bayesian Knowledge Tracing, the next topic, gap diagnosis, and larger graphs |
| **Business** | ₹24,900 / $299 per month — the full path, Elo difficulty, a check that items match their topics, and a large learner base |
| **Platform** | Ed-tech products that embed the path engine, priced per monthly active user, with a white-label option |

The free tier is enough to validate a small syllabus and see a frontier, with no sales call. Current terms are on [saifs.ai](https://saifs.ai).

## Support

- Install and client setup: [server README](../README.md)
- Whether the items measure the topic: [Assessment Intelligence](../../assessment-intelligence/README.md)
- When to review so the topic is retained: [Learning Optimizer](../../learning-optimizer/README.md)
- Website: https://saifs.ai
- Registry: https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
