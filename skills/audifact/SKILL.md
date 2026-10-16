---
name: audifact
description: Log signed receipts of consequential decisions, external actions and subagent delegations to Audifact (log_decision, log_outcome, get_proof). Use sparingly - 3-10 events per active day.
---
# Audifact receipts

You have the Audifact MCP tools. They write **signed, append-only receipts** that cannot be edited or deleted. Use them for the few things that matter, not as a diary.

Tools, in order of preference: `log_decision`, `log_outcome`, `get_proof`. `list_my_recent_decisions` is for when the user asks what you logged. `register_agent` is only needed if a call reports your agent is unknown.

## Always
- Pass your own `agent_name` on every call (e.g. `grokbot-cos`). Never log on behalf of another bot.
- Pass `decision_value` as a **string** (e.g. `"connected"`, `"1"`). Numbers are coerced server-side, but strings avoid hash mismatches.
- Proof link for any event: `https://audifact.io/e/{event_hash}`.
- Leave public labels unset unless the user has turned them on.

## Setup (first run only)
Make one `log_decision` call with `decision_type: plugin_connected`, `decision_value: audifact`, `decision_label: "Audifact plugin connected"`. Then give the user the proof URL from the returned `event_hash`. Never ask for the API key in chat. It lives only in the plugin's secret field.

## When to log (target 3-10 events per active day)
1. **Consequential decisions:** choosing between options for the user, changing a plan, spending, scheduling. Use `log_decision` with a domain `decision_type` (e.g. `plan_change`, `topic_pick`).
2. **External actions:** sending, posting, buying, deleting, sharing, submitting. Use `log_decision` with `decision_type` = `action:send`, `action:post`, `action:purchase`, `action:delete`, `action:share` or `action:submit`. Put `approved_by_user` (true/false) and `target` (e.g. "email to 1 recipient") in the context or metadata. Log it when you act, not when you draft.
3. **Delegations:** when you hand work to a subagent, log `decision_type: delegation` with a one-line task and a `run_id`. When it returns, call `log_outcome` with `decision_event_hash` set to that delegation's `event_hash`.

## Outcomes
- Use `log_outcome` once the result is known, always with `decision_event_hash`.
- `outcome_value` is **numeric**: `1` = succeeded / as intended, `0` = failed / declined / reverted. Use another number (a count, a dollar amount) only when it means something. Always add a short `outcome_label` ("sent", "declined by user", "subagent timed out").

## Do not log
Chit-chat, reads, searches, drafts, retries, internal reasoning, or routine status checks.

## Redaction (before every call)
- Never include API keys, tokens, passwords, cookies, 2FA codes, private keys, or account numbers.
- No full message bodies, transcripts, or file contents. Summarize in at most 1-2 lines.
- Prefer roles over personal data ("1 external recipient", not an email address) unless the user asked for named receipts.
- The ledger cannot be edited. If you are unsure whether something is sensitive, leave it out.

## Never block acting
Logging is a side effect. If Audifact errors or is slow, continue the task and mention the failure once at the end. A receipt is never user approval, and you never delay an approval request to log first.
