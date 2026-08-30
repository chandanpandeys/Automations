# AI-Powered Social Media Content Automation

An n8n workflow that turns a content brief into platform-specific social posts, routes drafts through a human approval step, and can publish approved content to configured social channels.

> **Project history:** built in October 2025. Documentation reviewed in August 2026. The committed JSON is an exported workflow snapshot, so node/provider compatibility depends on the n8n version and credentials available in your own instance.

**Workflow file:** `AI-Powered-Content-Creation-using-n8n-.json`

## What this workflow demonstrates

- Form-based content intake
- An AI-agent step that generates structured content for multiple platforms
- Structured JSON output validation/parsing
- Optional image generation and image-hosting steps
- Human approval before publishing
- Multi-channel publishing nodes
- Result aggregation and notification via email/Telegram

Targeted output formats include LinkedIn, Instagram, Facebook, X/Twitter, TikTok, Threads, and YouTube Shorts.

## High-level flow

1. A user submits a topic, optional keywords/hashtags, and a link.
2. The content agent produces platform-specific drafts and media suggestions.
3. Structured output is parsed into fields used by downstream nodes.
4. A review email is sent to an approver.
5. Only approved content proceeds to publishing nodes.
6. Publishing results are aggregated and sent through configured notification channels.

## Prerequisites

- n8n Cloud or a self-hosted n8n instance
- Credentials for the model/provider nodes you enable
- Credentials for the social platforms you actually want to publish to
- Optional Gmail, Telegram, search, and image-hosting credentials depending on enabled nodes

## Import

1. Open your n8n instance.
2. Create or open a workflow.
3. Import `AI-Powered-Content-Creation-using-n8n-.json`.
4. Reconnect every credential-backed node to credentials from **your** n8n instance.
5. Replace placeholder page/organization IDs and environment-variable references.
6. Disable provider/platform branches you do not intend to use.
7. Test with non-production accounts/content before enabling publishing.

## Configuration referenced by the workflow

Depending on which branches are enabled, the export references integrations such as:

- OpenAI / LangChain model nodes
- Google Gemini / PaLM-compatible model credentials
- Gmail OAuth2 for approval and result messages
- Facebook / Instagram Graph API
- LinkedIn OAuth2
- X / Twitter OAuth2
- Telegram Bot
- imgbb image upload
- Optional search/research provider nodes

Environment-variable style values referenced by the workflow include:

- `IMGBB_API_KEY`
- `EMAIL_ADDRESS_JOE`
- `TELEGRAM_CHAT_ID`

The workflow also contains placeholder IDs such as `[your-unique-id]` that must be replaced for your accounts.

## Credential-export note

n8n workflow exports can contain **credential reference IDs/names and instance metadata** even when they do not contain the underlying secret values. The committed snapshot includes those kinds of references, so importing it into another instance still requires reconnecting each node to local credentials.

No real API key should ever be committed to this repository. If you fork or re-export the workflow, review the JSON before publishing it and keep tokens, OAuth secrets, passwords, and private webhook URLs out of source control.

## Notable nodes / sections

- `Submit Social Post Details` — form trigger for the content brief
- `Social Media Content Factory` — central content-generation agent
- `Social Media Content` — structured-output parser/schema
- LLM nodes — provider backends such as OpenAI/Gemini
- Image generation/upload — optional media branch
- `Gmail User for Approval` — human approval gate
- Publishing nodes — X, Instagram, Facebook, LinkedIn and other configured channels
- Aggregation/results — status collection and Gmail/Telegram reporting

## Troubleshooting

**401 / OAuth errors**  
Reconnect the affected credential and verify the scopes required by that platform.

**Facebook / Instagram object-ID errors**  
Confirm the page/business account IDs and Graph API permissions.

**Image upload failures**  
Check the configured image-hosting credential, accepted file size/type, and whether the node is still enabled.

**Imported nodes show missing credentials**  
This is expected on a different n8n instance. Select/create your local credential for each node.

**Provider/model node errors**  
The workflow is a 2025 snapshot. If a model name or node version is no longer available, select a currently supported equivalent and retest the structured output.

## Security and reliability

- Keep the approval gate enabled while testing publishing branches.
- Use n8n credentials/environment variables rather than inline secrets.
- Give social-platform credentials only the permissions required for the workflow.
- Test against sandbox/non-production destinations where possible.
- Review user-provided content before forwarding sensitive information to third-party AI services.

## Status

**Working workflow snapshot / portfolio automation project.** The architecture remains useful, but external APIs, node versions, and model names can change. Treat the JSON as an importable starting point rather than a promise that every third-party integration will run unchanged forever.

## License

No explicit open-source license is currently included. Public visibility allows people to read the source, but it does not automatically grant reuse rights; add a license only after choosing the terms you want.
