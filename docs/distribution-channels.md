# Audifact distribution channels (researched 2026-10-16)
Assets: public repo audifact/grokbot-plugin; hosted MCP https://api.audifact.io/mcp (streamable HTTP, X-API-Key); SKILL.md (Agent Skills format). Already submitted: x.ai Grok Bot + Cursor (10/16).
Confidence: "V" = verified against live docs today; "K" = from prior knowledge, re-check page before submitting.

## Do first
1. **Official MCP Registry** (V) — publish once; feeds VS Code/Copilot MCP gallery, PulseMCP, Glama and other aggregators that ingest it. Remote + secret header supported natively (`remotes[].headers[] isSecret`). Today: 0 results for "audifact".
   - Add `server.json` (draft: drafts/mcp-registry/server.json) to repo root.
   - `brew install mcp-publisher` (or release binary from github.com/modelcontextprotocol/registry).
   - `mcp-publisher login github` as an audifact org member (org membership must be public) -> namespace `io.github.audifact/*`. Alt: `io.audifact/*` via DNS TXT (`mcp-publisher login dns`) — nicer brand, needs DNS record.
   - `mcp-publisher publish`; check `https://registry.modelcontextprotocol.io/v0/servers?search=audifact`.
2. **Claude Code: make the repo its own marketplace** (V) — free, no review. Add drafts/claude-plugin/* . Users: `/plugin marketplace add audifact/grokbot-plugin` then `/plugin install audifact@audifact`. Claude uses `userConfig` with `sensitive: true` (keychain) and `${user_config.audifact_api_key}` in .mcp.json headers. Run `claude plugin validate . --strict`.
   - Caveat: root .mcp.json is shared with Grok's `${AUDIFACT_API_KEY}`; keep Claude's MCP config in a separate file referenced from .claude-plugin/plugin.json.
3. **Anthropic directory (claude.ai/directory)** (V) — one listing reaches claude.ai, Cowork and Claude Code. Submit at https://claude.ai/directory/manage (needs paid claude.ai plan); checklist https://claude.com/docs/plugins/pre-submission-checklist. Do after #2 validates. Remote connectors there generally expect OAuth — an API-key-only plugin may be fine, connector listing may not. Review time unknown.
4. **Agent Skills / skills.sh** (K) — skills.sh indexes repos installed via `npx skills add audifact/grokbot-plugin`; no submission form, appears from installs. Also works for Codex, Cursor, Copilot, Gemini CLI etc. that read SKILL.md. Effort S: just make sure SKILL.md frontmatter (name, description) is valid; add install line to README.
5. **Smithery** (K) — smithery.ai/new: sign in with GitHub, "external/remote" server by URL; supports config schema for API key. Effort S. Big MCP audience.
6. **Glama + mcp.so + PulseMCP + MCP Market + mcpservers.org** (K) — directory submit forms (Glama also auto-indexes GitHub repos with MCP, and awesome-mcp-servers PR requires a Glama listing/score badge). ~10 min each. Effort S.
7. **punkpeye/awesome-mcp-servers PR** (K) — entry draft in drafts/awesome-mcp-entry.md; needs Glama listing first. Also appropriate: awesome Claude Code plugin lists, VoltAgent/awesome-agent-skills.

## Full table
| Channel | Accepts | Method | Requirements / notes | Cost | Fit | Effort | Needs new from Audifact |
|---|---|---|---|---|---|---|---|
| MCP Registry registry.modelcontextprotocol.io | remote URL | CLI mcp-publisher | server.json, namespace verify (GitHub org or DNS) | free | high (feeds others) | S | server.json (drafted) |
| VS Code / GitHub Copilot MCP gallery (github.com/mcp) | remote | pulls from MCP Registry | listing via registry | free | high | S (via #1) | none beyond #1 |
| Claude Code self-hosted marketplace | plugin repo | marketplace.json in repo | userConfig sensitive key | free | high | S | .claude-plugin/ (drafted) |
| Anthropic directory claude.ai/directory | plugin / connector | portal claude.ai/directory/manage | paid plan; review; connectors favor OAuth | plan | high | M | likely OAuth for connector |
| anthropics/skills | skills | repo is Anthropic-curated, not open submissions | — | — | low | — | skip |
| claude-plugins-official | plugins | partner contact only | — | — | — | — | skip |
| x.ai Grok Bot + Cursor | plugin repo | form (done 10/16) | awaiting review | free | high | done | — |
| cursor.directory | MCP/rules | community submit form on site | K | free | med | S | none |
| skills.sh / agentskills.io / skillsmp.com | SKILL.md | auto-index from GitHub/installs | valid frontmatter | free | med | S | maybe skill-only repo for clarity |
| OpenAI Codex skills | SKILL.md | users install from repo; openai/skills curated | K | free | med | S | none |
| OpenAI Apps / ChatGPT connectors | remote MCP | Apps SDK submission | OAuth required for auth'd apps; review | free | med | L | OAuth |
| Smithery | remote URL | web, GitHub login | config schema for key | free | high | S | none |
| Glama | repo/remote | auto-index + claim | — | free | med | S | none |
| mcp.so, PulseMCP, MCP Market, mcpservers.org | repo/URL | submit forms (PulseMCP also ingests registry) | — | free | med | S | none |
| awesome-mcp-servers (punkpeye, wong2) | repo | GitHub PR | Glama badge for punkpeye | free | med | S | entry drafted |
| Windsurf (Cascade MCP store) | remote | curated; K: via partner/registry | — | free | med | M | none |
| Cline MCP Marketplace | repo | GitHub issue on cline/mcp-marketplace | 400x400 PNG logo, README install steps | free | med | S | PNG logo |
| Goose extensions | remote/cmd | PR to block/goose extensions site | K | free | low-med | S | none |
| Zed | extensions (mostly stdio context servers) | PR zed-industries/extensions | needs Rust/wasm wrapper | free | low | M-L | wrapper |
| Gemini CLI extensions | gemini-extension.json repo | gallery auto-indexes GitHub repos tagged | add gemini-extension.json (mcpServers httpUrl + headers) | free | med | S | gemini-extension.json |
| Docker MCP Catalog | images, now remote too | PR docker/mcp-registry | K | free | med | M | maybe |
| LangChain/LangGraph | via langchain-mcp-adapters | docs PR / blog | works today with remote URL | free | med-high | S (guide) | docs snippet |
| CrewAI tools | MCP via MCPServerAdapter | docs/community | — | free | med | S | docs snippet |
| LlamaHub | Python pkg | PR llama_index | needs package | free | low | M | pypi pkg |
| Composio | toolkit | partner request | K | free | med | M | — |
| Zapier MCP / n8n community nodes | n8n: npm node pkg | npm publish + verify | n8n-nodes-audifact package | free | med | M | npm package |
| Hugging Face (Spaces/MCP) | Space | create Space | demo only | free | low | M | — |
| AWS Marketplace AI agents & tools / AgentCore | SaaS/MCP | seller registration, listing review | business, pricing, tax | fees | high (enterprise compliance) | L | seller account, pricing |
| Microsoft Copilot Studio / Power Platform connectors | custom connector, MCP | certified connector PR (microsoft/PowerPlatformConnectors) | OpenAPI + review | free | high (enterprise) | L | connector def |
| Product Hunt, TAAFT, Futurepedia, etc. | product | form/launch | — | free/paid | low-med | S | assets |

## Skip / later
- OpenAI Apps, Anthropic connector (vs plugin), Composio: want OAuth; revisit after Audifact OAuth.
- AWS Marketplace / Copilot Studio: best-fit buyers (compliance) but L effort + business setup; after a few real users.
- n8n / LlamaHub / Zed: need new packages or wrappers; low return now.
- anthropics/skills, claude-plugins-official: not open to submissions.
- Product Hunt / AI directories: save for a launch moment with demo + receipts page.
- Caution: don't list widely until the in-app header injection (A1) is confirmed and the append-only/billing note is in every listing.
