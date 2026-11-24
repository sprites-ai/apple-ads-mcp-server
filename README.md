# Apple Ads MCP Server

Connect Apple Ads (App Store search ads) to Claude, ChatGPT, Cursor and any MCP client. Search term reports, budgets, bids, keywords and negative keywords, with every change approved by you first.

![License: MIT](https://img.shields.io/badge/license-MIT-blue) ![MCP: remote](https://img.shields.io/badge/MCP-remote-6E56CF) ![Transport: streamable HTTP](https://img.shields.io/badge/transport-streamable%20HTTP-informational) ![Auth: OAuth](https://img.shields.io/badge/auth-OAuth-success) ![Hosted by Sprites](https://img.shields.io/badge/hosted%20by-Sprites-111111)

**Server URL:** `https://mcp.sprites.ai/mcp` · streamable HTTP · OAuth sign-in · 6 read tools · 14 write tools · every write approved by you

> This repository holds the documentation, client configs and MCP Registry metadata for the hosted
> Apple Ads MCP server run by [Sprites](https://www.sprites.ai). The server itself is hosted by Sprites and its source is not
> in this repository. There is nothing to install or run.

## What's new

| Date | Change |
|---|---|
| 2026-11-23 | Initial public release of connector docs and configs |

## Works with

- **Claude**: claude.ai and Claude Desktop, custom connector (renders approval cards)
- **Claude Code**: `claude mcp add`
- **ChatGPT**: developer mode app (Plus, Pro, Business, Enterprise, Edu) (renders approval cards)
- **Codex**: `codex mcp add` or `config.toml`
- **Cursor**: `mcp.json`
- **Gemini CLI**: `settings.json`
- **GitHub Copilot (VS Code)**: `.vscode/mcp.json`, agent mode
- **Google Antigravity**: `mcp_config.json`
- **Windsurf**: `mcp_config.json`
- **Lovable**: Connectors, Custom MCP
- **Grok**: grok.com/connectors, Custom
- **Cline**: Remote Servers, Streamable HTTP
- **Devin**: Customize, MCPs
- **Replit Agent**: Integrations, MCP Servers

Any other client that speaks remote MCP over streamable HTTP with OAuth should work with the same URL.

## Quick start

1. **Connect Apple Ads in Sprites.** Sign up at [sprites.ai](https://www.sprites.ai) and connect Apple Ads once in the Sprites integrations menu. Start a trial from the Sprites site.
2. **Add the server to your client** with the URL `https://mcp.sprites.ai/mcp` (one endpoint for every client and every channel).
3. **Sign in.** Your client opens the Sprites consent screen. Sign in with the account your channels are connected to and approve.
4. **Ask.** Try: "With Sprites, how did Apple Ads do last 30 days vs the 30 before?"

## Setup by client

### Claude (claude.ai and Claude Desktop)

Works on Free (one custom connector), Pro, Max, Team and Enterprise.

1. Settings, then Customize, then Connectors. On Team and Enterprise an Owner adds it once under Organization settings, Connectors.
2. Click **+**, then **Add custom connector**. Name it `Sprites` and paste `https://mcp.sprites.ai/mcp`. Leave Advanced settings empty.
3. Click **Connect**, sign in to Sprites and approve.

Claude draws the Sprites approval card in the chat, so you can edit a budget or a headline in place and approve it.

### Claude Code

```bash
claude mcp add --transport http sprites https://mcp.sprites.ai/mcp
```

Then type `/mcp` inside Claude Code to finish the browser sign-in.

### ChatGPT

Plus, Pro, Business, Enterprise and Edu, on the web app. Not available on Free.

1. Settings, Security and login: turn on **Developer mode**. On Business and Enterprise an admin has to allow it first.
2. Settings, **Plugins** (formerly Connectors), then **+** to create an app.
3. Name `Sprites`, MCP server URL `https://mcp.sprites.ai/mcp`, Authentication **OAuth**. Tick the trust box and click Create.
4. Connect the app and sign in to Sprites.

### Cursor

`~/.cursor/mcp.json` (every project) or `.cursor/mcp.json` (one repo):

```json
{
  "mcpServers": {
    "sprites": {
      "url": "https://mcp.sprites.ai/mcp"
    }
  }
}
```

Cursor discovers the Sprites authorization server from the endpoint and opens a browser to sign in.

### Codex

```bash
codex mcp add sprites --url https://mcp.sprites.ai/mcp
codex mcp login sprites
```

Or by hand in `~/.codex/config.toml`:

```toml
[mcp_servers.sprites]
url = "https://mcp.sprites.ai/mcp"
auth = "oauth"
```

### VS Code and GitHub Copilot

`.vscode/mcp.json`. The top-level key is `servers`, not `mcpServers`, and a remote server needs `"type": "http"`:

```json
{
  "servers": {
    "sprites": {
      "type": "http",
      "url": "https://mcp.sprites.ai/mcp"
    }
  }
}
```

Run **Auth** above the entry, sign in, then switch Copilot Chat to **Agent** mode. Ask mode does not call MCP tools.

### Gemini CLI

`~/.gemini/settings.json`. Use `httpUrl`; a `url` entry is treated as SSE:

```json
{
  "mcpServers": {
    "sprites": {
      "httpUrl": "https://mcp.sprites.ai/mcp"
    }
  }
}
```

Restart the CLI; the first call opens the sign-in. Check with `gemini mcp list`.

### Other clients

| Client | How |
|---|---|
| Windsurf | `~/.codeium/windsurf/mcp_config.json` with `"serverUrl": "https://mcp.sprites.ai/mcp"` |
| Google Antigravity | `~/.gemini/config/mcp_config.json` with `"serverUrl": "https://mcp.sprites.ai/mcp"` (`url` and `httpUrl` are skipped) |
| Cline | Remote Servers tab, transport Streamable HTTP; in the file, `"type": "streamableHttp"` |
| Lovable | Connectors, Custom MCP, paste the URL |
| Grok | grok.com/connectors, New Connector, Custom (paid tiers) |
| Devin | Customize, MCPs, Add custom MCP, transport HTTP, authentication OAuth |
| Replit Agent | Integrations, MCP Servers, Add MCP server, Test & Save |

Copy-ready files for each are in [`configs/`](configs/).

## What it can read

- Campaigns, ad groups, ads, keywords and their status
- Spend, impressions, taps, tap-through rate, cost per tap, installs and cost per install
- Reports at campaign, ad group, keyword and search term level
- Keyword and recommendation research

## What it can change (with your approval)

- Budgets, bids and statuses
- Keywords and negative keywords
- Create and edit campaigns, ad groups and ads
- Apply or dismiss Apple's daily budget and target CPA recommendations

Every change arrives as an approval with the before and after. In Claude and ChatGPT it is a card you can edit
in place. In clients without cards, Sprites returns the change written out with a gate id, and applies it only
after you confirm (`approve_change`). Nothing is applied by the model on its own.

## Permissions

- **API key, not a sign-in.** Apple Ads has no three-legged OAuth. An account admin creates an API user in Apple Ads once. Sprites generates the key pair; you paste the public key under Account Settings > API, then copy the `clientId`, `teamId` and `keyId` Apple shows back into Sprites. The private key stays with Sprites.
- **Roles.** The API user's role decides what Sprites can do. `Admin`, `API Account Manager` or `Limited Access: API Read & Write` can change the account. A read-only role lets Sprites report but not change anything.
- **Account selection.** The ad account comes from the connection, never from a tool argument, so the agent cannot reach an account you did not select.
- **Your AI client never holds an Apple credential.** It holds a Sprites OAuth token bound to `https://mcp.sprites.ai/mcp`.

## Tools

The server exposes 20 tools to the model: 6 read tools and 14 write tools. Write tools that touch an ad account or a site only propose; nothing reaches either until you approve it. The tools that do the Apple Ads work:

| Tool | What it does on Apple Ads |
|---|---|
| `analyze` | Spend, taps, installs and cost per install, with the period comparison. Name Apple Ads in the goal. |
| `optimize` | Search terms to promote or negate, bids and budgets to move. |
| `change` | One budget, bid, status, keyword or negative edit. |
| `launch` | A new Apple Ads campaign and ad group as approval cards. |
| `run_task` | Anything else on Apple Ads, such as setup questions. |

The tool schema has no Apple-specific `channel` value yet. Leave `channel` as `unknown` (the default) and name Apple Ads in the request; Sprites hands it to its Apple Ads specialist.

Every task tool takes the same input: `goal` (the request written to stand on its own: Sprites sees only this
string, not the chat), `title` (a short label), `channel`, and `today` (the user's local date, so "last 30 days"
resolves correctly). Long tasks return a run id; `task_status` collects the result and `cancel_task` stops it.

<details>
<summary>Full tool list (20 tools, plus <code>approve_change</code> on clients without cards)</summary>

| Tool | Type | What it does |
|---|---|---|
| `analyze` | read | Performance, reports, spend, revenue and Shopify sales questions, period comparisons, diagnosing a drop. Reads only. |
| `research` | read | Competitors and their ads, keyword research (paid or SEO), market and audience research, AI visibility. |
| `optimize` | write | Audits, wasted spend, scaling winners, moving budget. Reads, reports, and queues the recommended changes as approval cards. |
| `create_creative` | write | Ad copy, headlines and descriptions, and written concepts for image or video ads. Writes creative; does not generate images, video or audio. |
| `launch` | write | New campaigns, ad sets or ad groups, audiences, retargeting, duplicating a campaign, A/B tests, proposed as approval cards. |
| `change` | write | One named edit: budget, pause or enable, keywords or negatives, targeting, rename. Opens one approval card. |
| `schedule_task` | write | Recurring work that runs inside Sprites: weekly briefs, monitors, alerts, recurring content. |
| `write_seo_blog_post` | write | A researched blog post for your site, drafted in your CMS and published once approved. |
| `create_seo_page` | write | Create or edit a landing or service page, meta titles and descriptions, schema; a diff card before publishing. |
| `create_aeo_content` | write | Content meant to be cited by ChatGPT, Perplexity, Gemini or AI Overviews: answer, FAQ and comparison pages, llms.txt. |
| `run_task` | write | Anything else: setup, capability questions, cross-channel and multi-step jobs. |
| `linkedin_ads_report` | read | LinkedIn Ads performance: spend, leads, CPL, CTR, reach, period comparisons. |
| `linkedin_forecast` | read | LinkedIn's own forecast for up to 5 audiences: members, cost per lead, CPC and CTR over 30 days. Creates nothing. |
| `linkedin_ads_campaign` | write | Create a LinkedIn campaign: Sponsored Content, lead gen forms, message ads, retargeting. |
| `linkedin_abm_campaign` | write | LinkedIn account-based marketing: target account lists, company matched audiences. |
| `linkedin_ads` | write | Any other LinkedIn Ads task, routed by intent (audit, budget, pause, targeting, creative). |
| `account_status` | read | Which channels are connected in Sprites and how many credits are left. |
| `choose_business` | write | Select which Sprites business the tools work on, by name, or list your businesses. |
| `task_status` | read | Status and result of a Sprites task by its run id. |
| `cancel_task` | write | Stop a running task. |
| `approve_change` | write | Only on clients that do not render the approval card (Codex, Cursor, Claude Code and others): apply or decline a pending change by its gate id, after you confirm it. |

</details>

## Example prompts

1. With Sprites, how did Apple Ads do last 30 days vs the 30 before?
2. Use Sprites to show Apple Ads search terms with spend and no installs this month.
3. Ask Sprites to add those search terms as negative keywords in Apple Ads.
4. Have Sprites raise the Apple Ads bid on our brand keyword to $2.50.
5. With Sprites, which Apple Ads keywords have the lowest cost per install?
6. Use Sprites to set the Apple Ads campaign daily budget to $100.
7. Ask Sprites to pause the Apple Ads ad groups with no installs in 30 days.
8. With Sprites, find new Apple Ads keywords for our app category.
9. Have Sprites review Apple's recommendations for our account and say which to apply.
10. Use Sprites to send me a weekly Apple Ads install and CPI summary.

More in [`examples/prompts.md`](examples/prompts.md). Saying "with Sprites" or "use Sprites" helps clients that
rank their own built-in tools first (Cursor, Grok).

## FAQ

**Is this a server I have to host?**
No. It is a hosted remote MCP server. Paste `https://mcp.sprites.ai/mcp` into your client and sign in.

**Is this Apple Search Ads?**
Yes. Apple renamed Apple Search Ads to Apple Ads. This covers ads in App Store search results.

**Why an API key instead of a sign-in?**
Apple Ads API access uses a key pair tied to an API user. Sprites generates the pair, so you only paste the public key into Apple Ads.

**Sprites says it can report but not change my account. Why?**
The Apple Ads API user is read-only. Give it the API Account Manager role to allow changes.

**Do I pick a channel for Apple Ads?**
No. Leave `channel` as `unknown` and say Apple Ads in the request.

**Does it cover other channels?**
Yes. The same URL reaches Google Ads, Meta Ads, LinkedIn Ads, TikTok Ads, Reddit Ads, Microsoft Advertising,
Apple Ads, GA4, Search Console, Shopify, SEO and AI visibility, and Google Business Profile, for whatever is
connected in Sprites. See [ads-mcp-servers](https://github.com/sprites-mcp/ads-mcp-servers).

**How much does it cost?**
The connector is included in every Sprites plan, and every plan starts with a short trial. See sprites.ai for current plans. Apple Ads charges its own media
cost as usual.

**The client says it has no tools, or every call is unauthorized.**
Remove the server, re-add it with exactly `https://mcp.sprites.ai/mcp`, and sign in again. Grants are bound to that one URL.

**How do I revoke access?**
Disconnect the connector in your client, or remove the connection in Sprites. The next tool call fails closed.

## Maintained by Sprites

This server is built and run by [Sprites](https://www.sprites.ai), an AI marketing agent for paid ads, SEO and AI visibility.

- Product page: [sprites.ai/integrations](https://www.sprites.ai/mcp/apple-ads)
- All Sprites MCP servers: [github.com/sprites-mcp](https://github.com/sprites-mcp)
- Issues with these docs or configs: open an issue here. Security reports: see [SECURITY.md](SECURITY.md).

Licensed under the [MIT License](LICENSE). The license covers the documentation and configuration files in this repository.
