# x.ai plugin resubmit — audifact-plugin

Form: https://x.ai/bot/marketplace/submit-a-plugin

## Why resubmit
Replaces the 2026-10-16 ~9:48 ET submission that pointed at `github.com/audifact/grokbot-plugin`.
Repo renamed to **audifact-plugin** so Claude/Cursor/Registry users are not put off by a Grok-only name.
Same product, same MCP endpoint, same skill; packaging repo name only.

## Form fields

| Field | Value |
|--------|--------|
| Organization name | Audifact |
| Organization handle | audifact |
| Website URL | https://audifact.io |
| Logotype URL | https://raw.githubusercontent.com/audifact/audifact-plugin/main/assets/logo.svg |
| List where | Grok Bot (and Cursor if offered) |
| MCP server URL OR public GitHub repository URL | https://github.com/audifact/audifact-plugin |
| Brief description | Your bot keeps a signed receipt of its decisions. Connects to Audifact's hosted MCP at https://api.audifact.io/mcp with an API key. Logs consequential decisions, external actions, and subagent delegations to an append-only hash-chained ledger. Events bill to the key's Audifact account. |

## Testing instructions (paste)

Create an Audifact API key (af_...) at https://audifact.io Dashboard → API Keys.
Install the plugin; paste the key into AUDIFACT_API_KEY (masked secret). Never paste into chat.
Ask the bot to confirm the Audifact connection. It should log decision_type=plugin_connected as agent_name for that bot and return a proof URL https://audifact.io/e/{event_hash}.
Open the proof URL; hash integrity and agent chain should validate.
Dogfood proof (CoS, agent_name=grokbot-cos): https://audifact.io/e/e17686cb5316a32437ff278542165e802fd78855fe0dfd4bfee1152e77f561fb

**Note for reviewers:** This submission replaces the earlier 2026-10-16 listing for repository `audifact/grokbot-plugin` (renamed to `audifact/audifact-plugin`). Please treat this as the canonical Audifact plugin entry. GitHub redirects the old repo URL to the new one.

## After submit
- Confirm page / email to creo@audifact.io
- If xAI reports a duplicate, this resubmit is canonical
