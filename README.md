# Audifact for Grok Bot

**Your bot keeps a signed receipt of its decisions.**

This plugin connects Grok Bot to Audifact's hosted MCP server. A bundled skill tells the bot to log its consequential decisions, external actions (send, post, buy, delete) and subagent delegations, attach outcomes, and hand you proof links (`https://audifact.io/e/{event_hash}`). The bot logs only what the skill tells it to log. It is **receipts, not silent full tracing**.

## Setup
1. Create an Audifact account yourself at https://audifact.io/signup. The plugin never creates accounts.
2. Dashboard → API Keys → create a key (`af_...`, shown once).
3. Install the plugin and paste the key into the **AUDIFACT_API_KEY** secret field. Never paste it into chat or files. To rotate the key, create a new one in Audifact, replace the plugin secret, then revoke the old key.
4. Ask your bot to confirm the Audifact connection. It logs one `plugin_connected` decision and returns the proof link.

## ⚠️ Append-only ledger
Everything logged is signed and hash-chained, and **cannot be edited or deleted**. The skill redacts before writing: no secrets, tokens, passwords, full message bodies or file contents, only short summaries. Don't ask your bot to log anything you wouldn't want kept permanently.

## Billing
Every event consumes credits on **the Audifact account that owns the API key**. The skill targets about 3-10 events per active day per bot.

## Privacy
- Public labels are **off by default**. Proof pages show only the labels you choose to make public.
- Each bot logs under its own `agent_name` (e.g. `grokbot-cos`).

## Network endpoints and credentials
- `https://api.audifact.io/mcp`: streamable-HTTP MCP. Auth header `X-API-Key: ${AUDIFACT_API_KEY}`.
- Proof links: `https://audifact.io/e/{event_hash}`.
- There is no other network access, telemetry, hooks or local code.

## Tools
`log_decision`, `log_outcome`, `get_proof`, `register_agent`, `list_my_recent_decisions`.
