# Prowlo MCP — Reddit & X for AI agents

[![smithery badge](https://smithery.ai/badge/egidijus-ambr/prowlo)](https://smithery.ai/servers/egidijus-ambr/prowlo)

> **Hosted MCP server** that gives Claude, Cursor, Cline, or any MCP client clean, semantically-searchable **Reddit** and **X** data — crawled through residential proxies, **read-only**, with **no Reddit API credentials**.

**Endpoint:** `https://api.prowlo.com/mcp`  ·  [Homepage](https://prowlo.com)  ·  [Setup docs](https://prowlo.com/docs/mcp-setup)  ·  Free 14-day trial, then $19/mo

---

## Why a hosted Reddit MCP server?

Most free Reddit MCP servers broke in 2026, when Reddit started returning `403` on the unauthenticated endpoints they relied on. Prowlo runs the whole pipeline for you — residential-proxy crawl, anti-bot, embeddings, and the MCP endpoint — so it keeps working. Connect once and your agent can **search, rank, and cite** Reddit (and X) over time, instead of taking a one-off peek that breaks next week.

- **Semantic search** over a persistent **Dataset** (vector embeddings) — find by meaning, not keyword regex
- **Clean typed JSON** — no HTML scraping, no Reddit API, no glue code
- **Read-only** — Prowlo never posts; your agent finds and ranks, you stay in the loop
- Delivery over **MCP + REST + webhooks**, plus Slack/email **Alerts**

## Connect in 30 seconds

### OAuth (recommended)
Add `https://api.prowlo.com/mcp` as a **custom connector** → **Connect** → sign in. No API key, no JSON, no restart.

### Config block (Claude Desktop / Cursor / Cline)
```json
{ "mcpServers": { "prowlo": { "url": "https://api.prowlo.com/mcp" } } }
```

### Claude Code
```bash
claude mcp add --transport http prowlo https://api.prowlo.com/mcp
```

### Clients without native HTTP MCP (stdio bridge)
```bash
npx mcp-remote https://api.prowlo.com/mcp
```

See full guides for [Claude Desktop, Cursor, Cline, and Windsurf](https://prowlo.com/integrations/claude-mcp).

## Tools

| Tool | What it does |
|---|---|
| `search_dataset` | Semantic search over your Dataset (Reddit + X), ranked by relevance |
| `reddit_search` | Live Reddit search across subreddits |
| `list_records` / `get_record` | List and fetch typed records (cursor-paginated) |
| `read_post` / `read_comments` | Full post body + comment threads |
| `list_subreddit_posts` | Recent posts from a subreddit |
| `list_watchers` / `watcher_create` | Manage Watchers — what Prowlo ingests into your Dataset |

Full reference: <https://prowlo.com/docs/api-reference>

## Example prompts

- "What is r/MachineLearning saying about vector databases this week?"
- "Find people on Reddit asking for a GummySearch alternative, ranked by buying intent."
- "Summarize the top complaints about \<competitor\> across r/SaaS this month, with sources."

## Sources

- ✅ **Reddit** — live
- 🔜 **X / Twitter** — rolling out
- 🔜 More social sources

## Links

- Homepage: <https://prowlo.com>
- MCP setup: <https://prowlo.com/docs/mcp-setup>
- Integration guides (Claude / Cursor / Cline / Windsurf): <https://prowlo.com/integrations/claude-mcp>
- Pricing: <https://prowlo.com/pricing>

---

Prowlo is a **hosted, commercial service**. This repository is documentation and connection examples — there's nothing to install or run; the MCP server lives at `https://api.prowlo.com/mcp`.
