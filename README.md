# Prowlo — Reddit, Hacker News & more for your AI agent

[![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![MCP Registry](https://img.shields.io/badge/MCP_Registry-com.prowlo%2Freddit--x-ff7400)](https://registry.modelcontextprotocol.io/v0/servers?search=com.prowlo/reddit-x)

**Social research for AI agents — research that runs 24/7.** Prowlo is a hosted MCP server. Watchers collect new posts from Reddit and Hacker News (plus Mastodon, RSS and X), and your agent searches them by meaning from Claude, Cursor or any MCP client.

```text
https://api.prowlo.com/mcp
```

[Website](https://prowlo.com) · [Setup guide](https://prowlo.com/docs/mcp-setup) · [Pricing](https://prowlo.com/pricing) · Free 14-day trial, then $19/month

- **Memory, not a one-off peek.** Watchers keep collecting while your laptop is closed, so your agent can look back at last week or last month.
- **Search by meaning.** Ask for "people frustrated with their CRM" and get those posts, not just exact keyword hits.
- **Live reads when you need them.** Pull a Reddit or Hacker News post or comment thread right now.
- **Prowlo is read-only.** It never posts, comments or votes.
- **No setup.** Sign in with OAuth. No Reddit API keys, no config files for most clients.

## Contents

- [Connect in 30 seconds](#connect-in-30-seconds)
- [What you can ask](#what-you-can-ask)
- [Tools](#tools)
- [Sources](#sources)
- [Limits & pricing](#limits--pricing)
- [Privacy & security](#privacy--security)
- [Support](#support)
- [About this repository](#about-this-repository)

## Connect in 30 seconds

Every client connects to the same hosted endpoint. OAuth is the recommended path: no API key to copy, store or rotate.

### Claude (web, Desktop, mobile)

1. Open **Customize → Connectors → Add custom connector**.
2. Paste `https://api.prowlo.com/mcp` and click **Add**.
3. Click **Connect** and sign in to Prowlo.

> On Claude's Free plan you can add one custom connector. On Team and Enterprise, an org Owner adds it. If that blocks you, Claude Code, Cursor and VS Code connect per user over the same endpoint.

### Claude Code

```bash
claude mcp add --transport http prowlo https://api.prowlo.com/mcp
```

Then run `/mcp` inside Claude Code, pick **prowlo** and choose **Authenticate**. Add `-s user` to the first command to make Prowlo available in every project.

Claude Code plugin with skills: [github.com/prowlo-com/prowlo-plugin](https://github.com/prowlo-com/prowlo-plugin)

### Cursor

Add to `~/.cursor/mcp.json` (or **Settings → MCP → Add new server**). Cursor opens a browser window to sign in.

```json
{
  "mcpServers": {
    "prowlo": { "url": "https://api.prowlo.com/mcp" }
  }
}
```

### VS Code

```bash
code --add-mcp '{"name":"prowlo","type":"http","url":"https://api.prowlo.com/mcp"}'
```

### Windsurf and Cline

**Windsurf** (`~/.codeium/windsurf/mcp_config.json`) signs in with OAuth when there is no headers block:

```json
{
  "mcpServers": {
    "prowlo": { "serverUrl": "https://api.prowlo.com/mcp" }
  }
}
```

**Cline** (`cline_mcp_settings.json`) and other clients that need a key: create an API key in your Prowlo settings and send it as a header. Cline needs `"type": "streamableHttp"`; without it, Cline falls back to the legacy SSE transport.

```json
{
  "mcpServers": {
    "prowlo": {
      "type": "streamableHttp",
      "url": "https://api.prowlo.com/mcp",
      "headers": { "Authorization": "Bearer YOUR_PROWLO_API_KEY" }
    }
  }
}
```

### Any other client (stdio bridge)

For clients that only speak stdio, bridge to the hosted server with [`mcp-remote`](https://www.npmjs.com/package/mcp-remote) (Node.js 18+). It runs a small local shim; the server is still the hosted one.

```bash
npx mcp-remote https://api.prowlo.com/mcp
```

Ready-to-copy config files for each client are in [`examples/`](examples/).

## What you can ask

Once connected, just ask your agent:

- "Watch r/SaaS and r/startups, and add a keyword monitor for 'GummySearch alternative' across all of Reddit."
- "Search my Prowlo records from the last 30 days for discussions about tracking Reddit mentions. Summarize the main points with links."
- "Which subreddits are best for indie SaaS founders, and what are their posting rules?"
- "What are people on Hacker News saying about running LLMs locally this week? Read the top thread and summarize the main arguments."
- "Create an alert that sends new posts mentioning our brand to Slack."
- "Set up a daily trend for mentions of our competitor and show me how it changed this month."
- "Pull the most common complaints about our competitor from my records and quote the exact wording people used."
- "Tag every post that mentions pricing as `pricing`, then list the tagged posts from this week."

## Tools

Prowlo exposes 43 tools. **Read** tools never change anything. **Write** tools change only your own Prowlo setup (Watchers, Alerts, Trends, tags, profile), never anything on Reddit or X. Every tool carries MCP `readOnlyHint` / `destructiveHint` annotations, so clients can ask before running a write.

### Dataset

Your own indexed posts, collected by your Watchers. Free, rate-limited only.

| Tool | What it does | Access |
|---|---|---|
| `search_dataset` | Search your collected posts by meaning or keyword, with filters for community, platform, date, score, tags and more | Read |
| `list_records` | List records in your Dataset with the same filters, page by page | Read |
| `get_record` | Fetch one record with its full body and tags | Read |
| `get_record_comments` | Fetch the current comment thread for a record (live, own daily cap) | Read |
| `get_dataset_summary` | Record count, last update, busiest channels and today's live-read usage | Read |

### Live reads

Real-time Reddit and Hacker News reads. Metered: 1 credit each, daily cap.

| Tool | What it does | Access |
|---|---|---|
| `social_search` | Search Reddit or Hacker News right now | Read |
| `social_list_posts` | List a subreddit's posts or Hacker News stories (hot, new, top, ...) | Read |
| `social_get_post` | Fetch one post or story with its comment thread | Read |
| `social_get_user` | Public profile of an account in a thread you're reading (Reddit or Hacker News) | Read |
| `social_get_user_activity` | Recent public posts or comments of an account in a thread you're reading | Read |
| `social_find_channels` | Find subreddits, or get one subreddit's details and rules | Read |

### Catalog

Prowlo's directory of ~1,200 tracked subreddits. Free, not metered.

| Tool | What it does | Access |
|---|---|---|
| `catalog_search_subreddits` | Find subreddits by topic | Read |
| `catalog_get_subreddit` | Size, activity, growth, self-promotion tolerance and similar communities for one subreddit | Read |
| `catalog_top_subreddits` | Rank subreddits: largest, most active or fastest growing | Read |

### Watchers & Keyword monitors

What Prowlo collects into your Dataset.

| Tool | What it does | Access |
|---|---|---|
| `list_watchers` | List your Watchers, their sources, tags and status | Read |
| `watcher_create` | Start watching a subreddit, Hacker News, a Mastodon or RSS feed, an X account, or a Reddit-wide search query | **Write** |
| `watcher_update` | Pause or resume a Watcher, or replace its tags | **Write** |
| `watcher_delete` | Delete a Watcher and its records | **Write** |
| `watcher_remove_source` | Remove a source from a Watcher | **Write** |
| `keyword_monitor_list` | List your Keyword Monitors and keyword quota | Read |
| `keyword_monitor_create` | Search all of Reddit daily for a query and collect the matches | **Write** |
| `get_crawl_activity` | Recent collection runs: new posts, matches, errors | Read |

### Alerts & Trends

Saved queries that notify you or chart a topic over time.

| Tool | What it does | Access |
|---|---|---|
| `alert_list` | List your Alerts and their delivery stats | Read |
| `alert_create` | Get a Slack or webhook notification when new posts match | **Write** |
| `alert_update` | Change an Alert's query, destination, threshold or status | **Write** |
| `alert_delete` | Delete an Alert | **Write** |
| `alert_get_deliveries` | See what an Alert matched and where it was sent | Read |
| `trend_list` | List your Trends | Read |
| `trend_get` | Get a Trend's time series | Read |
| `trend_create` | Track how often a topic comes up, by hour or day | **Write** |
| `trend_update` | Change a Trend's query, interval or window | **Write** |
| `trend_delete` | Delete a Trend | **Write** |

### Tagging

Your own labels on collected posts. These do not search Reddit.

| Tool | What it does | Access |
|---|---|---|
| `tag_rule_list` | List your tagging rules | Read |
| `tag_rule_create` | Create rules that label matching posts automatically | **Write** |
| `tag_rule_update` | Rename or change a tagging rule | **Write** |
| `tag_rule_delete` | Delete a tagging rule | **Write** |
| `tag_record` | Add tags to one record | **Write** |

### Account & usage

| Tool | What it does | Access |
|---|---|---|
| `get_account` | Your organization, plan and connected Slack channels and webhooks | Read |
| `get_usage` | Today's live-read usage, daily cap and reset time | Read |
| `get_operational_alerts` | Failed collection runs and failed webhook deliveries | Read |
| `get_product_profile` | Your product profile: description, audience, competitors | Read |
| `update_product_profile` | Update your product profile | **Write** |

### Research

| Tool | What it does | Access |
|---|---|---|
| `research_get` | Read your community research report: ranked subreddits with quoted evidence, competitors and voice of customer, if Prowlo has prepared one for your workspace | Read |

Full API reference: <https://prowlo.com/docs/api-reference>

## Sources

| Source | Watch (collected into your records) | Live reads |
|---|---|---|
| **Reddit** | Subreddits and keyword searches across all of Reddit | Yes |
| **Hacker News** | Yes | Yes |
| **Mastodon** | Yes | — |
| **RSS** | Any feed | — |
| **X** | Accounts (paid plan) | — |

Each Watcher follows one source. When your agent creates a Watcher, it sets the `platform` to one of:

| `platform` | Watches |
|---|---|
| `reddit` | One subreddit (the default) |
| `reddit_search` | A search query across all of Reddit, checked daily (a Keyword Monitor) |
| `hackernews` | A Hacker News feed or query |
| `mastodon` | A Mastodon hashtag or account feed |
| `rss` | Any RSS or Atom feed |
| `x_user` | One X account (paid plan only) |

New posts are collected on an hourly schedule and become searchable by meaning. Keyword Monitors search all of Reddit once a day.

## Limits & pricing

**Free 14-day trial, no card to start. Then $19/month** (or $190/year).

The plan includes:

- 25 Watchers, each checked hourly
- 20 Keyword Monitors across all of Reddit (3 during the trial)
- 200 live reads per day (50/day during the trial)
- 10 Alerts + Trends and 3 webhook destinations
- MCP, REST and webhooks

What counts against a limit:

- **Live reads** (`social_*` tools) use 1 credit each, against a daily cap.
- **Record comment threads** (`get_record_comments`) have their own daily cap.
- **Your records, the catalog and research reports** are not metered, only rate-limited.

Ask your agent to run `get_usage` to see today's usage and when it resets. Current details: <https://prowlo.com/pricing>.

## Privacy & security

- **Read-only.** Prowlo never posts, comments or votes on any platform.
- **No platform credentials.** You never hand over a Reddit or X login or API key.
- **OAuth sign-in.** Your client signs in to your Prowlo account through a browser. The API key is only a fallback for clients that need one.
- **Revocable.** Disconnect in your client, or revoke under **Settings → Connected Apps** in your Prowlo account (admins).
- **Scoped to your account.** Your Watchers, records, Alerts and tags belong to your organization.

**Responsible use.** Don't use Prowlo to build profiles of individuals or track specific people.

Read the [Privacy Policy](https://prowlo.com/privacy) and [Terms of Service](https://prowlo.com/terms).

## Support

- Email: [support@prowlo.com](mailto:support@prowlo.com)
- Setup guide: <https://prowlo.com/docs/mcp-setup>
- Bugs or doc fixes for this repo: [open an issue](https://github.com/prowlo-com/prowlo-mcp/issues)

## About this repository

This repository holds documentation, connection examples and registry manifests only. It contains no server code and there is nothing to install or run: Prowlo is a hosted service at `https://api.prowlo.com/mcp`.

| File | Purpose |
|---|---|
| [`server.json`](server.json) | Official MCP Registry entry (`com.prowlo/reddit-x`) |
| [`plugin.json`](plugin.json) + [`mcp.json`](mcp.json) | [Agent Plugins](https://agent-plugins.org) manifest and server config |
| [`examples/`](examples/) | Per-client connection configs |

The Claude Code plugin with skills lives in its own repository: [github.com/prowlo-com/prowlo-plugin](https://github.com/prowlo-com/prowlo-plugin).

## License

The [MIT License](LICENSE) covers the documentation and examples in this repository. The Prowlo service itself is proprietary and is governed by the [Terms of Service](https://prowlo.com/terms).
