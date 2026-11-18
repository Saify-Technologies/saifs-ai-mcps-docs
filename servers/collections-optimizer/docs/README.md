# Collections Optimizer

[SAIFS AI](https://saifs.ai) (`saifs.ai`) publishes this as a remote MCP server under the registry name `ai.saifs/collections-optimizer`. It builds the morning collections queue for the creditor’s own recovery team: who to contact, who to leave alone, and which channel and time stay inside the contact rules.

People search for this as collections prioritization, debt collection, dunning, accounts receivable, roll rate, or payment propensity. The questions are “who to chase first” and “the collections list.” The short install page is the [server README](../README.md). This page is what the queue is, why oldest-first wastes the day, and how to use each tool.

| | |
|---|---|
| **Registry name** | `ai.saifs/collections-optimizer` |
| **Endpoint** | `https://collections-optimizer-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## What it is

Decision support for a collections operation that already owns the receivables book: banks, NBFCs, MFIs, BNPL, telecom, utilities, and B2B trade-credit desks. The creditor’s agents make the contact. This server ranks the work and keeps every recommendation inside the hour limits, frequency caps, and cease-contact flags you send.

Eight tools cover the day:

- The chance an account pays, the chance it pays with no contact, and the expected recovery.
- A work list ordered by the extra recovery a contact is likely to produce, not by age or balance alone.
- A channel, a time, and a tone-tier (call, WhatsApp, or notice) for that account.
- Accounts to skip because they are likely to self-cure.
- Bucket-to-bucket roll and cure rates for provisioning.
- An assignment of accounts to agents.
- A written reason for the rank, for the agent and for the file.
- A fresh queue when you send the book again the next morning.

When the question is which week cash arrives, that is the [13-Week Cash Flow Forecast](../../cash-forecast/README.md). This server is who, today, is worth a contact.

The call is stateless. Send the overdue book, the payment history, and the contact-outcome history in the request. The server processes them and does not store the book. You keep the queue, the explanation, and the audit record.

Teams that work the uplift queue, with the same agents, see recoveries move by about 5–15%.

## Why oldest-first misses the work

Agents work overdue accounts every day. The usual rule is oldest first, or largest balance first. That spends the morning on accounts that would have paid anyway, and on accounts that will not pay, and it misses the moveable middle: people whose chance of paying actually changes because someone contacted them at the right time, on a channel they answer.

Propensity-to-pay and contact strategy have been in large-bank collections platforms for years. Mid-market lenders, NBFCs, and telcos often still run the book from a spreadsheet. A language model cannot hold each account, score it the same way tomorrow, and rebuild the queue before the shift starts.

## How to use it

Connect the endpoint in the [install README](../README.md) and sign in when the client asks. Use pseudonymous account references. The creditor owns the contact. Send the rules that contact must follow, and the tools will not recommend anything outside them.

A morning run:

1. **`watch_queue`** or **`prioritize_queue`** on today’s overdue export. The list is ordered by expected recovery uplift from a contact.
2. **`flag_self_cure`** for the accounts to skip. Leaving them alone saves the hour and avoids an unnecessary contact.
3. **`recommend_contact_strategy`** for the accounts the agent will actually work. Channel, time, and tone-tier come back inside the hours and frequency cap.
4. **`explain_priority`** when an agent or a reviewer asks why this account is on the list.
5. **`optimize_agent_allocation`** when a team lead is splitting the queue across agents.
6. **`predict_roll_rate`** when the recovery head needs cure and roll rates for provisioning. **`score_propensity`** is the single-account view of the same book.

### What you send

Overdue accounts and payment history from the collections system or the receivables export. Contact-outcome history (what was tried, and whether a payment followed) is what lets the uplift rank improve. You send it each time. It is not kept on the server.

| Field | What it is |
|---|---|
| `account_ref` | A pseudonymous id. Not a name. |
| `days_past_due` | How late the account is. |
| `amount` | The overdue amount. |
| `prior_payment` | Recent payment behaviour. |
| `promise_kept` | Whether earlier payment promises were kept. |
| `contactability` | Whether this account can be reached on the channels you use. |
| `product_type` | The product or book the account sits in. |
| `contacts_recent` | Contacts already made, so a frequency cap can be applied. |
| `permitted_hours` | The hours your rulebook allows. |
| `cease_contact` | True when this account must not be contacted. |

```json
{
  "account_ref": "A-1042",
  "days_past_due": 34,
  "amount": "18500",
  "prior_payment": "partial_last_cycle",
  "promise_kept": true,
  "contactability": ["call", "whatsapp"],
  "product_type": "personal_loan",
  "contacts_recent": 2,
  "permitted_hours": { "start": "08:00", "end": "19:00" },
  "cease_contact": false
}
```

### What a morning sounds like

- “Work these 40 accounts in this order. Skip these 15. They are likely to self-cure. Call this segment between 6 pm and 8 pm. WhatsApp the rest.”
- “Account A-1042 is on the list because a contact changes the chance of payment. The time is inside the hours you allowed. Two contacts are already on the file, so the cap still has room.”
- “This week’s roll rates, for the provisioning pack.”

### Prompts

- “Who should we chase for payment today?”
- “Which overdue accounts are likely to pay?”
- “Plan this week’s collection calls.”
- “Here’s our overdue book. Who should agents work today, and who will likely self-cure?”
- “For account A-1042, what’s the best channel and time to contact, within permitted hours?”
- “Allocate this queue across 8 agents by who recovers which product best.”
- “Why is this account third on the list?”

## Tools

| Tool | What you send | What comes back | Do not use it when |
|---|---|---|---|
| `explain_priority` | One account and the queue or score it came from | Why this account, and why this strategy, in language an agent and an auditor can file | You want a new rank for the whole book. Run `prioritize_queue` first |
| `flag_self_cure` | Overdue accounts and payment history | Accounts likely to pay with no contact | You want those accounts dialled anyway. The point of the flag is to skip them |
| `optimize_agent_allocation` | Agents (skill and success history) and a queue | An assignment of accounts to agents | You are hiring, ranking people for performance management, or handing an account to someone outside the collections team |
| `predict_roll_rate` | A portfolio with bucket and history | The probability an account cures or rolls to the next bucket | You want today’s call list. That is `prioritize_queue` |
| `prioritize_queue` | Overdue accounts, history, and the contact caps | A ranked daily work list by expected recovery uplift from contact | You only want the oldest balance, or the largest balance, with no view of who a contact can still move |
| `recommend_contact_strategy` | One account, its history, permitted hours, recent contact count, and any cease-contact flag | Channel, timing, and tone-tier: call, WhatsApp, or notice | The account is cease-contact, the hour is outside the permitted window, or the frequency cap is already met. The tool will not return a contact in those cases |
| `score_propensity` | One account’s data | P(will pay), P(self-cure), and expected recovery | You want the ordered list for the shift. Score one account to explain it, then queue the book |
| `watch_queue` | The latest portfolio export | A re-scored queue for the next morning | You expect the server to remember yesterday’s book. Send the export again. Nothing is stored |

## How an account is ranked

### Propensity to pay

A logistic scorecard:

```text
P(pay) = 1 / (1 + e^(−(β₀ + Σβᵢxᵢ)))
```

The features are the ones you can defend on a collections file: days past due, amount, prior payment behaviour, promises kept, contactability, and product type. `score_propensity` returns that probability, plus the probability the account pays with no contact, plus expected recovery. It is a likelihood, not a promise that the money arrives.

### Self-cure and roll rate

Kaplan-Meier cure curves, and Cox hazards, answer two related questions. Will this account cure if nobody calls? And, looking across the book, what share of a bucket rolls forward or cures? `flag_self_cure` is the first question, so agents skip work that does not need them. `predict_roll_rate` is the second, for provisioning. An open account is not treated as a permanent loss, and it is not treated as paid on the due date.

### Uplift, not just who will pay

The queue is ordered by whose payment probability increases most if they are contacted. That is the moveable middle. An account that will pay anyway, and an account that will not pay, both sit below an account a contact can still change. The framing is a Markov decision over the states an account actually moves through: current, 30 days past due, 60 days past due, cured, charged off. Contact is an action with a cost. The reward is recovery. The recommended action is the one with the best expected uplift inside the contact rules.

Champion-challenger support on the Business tier lets two strategies run on comparable slices of the book so a team lead can see which one recovers more, instead of switching the whole floor on a hunch.

### Agent assignment

`optimize_agent_allocation` assigns accounts to agents with the Hungarian method, matching the queue to skill and to who has recovered that kind of product before. It splits today’s work. It is not a performance score for HR.

## Contact rules are part of the result

The creditor owns every contact and has to follow the fair-collection rules that apply to that book. In India that includes the Reserve Bank of India’s Fair Practices Code and the digital-lending and recovery-agent guidelines: permitted hours, no harassment, frequency caps, and a grievance trail. For a US book, the Fair Debt Collection Practices Act and Regulation F set the same kind of limits, including the 7-contacts-in-7-days rule, permitted hours, and cease-contact.

Those limits are inputs the optimizer will not cross.

- No recommended contact outside the permitted hours you send.
- No recommended contact above the frequency cap, counted from `contacts_recent`.
- A `cease_contact` flag produces no channel and no time.
- Tone-tier stays on call, WhatsApp, or notice. The tool does not write a script, a threat, or a public disclosure.
- `explain_priority` is the record you file. The server does not keep it.

This enables a queue that stays inside the rules you operate under. It does not certify that the creditor is compliant. Conduct, complaints, and the legal record stay with the creditor.

Skipping likely self-cures is part of the same design. Fewer contacts that were never needed is better for the agent’s hour and for the person who was going to pay.

## How to read a result

- **Uplift rank.** Place on the list is “a contact here changes the outcome,” not “this is the largest balance” or “this is the oldest.”
- **P(will pay) and P(self-cure).** Both are probabilities. A high self-cure probability is a reason to skip, which is what `flag_self_cure` is for.
- **Expected recovery.** An amount weighted by those probabilities. It is not cash already received.
- **Channel and time.** Usable only inside the hours and the cap. If the tool returns no contact, the rules blocked it. Do not work around that in the conversation.
- **Roll rates.** A forecast for provisioning, with the uncertainty of a survival curve on the history you sent. Thin history should be read as thin.
- **Audit line.** Keep `explain_priority` with the day’s file. That is the trail. The server does not store the book.

## Data and what this will not do

Debtor and account data is sensitive personal and financial data. Send pseudonymous references. The server processes the request and does not store it. Each user signs in with OAuth. Your own duties under India’s Digital Personal Data Protection Act, the GDPR, or any other law that applies to the book stay with you.

The server will not recommend contact outside permitted hours, over a frequency cap, or against a cease-contact flag. It will not draft harassment, threats, or a public shaming message. It will not guarantee a recovery rate. It will not replace the creditor’s compliance officer.

Privacy policy: https://saifs.ai/privacy_policy

## Plans

| Tier | What it covers |
|---|---|
| **Pro** | ₹16,500 / $199 per month — propensity scoring, the daily prioritized queue, and self-cure flagging |
| **Business** | ₹66,000 / $799 per month — uplift and contact strategy, agent allocation, champion-challenger, roll-rate forecasting, and scheduling inside the contact caps |
| **Volume** | Per-account or per-recovery metering, and success-linked pricing, agreed for larger books |
| **Enterprise** | Banks and other regulated lenders, on a private offer after a deployment review |

Current terms are on [saifs.ai](https://saifs.ai).

## Support

- Install and client setup: [server README](../README.md)
- When the cash actually arrives: [13-Week Cash Flow Forecast](../../cash-forecast/README.md)
- Website: https://saifs.ai
- Registry: https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
