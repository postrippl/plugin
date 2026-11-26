# Postrippl for Claude Code and Cursor

[![Postrippl MCP connector](https://glama.ai/mcp/connectors/com.postrippl/postrippl/badges/score.svg)](https://glama.ai/mcp/connectors/com.postrippl/postrippl)

Schedule and publish social media posts from your agent. [Postrippl](https://postrippl.com) posts to Instagram, TikTok, X, YouTube, LinkedIn, Facebook, Threads, Pinterest, Bluesky and Discord.

This plugin adds:

- **The Postrippl MCP server** (`https://postrippl.com/api/mcp`). You sign in with your Postrippl account in the browser. There is no API key to copy.
- **A `schedule` skill**: how to check your accounts and limits, write a caption per platform, attach local images and videos, and schedule, publish or draft.

You need a Postrippl account with at least one connected social account.

## Claude Code

```
/plugin marketplace add postrippl/plugin
/plugin install postrippl@postrippl
```

Then run `/mcp`, pick `plugin:postrippl:postrippl` and sign in. Try:

- "Schedule this for tomorrow at 9am on X and LinkedIn: ..."
- "Post ./screenshots/launch.png to Instagram with a caption about the new calendar."
- "Plan a week of LinkedIn posts about our spring menu, one a day at 8am."

## Cursor

Install **Postrippl** from the Cursor Marketplace (Customize in the sidebar, then Plugins). Sign in when Cursor asks to connect the `postrippl` MCP server. Run the skill with `/schedule`, or just ask.

## Without the plugin

Any MCP client can connect to `https://postrippl.com/api/mcp` directly. Setup guides: [Claude Code](https://postrippl.com/guides/claude-code), [Cursor](https://postrippl.com/guides/cursor), [Claude](https://postrippl.com/guides/claude), [ChatGPT](https://postrippl.com/guides/chatgpt).

## Data and privacy

- The MCP server at `postrippl.com` receives what you ask Claude to post: captions, times, the accounts to post to, and media links. It also returns your connected accounts (platform, username, profile link) and your posts.
- When you attach a file from your computer, the `schedule` skill uploads it with an HTTP PUT to a short-lived signed URL that Postrippl returns. That URL points to Postrippl's media storage on Cloudflare R2, not to postrippl.com itself. Files never attached to a post are deleted after 24 hours.
- Posts and media are kept while your Postrippl account is open. See the [privacy policy](https://postrippl.com/privacy-policy).

## Files

| Path | What it is |
|---|---|
| `.claude-plugin/plugin.json`, `.claude-plugin/marketplace.json` | Claude Code manifest and marketplace |
| `.cursor-plugin/plugin.json` | Cursor manifest |
| `.mcp.json` / `mcp.json` | The MCP server, for Claude Code / Cursor |
| `skills/` | The `schedule` skill |

## License

MIT
