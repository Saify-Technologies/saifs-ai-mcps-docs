# Synthetic Data Generator - Saifs AI

Synthetic Data Generator - Saifs AI is an MCP (Model Context Protocol) server that lets Microsoft Copilot Studio agents and other AI experiences generate realistic, fake user profiles for demos, UI fixtures, QA, and development. Profiles can include name, gender, location, email, login details, date of birth, phone, picture, and nationality. All data is synthetic—not real personally identifiable information (PII)—so teams can build and test without exposing customer data.

## Publisher: Saifs AI

Website: [https://saifs.ai](https://saifs.ai)  
Sign up / Get started: [https://saifs.ai](https://saifs.ai)  
Contact us / Help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)  
Privacy: [https://saifs.ai/privacy_policy/synthetic-data-generator](https://saifs.ai/privacy_policy/synthetic-data-generator)  
Terms: [https://saifs.ai/eula](https://saifs.ai/eula)

**Authentication:** OAuth 2.0

## Who can use this MCP

This MCP is intended for makers, developers, and teams who need safe sample people data inside Microsoft 365 Copilot and Copilot Studio agents:

| Audience | How it helps |
| --- | --- |
| **Copilot Studio makers** | Add synthetic people tools to agents so demos and prototypes can show realistic contacts, personas, and user lists without hardcoding fake data. |
| **App and UI developers** | Quickly generate sample contacts and demo personas for screens, forms, and fixtures while building or reviewing experiences. |
| **QA and test engineers** | Create consistent fake profiles for functional, regression, and UX testing without using production or real customer data. |
| **Product designers and demo teams** | Populate walkthroughs, sales demos, and training scenarios with believable user profiles on demand. |
| **Privacy-conscious organizations** | Reduce risk by using clearly synthetic data instead of anonymizing or copying real PII for non-production work. |

Administrators who enable certified MCP servers for their tenant can make this capability available to approved makers under their organization's governance policies.

## Prerequisites

You will need the following to proceed:

* Access to Microsoft Copilot Studio (or another supported Microsoft agent experience that can use certified MCP servers)
* Permission from your Microsoft 365 / tenant administrator to enable and use this MCP server
* Valid authentication credentials for the Synthetic Data Generator - Saifs AI service (see [Obtaining Credentials](#obtaining-credentials))
* Network access from the agent runtime to the MCP server endpoint

## Supported Operations

The MCP server currently exposes the following tool:

### Generate Random Users (`random_users`)

Generates one or more synthetic (fake) user profiles via the MwVideos random users API. Use this tool when the user asks for random users, fake people, sample contacts, demo personas, UI fixtures, or test profiles.

**Returned profile fields (typical):** name, gender, location, email, login, date of birth (dob), phone, picture, and nationality (`nat`).

**Parameters:**

| Parameter | Type | Required | Description |
| --- | --- | --- | --- |
| `results` | integer | No | Number of random user profiles to generate. Must be an integer greater than or equal to `1`. Default: `1`. Examples: `1`, `5`, `10`. |
| `isPro` | integer | No | Pro flag passed through to the upstream API. Allowed values: `0` or `1`. Default: `0`. |

**Behavior notes:**

* Authenticated `user_id` is injected server-side. Do not invent profiles in the agent; always call this tool.
* Generated people are fake and intended for demos and testing only—not real PII.
* This operation is read-only with respect to customer data stores; it generates synthetic data and does not modify or delete existing business records.

## Obtaining Credentials

Synthetic Data Generator - Saifs AI uses **OAuth 2.0** authentication. Users sign in through the OAuth 2.0 consent flow configured for the MCP in Copilot Studio / Microsoft 365.

For Microsoft certification and validation, OAuth 2.0 configuration is supplied through **Azure Key Vault** as described in [Microsoft MCP server certification](https://learn.microsoft.com/en-us/microsoft-copilot-studio/mcp-certification). Secret names are case-sensitive and must match exactly.

1. Create an Azure Key Vault in your Azure tenant.
2. Store the following OAuth 2.0 secrets in the Key Vault:

   **Required:**
   * `ClientId`
   * `ClientSecret`
   * `AuthorizationUrl`
   * `TokenUrl`

   **Optional (as applicable to your OAuth 2.0 setup):**
   * `RefreshUrl` (if using refresh tokens)
   * `Scopes` (if your identity provider requires scopes)
3. Grant the Microsoft certification service principal `8e91e74f-afe9-41cd-8c3f-17a9562a74ea` **Key Vault Secrets User** (or equivalent read access).
4. Reference the Key Vault URI (not a secret URI) in the MCP package authorization configuration:

   ```json
   "authorization": {
     "type": "AzureKeyVault",
     "referenceId": "https://saifs-mcp-kv.vault.azure.net/"
   }
   ```

Contact Saifs AI support at [https://saifs.ai/contact-us](https://saifs.ai/contact-us) if you need production OAuth 2.0 client credentials or onboarding assistance for your organization.

## Getting Started

1. Ask your Microsoft 365 or Copilot Studio administrator to enable **Synthetic Data Generator - Saifs AI** for your environment (after certification / publication, via the supported discovery surfaces such as Copilot Studio and Azure Foundry as applicable).
2. Add the MCP server (or its tools) to your Copilot Studio agent.
3. Complete any required sign-in or consent prompts.
4. In the agent, ask for synthetic users—for example:
   * "Generate 5 fake user profiles for a demo contact list."
   * "Create a sample persona with name, email, and phone for UI testing."
5. Confirm that returned profiles are treated as synthetic test data only and are not used as real customer records.

## Known Issues and Limitations

* **Active account required:** An active Saifs AI account with access to Synthetic Data Generator is required. Tools cannot be called until OAuth 2.0 sign-in completes successfully on the connection.
* **Synthetic data only:** Profiles are fake. Do not use them as real identities, for production customer communications, identity verification, or compliance workflows that require real PII.
* **Single tool surface (current release):** This version focuses on random user profile generation (`random_users`). Additional synthetic data types may be added in later package updates and will require recertification / resubmission when tools or certified metadata change.
* **Upstream API dependency:** Generation depends on the MwVideos random users API (`POST /mwvideos/api/random_users`). Temporary upstream outages or rate limits may cause tool failures or delayed responses.
* **Parameter bounds:** `results` must be an integer `>= 1`. Very large `results` values may be limited by upstream API quotas or response size; if requests fail, retry with a smaller count.
* **`isPro` semantics:** The `isPro` flag (`0` or `1`) is passed through to the upstream API. Behavior differences between `0` and `1` depend on the Saifs AI / MwVideos service configuration for your account.
* **ASCII package constraints:** Per Microsoft MCP certification guidance, manifest and tool definition header names and values should use ASCII characters to avoid validation failures.
* **Not a substitute for production data platforms:** This MCP is for demos, fixtures, and testing—not for generating production master data or migrating real customer datasets.

## Frequently Asked Questions

### Is the generated data real personal information?

No. The tool produces fake / synthetic people for demos and testing. It should not be treated as real PII.

### Can I generate more than one profile at a time?

Yes. Set the `results` parameter to the number of profiles you need (integer `>= 1`). For example, `results: 10` requests ten profiles.

### Do I need to invent profile fields in my agent prompt?

No. Authenticated identity is handled server-side. Always call the `random_users` tool rather than fabricating profile JSON in the agent.

### Who should enable this MCP in our tenant?

A Microsoft 365 / Copilot Studio administrator (or equivalent) should enable certified MCP servers according to your organization's governance policies. Makers then attach the tools to their agents.

### Where will certified MCP servers appear?

After Microsoft certification and publication, certified MCP servers are expected to be discoverable in Copilot Studio and Azure Foundry, with Microsoft 365 admin governance support where applicable. See [Microsoft MCP server certification](https://learn.microsoft.com/en-us/microsoft-copilot-studio/mcp-certification).

## Deployment Instructions

For Microsoft Copilot Studio MCP certification packaging:

1. Include this `intro.md` in the MCP submission package together with the manifest, tool definition file (`synthetic_mint_mcptools.json`), icons, and required metadata (support, privacy, and terms links).
2. Configure Azure Key Vault–based authorization in the manifest as required by the [Apps and Agents for M365 and Copilot](https://learn.microsoft.com/en-us/microsoft-copilot-studio/mcp-certification) offer type in Partner Center.
3. Submit the package through Partner Center using the **Apps and Agents for M365 and Copilot** offer type.
4. After approval, keep the live MCP implementation aligned with the certified package. Resubmit when adding tools, changing certified metadata, or making significant behavior changes.

For icon and branding assets, follow Microsoft 365 / Teams package guidance (sizing, safe area, contrast, and branding) as referenced in the MCP certification documentation.

## Support

- Product: Synthetic Data Generator - Saifs AI
- Publisher: Saifs AI
- Website: [https://saifs.ai](https://saifs.ai)
- Sign up / Get started: [https://saifs.ai](https://saifs.ai)
- Contact us / Help: [https://saifs.ai/contact-us](https://saifs.ai/contact-us)
- Privacy: [https://saifs.ai/privacy_policy/synthetic-data-generator](https://saifs.ai/privacy_policy/synthetic-data-generator)
- Terms: [https://saifs.ai/eula](https://saifs.ai/eula)
- MCP endpoint (reference): `https://synthetic-mint-mcp.saifs.ai/mcp`
