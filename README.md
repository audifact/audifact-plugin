# Audifact agent plugin

**Your agent keeps a signed receipt of its decisions.**

This repo packages Audifact’s hosted MCP server (`https://api.audifact.io/mcp`) plus a skill that tells the agent to log consequential decisions, external actions, and subagent delegations — then hand you proof links (`https://audifact.io/e/{event_hash}`).

It is **receipts, not silent full tracing**. Target about **3–10 events per active day**.

## Install

### 1. Audifact API key (all clients)

1. Create an account at https://audifact.io/signup (the plugin never creates accounts).
2. Dashboard → API Keys → create a key (`af_...`, shown once).
3. Paste it only into the client’s **secret / masked** field. Never into chat or files.

### 2. Pick your client

**Official MCP Registry / VS Code / Copilot / other registry clients**

After we publish `io.github.audifact/audifact`, add the remote server from the [MCP Registry](https://registry.modelcontextprotocol.io) (or your client’s MCP gallery). Provide `X-API-Key` when prompted. Metadata: [`server.json`](./server.json).

**Claude Code** (self-hosted marketplace — no review queue)

```text
/plugin marketplace add audifact/audifact-plugin
/plugin install audifact@audifact
```

Paste the key into **Audifact API key** (`userConfig`, stored in the OS keychain).

**Grok Bot / Cursor** (x.ai catalog)

Install from the catalog when listed. Paste the key into **AUDIFACT_API_KEY**.  
(Repo renamed from `grokbot-plugin` → `audifact-plugin`; resubmit replaces the 2026-10-16 draft.)

**Agent Skills only** (`npx skills add`)

```bash
npx skills add audifact/audifact-plugin
```

This installs the skill instructions. You still need the **MCP server** connected in the same client, or the skill has no tools to call.

### 3. Confirm

Ask the agent to confirm the Audifact connection. It should log one `plugin_connected` decision and return a proof URL.

## ⚠️ Append-only ledger

Everything logged is signed and hash-chained and **cannot be edited or deleted**. The skill redacts before writing: no secrets, tokens, passwords, full message bodies or file contents — only short summaries.

## Billing

Every event consumes credits on **the Audifact account that owns the API key**.

## Privacy

- Public labels are **off by default**.
- Each agent logs under its own `agent_name` (e.g. `grokbot-cos`, `claude-code`).

## Tools

`log_decision`, `log_outcome`, `get_proof`, `register_agent`, `list_my_recent_decisions`.

## Repo layout

| Path | Role |
|------|------|
| `.mcp.json` | Grok/Cursor: `X-API-Key: ${AUDIFACT_API_KEY}` |
| `.grok-plugin/` | x.ai / Grok Bot plugin manifest |
| `.claude-plugin/` | Claude Code marketplace + `userConfig` key (overrides MCP auth for Claude) |
| `skills/audifact/` | Agent Skills `SKILL.md` |
| `server.json` | Official MCP Registry remote metadata |
| `assets/logo.svg` | Brand AF monogram |

## License

MIT
