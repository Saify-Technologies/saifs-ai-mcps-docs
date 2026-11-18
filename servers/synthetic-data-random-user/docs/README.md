# Synthetic Data — Random User

[SAIFS AI](https://saifs.ai) (`saifs.ai`) publishes this as a remote MCP server under the registry name `ai.saifs/synthetic-data-random-user`. It generates fake user profiles for a demo, a UI fixture, or a QA run, so the screen is full and no real person is in the file.

People search for this as synthetic data, a fake user generator, test data, mock data, or a random user API. The questions are “give me sample users” and “fake customers for a demo.” The short install page is the [server README](../README.md). This page is what a profile contains, why a copy of production is the wrong fixture, and how to ask for a count.

| | |
|---|---|
| **Registry name** | `ai.saifs/synthetic-data-random-user` |
| **Endpoint** | `https://synthetic-mint-mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## What it is

One tool, `random_users`. You send how many profiles you want. You get that many synthetic people: name, gender, location, email, login, date of birth, phone, picture, and nationality.

The records are invented. They are not customers from a CRM, not a sample drawn from your production table, and not a real person you can look up. An email or a phone in the reply is a fixture. Do not send mail to it, do not dial it, and do not treat it as a contact who consented.

The call is stateless. The count is processed and the profiles are returned. Nothing is stored. The point of the tool is that a test environment and a demo tenant can look populated without anyone copying personal data into them.

## Why a paste of production fails the test

QA, a sales demo, and a UI mock all need rows. The rows in production are personal data under India’s Digital Personal Data Protection Act, the GDPR, or both. Copying them into a laptop, a slide, or a shared staging database is the leak. Hand-typed names go stale and still tempt someone to use a real colleague.

`random_users` is the fixture. It is not a statistical twin of your orders, and it does not come with a fidelity report or a privacy certificate. Those are a different product. This one fills a contacts screen.

## How to use it

Connect the endpoint in the [install README](../README.md) and sign in when the client asks. Ask for a count.

```json
{ "count": 20 }
```

Each profile comes back with:

| Field | What it is for |
|---|---|
| Name | A label on a card or a row |
| Gender | A fixture field, not a record about a person |
| Location | City and country text for the layout |
| Email | A synthetic address for a form |
| Login | A username for a sign-in mock |
| Date of birth | A date field in the form |
| Phone | A synthetic number for the phone column |
| Picture | A placeholder image URL for an avatar |
| Nationality | A label, not a citizenship check |

Twenty profiles fill a contacts demo. Five profiles fill a QA fixture. A directory mock uses the names and the pictures. Label the screen or the file as sample data so a later reader does not import the rows as customers.

### Prompts

- “Generate 20 fake users for my demo.”
- “Create sample contacts for testing.”
- “Fill this table with test profiles.”
- “Give me 20 random users to fill our contacts demo screen.”
- “Generate 5 test profiles with emails and phone numbers for our QA fixtures.”
- “I need placeholder avatars and names for a team directory mockup.”

## The tool

| Tool | What you send | What comes back | Do not use it when |
|---|---|---|---|
| `random_users` | A count | That many synthetic profiles, with the fields above | You need real customers, a copy of production, a government identifier, a KYC document, a GSTIN, a payment credential, or a certificate that the rows match a live table. This tool does not do those |

## How to read a result

- **Every row is synthetic.** A familiar-looking name is a collision with the language, not a match to a customer.
- **The picture is a placeholder.** It is not a photo of the named profile as a real person.
- **The email and the phone are not inboxes.** Do not use them as a destination.
- **The file is for demo, fixture, and prototype.** It is not training data certified against your production distribution, and it is not a dataset an auditor can treat as attested.
- **Say so in the UI.** “Sample data” on the screen is what stops the next person from treating the export as the CRM.

## Data and what this will not do

You do not send personal data to get profiles back. You send a count. The server does not look anyone up, does not read your customer file, and does not store the profiles. Each user signs in with OAuth.

The server will not return a real person, will not clone a production extract, and will not produce an identity document, a tax identifier, or a payment instrument. It will not claim the rows are indistinguishable from your customers.

Privacy policy: https://saifs.ai/privacy_policy

## Support

- Install and client setup: [server README](../README.md)
- Website: https://saifs.ai
- Registry: https://registry.modelcontextprotocol.io/v0.1/servers?search=ai.saifs
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
