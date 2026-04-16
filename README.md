# Neiru AI — MCP Server

Connect your AI assistant (Claude, Gemini CLI, Cursor, Windsurf, etc.) to [Neiru AI](https://neiru.ai) browser automation platform via [Model Context Protocol](https://modelcontextprotocol.io/).

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

#### Gemini CLI

```bash
gemini mcp add neiru "https://neiru.ai/api/v2/mcp" \
  -t http -s user --trust \
  -H "Authorization: Bearer bak_YOUR_API_KEY"
```

That's it. Run `gemini` and ask:

```
list my browser profiles
```

> **Note:** Use `http` transport (not `sse`). Gemini CLI works best with HTTP transport.

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
        "Authorization": "Bearer bak_YOUR_API_KEY"
      }
    }
  }
}
```

#### Claude Code

```bash
claude mcp add neiru --transport sse https://neiru.ai/api/v2/mcp/sse \
  --header "Authorization: Bearer bak_YOUR_API_KEY"
```

#### Cursor / Windsurf

Add to MCP settings:

```json
{
  "mcpServers": {
    "neiru": {
      "url": "https://neiru.ai/api/v2/mcp/sse",
      "headers": {
        "Authorization": "Bearer bak_YOUR_API_KEY"
      }
    }
  }
}
```

#### HTTP (curl, n8n, Make.com)

```bash
curl -X POST https://neiru.ai/api/v2/mcp \
  -H "Authorization: Bearer bak_YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"jsonrpc":"2.0","method":"tools/list","id":1}'
```

### 3. Done

Ask your AI: *"Open browser for profile 'twitter_main' and like the latest post"*

## Available Tools

95 tools across 15 categories:

| Category | Tools | What you can do |
|----------|-------|----------------|
| **Profiles** | 9 | List, create, delete, bulk operations on browser profiles |
| **Sessions** | 6 | Open/close browsers, list tabs, switch tabs |
| **Browser Control** | 7 | Get page DOM, click elements, type text, select options, scroll, execute JS |
| **Tasks** | 7 | Run automation tasks, check status, cancel, restart |
| **Scripts** | 7 | Create and run Playwright scripts in Docker sandbox |
| **Passwords & OTP** | 5 | Store/retrieve credentials, generate TOTP codes |
| **Context** | 7 | Manage profile data (accounts, notes, documents) |
| **Workspace** | 5 | Upload/download files per profile |
| **Proxies** | 6 | Manage proxy servers, health checks |
| **Cron** | 6 | Schedule recurring tasks |
| **Prompts** | 7 | Reusable task templates with variables |
| **Media** | 5 | Screenshots, recordings, downloads |
| **Email** | 1 | Search inbox for verification codes |
| **Cross-profile** | 1 | Read messages from another profile's browser |
| **System** | 3 | Overview, activity logs, token stats |

Use `search_tools` to find the right tool by keyword.

## Browser Control (Gemini CLI as Browser Agent)

With Gemini CLI connected to Neiru, you get a free AI browser agent. Gemini sees the page DOM and decides what to click, type, or scroll.

**How it works:**

1. `open_browser` — opens Chrome with anti-detect profile
2. `navigate_to` — loads a URL
3. `get_page_dom` — returns page structure (inputs, buttons, links, text)
4. `click_element` / `type_text` / `select_option` — interacts with elements
5. `take_screenshot` — shows the result

**Example conversation:**

```
You: open browser for profile twitter_main, go to x.com, find the first post and like it

Gemini: [calls open_browser] → Browser opened, session abc123
        [calls navigate_to url=https://x.com] → Navigated
        [calls get_page_dom] → Found buttons, links, posts...
        [calls click_element text="Like"] → Clicked Like button
        [calls take_screenshot] → Here's the result: [screenshot URL]
```

**Available browser control tools:**

| Tool | Description |
|------|-------------|
| `get_page_dom` | Compact page snapshot: URL, title, text, inputs, buttons, links |
| `get_page_html` | Full HTML source (up to 50k chars) |
| `click_element` | Click by visible text or CSS selector |
| `type_text` | Type into input by label, placeholder, or CSS |
| `select_option` | Select dropdown option by text |
| `scroll_page` | Scroll up/down/top/bottom |
| `execute_js` | Run arbitrary JavaScript (admin only) |

## Examples

**Open a browser and see the page:**
> "Open browser for profile 'my_twitter', go to x.com, get the page DOM and show me what's there"

**Click and interact:**
> "Click the 'Sign in' button, type 'user@email.com' into the Email field, then take a screenshot"

**Run a Playwright script:**
> "Create a script that opens google.com and searches for 'neiru ai', then run it on profile 'parser'"

**Check passwords:**
> "What's the password for twitter on profile 'my_twitter'?"

**Bulk operations:**
> "Run this task on all profiles in group 'social': like the latest post on the feed"

**Scheduled tasks:**
> "Create a cron task that runs every 4 hours on group 'social': open feed and scroll through posts"

## Resources

Read-only data endpoints:

```
browser-antic://profiles       → List all profiles
browser-antic://sessions       → Active browser sessions
browser-antic://tasks/recent   → Last 10 tasks
browser-antic://usage          → Token and storage stats
```

## Rate Limits

- 60 requests per minute per API key
- Exceeding returns `429 Too Many Requests`

## Links

- [Neiru AI](https://neiru.ai) — Platform
- [Documentation](https://neiru.ai/blog) — Guides and tutorials
- [MCP Protocol](https://modelcontextprotocol.io/) — About MCP

## License

MIT
