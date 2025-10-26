# AI-Powered Social Media Content Automation

This repository contains an n8n workflow that generates platform-optimized social media content using AI agents and optionally publishes it to X, Facebook, Instagram, LinkedIn and other platforms after approval.

Source workflow file: `AI-Powered-Content-Creation-using-n8n-.json` (import into n8n to use).

## What this workflow does

- Accepts a simple form input (Topic, optional keywords/hashtags and link).
- Uses an LLM agent to generate structured, platform-specific posts (LinkedIn, Instagram, Facebook, X/Twitter, TikTok, Threads, YouTube Shorts).
- Optionally generates or accepts an image for posts (OpenAI/third-party image APIs or user upload).
- Sends a review email to a configured approver (Gmail) and waits for approval before publishing.
- Publishes to social networks via the appropriate nodes (Facebook Graph / Instagram, X, LinkedIn). Results are collected and summarized (email/Telegram).

## Key features

- Multi-platform content outputs tailored to each network's style and limits
- Structured output schema (JSON) so you can adapt downstream processing or integrations
- Optional image generation and upload (imgbb example included)
- Approval gate (email-based) before publishing
- Result aggregation and status reporting (email + Telegram)

## Prerequisites

- n8n (self-hosted or cloud) with access to the editor and runtime
- Accounts / API keys for the services you want to use (see Credentials below)
- Node versions: workflow uses standard n8n nodes and the LangChain/OpenAI/agent nodes

## Credentials & Environment variables

You must configure the following credentials in n8n (names from the workflow are shown in the JSON):

- OpenAI account (used by several OpenAI/langchain nodes)
- Google (Gemini / PaLM) credentials (optional)
- SERP API (optional, used by the workflow for research)
- Gmail OAuth2 (for approval and result emails)
- Facebook Graph API (for Facebook & Instagram endpoints)
- LinkedIn OAuth2
- X / Twitter OAuth2
- Telegram Bot token (optional, for status notifications)
- imgbb API key (optional, if using imgbb for image uploads)

Environment variables referenced in the workflow (set these in your n8n environment or in the credentials where appropriate):

- `IMGBB_API_KEY` — imgbb API key used by the imgbb upload node
- `EMAIL_ADDRESS_JOE` — approver email address used by send-and-wait Gmail nodes
- `TELEGRAM_CHAT_ID` — chat id used by Telegram results node

Note: the workflow also contains placeholder values such as `[your-unique-id]` which must be replaced with your platform-specific page/organization IDs.

## Importing the workflow into n8n

1. Open your n8n instance.
2. Go to Workflows → Import and select `AI-Powered-Content-Creation-using-n8n-.json`.
3. Review nodes and connect the appropriate credential objects (click each node that requires credentials and select the matching credential in your n8n instance).

## Configuration checklist (before running)

- Configure all credentials listed above in the n8n Credentials area.
- Replace placeholder node parameters such as `[your-unique-id]` with real page or organization IDs.
- Set environment variables in your n8n runtime (or replace env references in nodes with explicit values if you prefer).
- Optionally enable/disable LLM nodes depending on which provider you use (OpenAI, Google Gemini, etc.).

## How the workflow is organized (high level)

- `Submit Social Post Details` (formTrigger): user-facing form to provide Topic, Keywords, Link
- `Social Media Content Factory` (agent): central LangChain agent that generates a structured JSON output using a schema for all target platforms
- LLM nodes (OpenAI / gpt-4o-mini / Gemini): language model backends used by the agent
- `Social Media Content` (output parser): validates & parses agent output to structured JSON
- Image generation / upload: OpenAI image node, optional external image generation (pollinations.ai) and imgbb upload
- `Gmail User for Approval`: sends approval email and waits for double-confirm approval before publishing
- Publish nodes: `X Post`, `Instragram Post`, `Facebook Post`, `LinkedIn Post` — these nodes attempt publishing and return status
- Aggregation & results: nodes to gather and send results via Gmail and Telegram

## Example usage flow

1. A user submits the form with a Topic (e.g., "Why n8n is the Best Workflow Automation Tool").
2. The agent generates platform-specific post drafts and image suggestions.
3. The workflow sends an approval email to the configured approver. The approver can approve/reject via the email flow.
4. If approved, the workflow posts to the configured platforms and collects responses.
5. A summary email and Telegram notification are sent with the publish results.

## Security & privacy notes

- Do not commit real API keys or OAuth tokens to source control. Store secrets in n8n credentials or environment variables.
- Review privacy requirements for any user-provided content and do not forward sensitive data to third-party services without consent.

## Troubleshooting

- Common failures:
	- 401 / access token errors: ensure OAuth credentials are connected and valid, refresh tokens if necessary.
	- `Object with ID` errors from Facebook/Instagram: confirm the page/IG business account IDs and Graph API permissions.
	- Image upload errors: check `IMGBB_API_KEY` and file sizes allowed by the service.

- Debugging tips:
	- Run the workflow in n8n editor with small test inputs and inspect node outputs.
	- Temporarily enable `continueOnFail` on nodes to collect more results for troubleshooting.

## Extending the workflow

- Add new platform nodes (e.g., YouTube API, additional Telegram sinks) and map the structured JSON output to the node inputs.
- Replace or augment the LLMs with your preferred provider by swapping or enabling/disabling the model nodes.

## Attribution & License

This repo contains an n8n workflow and supporting notes. Treat the JSON workflow itself as the single source of truth for node configurations. No explicit license file is included — add a LICENSE if you want to open source it under a specific license.

---

