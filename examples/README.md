# Connection examples

Every file here connects to the same hosted server: `https://api.prowlo.com/mcp`. There is nothing to install.

| File | Client | Auth |
|---|---|---|
| [`cursor.json`](cursor.json) | Cursor (`~/.cursor/mcp.json`) | OAuth |
| [`vscode.json`](vscode.json) | VS Code (`.vscode/mcp.json` or user `mcp.json`) | OAuth |
| [`windsurf.json`](windsurf.json) | Windsurf (`~/.codeium/windsurf/mcp_config.json`) | OAuth |
| [`claude_desktop_config.json`](claude_desktop_config.json) | Claude Desktop via the `mcp-remote` stdio bridge (needs Node.js 18+) | OAuth |
| [`with-api-key.json`](with-api-key.json) | Cline, or any client that needs a key instead of OAuth | API key |

**OAuth first.** Leave out the `headers` block and the client opens a browser window to sign in to Prowlo. No key to copy or rotate.

**API key fallback.** Create a key in your Prowlo settings and replace `YOUR_PROWLO_API_KEY`. Do not commit a real key.

Claude (web, Desktop and mobile) does not need a config file at all: add `https://api.prowlo.com/mcp` as a custom connector. Claude Code uses one command. See the [main README](../README.md#connect-in-30-seconds).
