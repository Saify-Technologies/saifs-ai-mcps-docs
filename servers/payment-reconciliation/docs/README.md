# Payment Reconciliation & Matching

[SAIFS AI](https://saifs.ai) (`saifs.ai`) publishes this as a remote MCP server under the registry name `ai.saifs/payment-reconciliation`. It proposes which receipts, bank lines, and GSTR-2B rows belong to which open invoices and ledger entries, with a confidence and a reason on every proposal.

A proposal is not a posting. Low-confidence matches go to a person. This server never posts a journal, never connects to a bank, and never connects to the GST portal. The accountant posts.

People search for this as bank reconciliation, payment matching, cash application, accounts-receivable automation, GSTR-2B reconciliation, or invoice matching. The questions are “match payments to invoices” and “reconcile the bank.” The short install page is the [server README](../README.md). This page is what a match is, why an exact amount is not enough, and how to send the lines.

| | |
|---|---|
| **Registry name** | `ai.saifs/payment-reconciliation` |
| **Endpoint** | `https://payment-reconciliation-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## What it is

You send bank lines or receipts, the open items, and an optional `rules_snapshot` from the last run. You get back proposed matches, the deductions those matches assume, an exceptions queue, a report, and an updated snapshot to store.

| Tool | What you ask |
|---|---|
| `match_receipts` | Which open invoices does this receipt pay, including a combination, a part payment, or a TDS net? |
| `match_bank_to_ledger` | Which ledger entries does this statement line clear, one-to-many or many-to-one? |
| `gst_2b_match` | Which purchase-register rows agree with GSTR-2B, and where is input tax credit at risk? |
| `explain_match` | Why did these particular items get paired? |
| `reconciliation_report` | What share matched, what value is still open, how old are the exceptions, and how much ITC is at risk? |
| `suggest_rules` | What aliases and tolerances did today’s confirmations teach, for the snapshot I will send tomorrow? |

The call is stateless. Learned aliases, TDS rates, and tolerances live in the snapshot you keep. Nothing is stored here. Teams that start from this queue, instead of from a blank Excel match, cut the manual matching effort by about 60–85%. The queue is the start. The posting is still theirs.

When the question is which week the cash will arrive, rather than which invoice this receipt already belongs to, that is the [13-Week Cash Flow Forecast](../../cash-forecast/README.md).

## Why exact amount plus reference leaves the day unfinished

Tally, Zoho, and an ERP will pair a line when the amount and the reference are identical. The hour goes into the rest:

- One payment covering three invoices.
- A part payment against a larger bill.
- A receipt that is the invoice total after TDS, or after a bank charge.
- A payer string that is not the customer master. `NORTH TRDRS` and `North Traders Pvt Ltd` are the same party once the legal suffix and the punctuation are stripped.
- An invoice number typed with a space, a dash, or a missing character.

Those cases are a search, not a lookup. The search proposes. A person confirms anything that is not exact.

## How to use it

Connect the endpoint in the [install README](../README.md) and sign in when the client asks. Export receipts or the bank statement, and the open invoices or the ledger. For GST, export GSTR-2B and the purchase register yourself. This server does not log in to either system.

```json
{
  "receipts": [
    {
      "id": "R-8841",
      "date": "2026-11-12",
      "amount": "27720.00",
      "narration": "UPI/NORTH TRDRS/INV 1042 1048",
      "payer_name": "NORTH TRDRS",
      "ref": "1042"
    }
  ],
  "open_items": [
    {"doc_id": "INV-1042", "party_ref": "C-18", "party_name": "North Traders Pvt Ltd", "date": "2026-10-28", "amount": "18000.00", "balance": "18000.00"},
    {"doc_id": "INV-1048", "party_ref": "C-18", "party_name": "North Traders Pvt Ltd", "date": "2026-11-02", "amount": "10000.00", "balance": "10000.00"}
  ],
  "rules_snapshot": {
    "aliases": [],
    "tds_rates": [0.01, 0.02, 0.10],
    "tolerance": "1.00"
  }
}
```

Use party references you can join on your side. A proprietor’s name is personal data. Where the reply can carry `party_ref` instead of the name, it does.

Amounts are matched in paise, as integers, so a floating-point rupee does not invent a one-paisa gap. A tolerance you set, here ₹1, is the only slack.

### What the confidence means

| Situation | Confidence | What you do |
|---|---|---|
| Reference and amount both agree | About 0.99 | Still your posting. The reason is exact. |
| A combination of invoices, plus a strong name | About 0.85 to 0.95 | Read the combination, then post if you agree. |
| Name only, no amount identity | Below 0.70, labelled suggested | A person decides. It is not a match until you confirm it. |

Fuzzy pairs stay suggested until a confirmation is fed to `suggest_rules` and the new alias comes back inside `rules_snapshot`. Exact matching has no minimum volume. A thin file still returns the exact pairs. It does not promote a weak name into a confident match to fill the page.

An AR day in this shape is 640 receipts, about 571 proposed with high confidence, about 52 suggested, and about 17 left as exceptions. Those counts are an example of the split. Your file produces its own. The point of the split is that the 17 are the work, and the suggested 52 are a queue rather than a posting.

### Receipts and bank lines

For each payment the candidate invoices are the ones whose party score clears the threshold and whose date sits in the window. The default window is an invoice dated on or before the payment, plus five days. Both are in the snapshot, so you can widen them.

Combination matching looks for a subset of those invoices whose total equals the payment within tolerance. The search is subset-sum over paise, and it stops at eight invoices in one match unless you set another cap. When several subsets fit, they are ranked by oldest invoice first, then fewest invoices, then a reference that actually appears in the narration.

If the payment is short of the invoice total, the matcher tries

```text
payment = (sum of invoices) × (1 − TDS rate) − bank charge
```

using the TDS rates in the snapshot, commonly 1%, 2%, and 10%, and the charge band you allow. A fit is a deduction, named as TDS or as a charge. A short payment that fits none of those rates is a dispute, not a match.

When several receipts compete for the same invoices, the assignment is global. A Hungarian (min-cost) assignment maximises the total score under one constraint: an invoice is used at most once. `match_bank_to_ledger` is the same engine on statement lines and ledger entries, and it allows many-to-one as well as one-to-many.

`explain_match` restates one proposal as the amount combination, the name score, the date gap, and the reference hit. That is the audit line you file with the posting.

### GSTR-2B

`gst_2b_match` compares the supplier’s GSTR-2B rows with your purchase register, inside the tolerance you send. Each row lands in one of four places: matched, mismatched on amount or date or GSTIN, missing in your books, or missing in 2B. Input tax credit on the unmatched and mismatched rows is the ITC at risk. A monthly file for a practice can come back with that credit in a range, for example about ₹3.2–3.6 lakh, and the supplier list beside it. The range is the uncertainty of the tolerance and the open mismatches, not a tax computation. The associate still files.

`reconciliation_report` rolls the proposals up: matched percent, unmatched value, how old the exceptions are, and ITC at risk when the GST tool was in the run.

### Rules you keep

`suggest_rules` reads the matches you confirmed. It returns aliases (`NORTH TRDRS` = `C-18`), and any tolerance you have now accepted, inside an updated `rules_snapshot`. Send that snapshot tomorrow. The server does not remember it.

### Prompts

- “Match today’s receipts to open invoices.”
- “Reconcile the bank statement.”
- “Run GSTR-2B vs purchase register.”
- “This receipt is ₹27,720 from NORTH TRDRS. Which open invoices does it cover after 2% TDS?”
- “Why was receipt R-8841 paired with INV-1042 and INV-1048?”

## Tools

| Tool | What you send | What comes back | Do not use it when |
|---|---|---|---|
| `explain_match` | A match id from a proposal you just received | The amount combination, the name score, the date gap, and the reference | You have not run a match yet |
| `gst_2b_match` | `gstr2b_rows`, the purchase register, and a tolerance | Matched, mismatched, missing in books, missing in 2B, and ITC at risk | You want the return filed or the credit claimed. This is the difference list. You do not connect this server to the GST portal |
| `match_bank_to_ledger` | Bank lines, ledger entries, and rules | Proposed one-to-many and many-to-one matches, with confidence, plus the lines that did not clear | You want the statement fetched from the bank. Paste or export the lines |
| `match_receipts` | Receipts, open invoices, and an optional `rules_snapshot` | Proposed applications, including combinations, part payments, and TDS or charge deductions, plus exceptions | You want a suggested name-only pair posted automatically. Below 0.70 it stays in the human queue |
| `reconciliation_report` | The results of a match you just ran | Matched percent, unmatched value, ageing of exceptions, and ITC at risk when GST was in the run | You want a new match. This only summarises one |
| `suggest_rules` | The matches a person confirmed | An updated `rules_snapshot` of aliases, TDS rates, and tolerances | You want those rules applied to a book the person has not confirmed. Unconfirmed pairs do not become aliases |

## How a name is scored

The string is upper-cased. Legal suffixes such as PVT LTD and LLP, and the punctuation, are removed. What remains is compared with Jaro-Winkler and with the overlap of the tokens. `NORTH TRDRS` against `NORTH TRADERS` is a high token score. An alias already in the snapshot overrides the score and treats the two strings as the same party. A UPI narration is read as text. It is not a live lookup.

## How to read a result

- **Confidence is the instruction.** Near 0.99, the reference and the amount agree. Between 0.85 and 0.95, check the combination. Below 0.70, it is a suggestion. Do not post a suggestion because the queue is long.
- **A deduction is named.** TDS at a rate from the snapshot, or a bank charge inside the band, is a reason. A short payment with no such reason is an exception, for the collector or the vendor, not a forced match.
- **One invoice, one use.** If a second receipt wanted the same bill, the assignment already gave it to the stronger score. The loser is in the exceptions, not double-applied.
- **ITC at risk is not a tax filing.** It is the credit sitting on rows that do not agree. The GST return is prepared by the person who signs it.
- **The snapshot is the memory.** Without the aliases you confirmed yesterday, tomorrow’s `NORTH TRDRS` is scored from scratch again.
- **This is not accounting advice and not tax advice.** The entries, the TDS section, and the credit you claim are the accountant’s.

## Data and what this will not do

Bank narrations and proprietor names can be personal data. The request is processed and discarded. Party references are preferred in the reply. Each user signs in with OAuth. Your own duties under India’s Digital Personal Data Protection Act, the GDPR, or any other law that applies to the export stay with you.

The server will not connect to a bank or to the GST portal, will not post a voucher, will not treat a name-only suggestion as cleared, and will not use the same invoice twice. It does not compute interest, and it does not advise when to pay or what cash discount to take. It matches the lines you send.

Privacy policy: https://saifs.ai/privacy_policy

## Plans

| Tier | What it covers |
|---|---|
| **Free** | 200 lines a month |
| **Pro** | ₹2,500 / $29 per month — 5,000 lines, GSTR-2B mode, and learned rules |
| **Business** | ₹8,300 / $99 per month — 50,000 lines, more than one entity, and bank, receivables, and payables together |
| **CA firm** | Priced per client GSTIN, in volume tiers |
| **Overage** | Per further 1,000 lines |

The free tier is enough to match one day’s receipts and read the confidence split, with no sales call. Current terms are on [saifs.ai](https://saifs.ai).

## Support

- Install and client setup: [server README](../README.md)
- When unpaid invoices are likely to become cash: [13-Week Cash Flow Forecast](../../cash-forecast/README.md)
- Website: https://saifs.ai
- Registry: https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
