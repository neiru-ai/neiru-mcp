# Neiru AI — MCP Server

Connect your AI assistant (Claude, Cursor, Windsurf, etc.) to [Neiru AI](https://neiru.ai) browser automation platform via [Model Context Protocol](https://modelcontextprotocol.io/).

Control browser profiles, run tasks, manage passwords, take screenshots — all through natural language.

## Quick Start

### 1. Get your API key

Go to [neiru.ai](https://neiru.ai) → Settings → API Keys → Create new key.

Your key looks like this:

```
bak_xK9mP2vL5nR8wQ4jT7yB3cF6hA0dS1eG
```

> Save it — the key is shown only once.

### 2. Configure your AI client

#### Claude Desktop

Open `claude_desktop_config.json`:

- **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`
- **Linux:** `~/.config/claude/claude_desktop_config.json`

Add:

```json
{
  "mcpServers": {
    "neiru": {
      "url": "https://neiru.ai/api/v2/mcp/sse",
      "headers": {
        "Authorization": "Bearer bak_xK9mP2vL5nR8wQ4jT7yB3cF6hA0dS1eG"
      }
    }
  }
}
```

#### Cursor / Windsurf

Add to MCP settings:

```json
{
  "mcpServers": {
    "neiru": {
      "url": "https://neiru.ai/api/v2/mcp/sse",
      "headers": {
        "Authorization": "Bearer bak_xK9mP2vL5nR8wQ4jT7yB3cF6hA0dS1eG"
      }
    }
  }
}
```

#### HTTP (curl, n8n, Make.com)

```bash
curl -X POST https://neiru.ai/api/v2/mcp \
  -H "Authorization: Bearer bak_xK9mP2vL5nR8wQ4jT7yB3cF6hA0dS1eG" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"tools/list","id":1}'
```

### 3. Done

Ask your AI: *"Open browser for profile 'twitter_main' and like the latest post"*

## Available Tools

73 tools across 14 categories:

| Category | Tools | What you can do |
|----------|-------|----------------|
| **Profiles** | 9 | List, create, delete browser profiles |
| **Sessions** | 3 | Open/close browsers, get session info |
| **Tasks** | 6 | Run automation tasks, check status |
| **Scripts** | 6 | Create and run Playwright scripts |
| **Passwords** | 3 | Store and retrieve credentials |
| **OTP** | 2 | Get TOTP codes for 2FA |
| **Context** | 5 | Manage profile data (accounts, notes) |
| **Workspace** | 5 | Upload/download files per profile |
| **Proxies** | 3 | Manage proxy servers |
| **Cron** | 6 | Schedule recurring tasks |
| **Prompts** | 7 | Task templates |
| **Media** | 4 | Screenshots and recordings |
| **Memory** | 3 | Agent knowledge base |
| **System** | 3 | Overview, logs, stats |

Use `search_tools` to find the right tool by keyword without loading all 73 definitions.

## Examples

**Open a browser and navigate:**
> "Open browser for profile 'my_twitter' and go to twitter.com"

**Run a task:**
> "Run task on profile 'shop_account': go to amazon.com and add the cheapest wireless mouse to cart"

**Take a screenshot:**
> "Take a screenshot of profile 'my_twitter' current page"

**Check passwords:**
> "What's the password for twitter on profile 'my_twitter'?"

**Bulk operations:**
> "Run this task on all profiles in group 'social': like the latest post on the feed"

## Resources

Read-only data endpoints for quick access:

```
browser-antic://profiles       → List all profiles
browser-antic://sessions       → Active browser sessions
browser-antic://tasks/recent   → Last 10 tasks
browser-antic://usage          → Token and storage stats
```

## Rate Limits

- 60 requests per minute per API key
- Exceeding → `429 Too Many Requests`

## Links

- [Neiru AI](https://neiru.ai) — Platform
- [Documentation](https://neiru.ai/blog) — Guides and tutorials
- [MCP Protocol](https://modelcontextprotocol.io/) — About MCP

## License

MIT
