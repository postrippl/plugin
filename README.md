# Postrippl for Claude Code and Cursor

Schedule and publish social media posts from your agent. [Postrippl](https://postrippl.com) posts to Instagram, TikTok, X, YouTube, LinkedIn, Facebook, Threads, Pinterest, Bluesky and Discord.

This plugin adds:

- **The Postrippl MCP server** (`https://postrippl.com/api/mcp`). You sign in with your Postrippl account in the browser. There is no API key to copy.
- **`schedule` skill**: how to check your accounts and limits, write a caption per platform, attach local images and videos, and schedule, publish or draft.
- **`announce` skill**: turns a release, merged pull request or changelog entry into launch posts.

You need a Postrippl account with at least one connected social account.

## Claude Code

```
/plugin marketplace add postrippl/plugin
/plugin install postrippl@postrippl
```

Then run `/mcp`, pick `plugin:postrippl:postrippl` and sign in. Try:

- "Schedule this for tomorrow at 9am on X and LinkedIn: ..."
- "Post ./screenshots/launch.png to Instagram with a caption about the new calendar."
- `/postrippl:announce v1.4.0`

## Cursor

Install **Postrippl** from the Cursor Marketplace (Customize in the sidebar, then Plugins). Sign in when Cursor asks to connect the `postrippl` MCP server. Run the skills with `/schedule` and `/announce`.

## Without the plugin

Any MCP client can connect to `https://postrippl.com/api/mcp` directly. Setup guides: [Claude Code](https://postrippl.com/guides/claude-code), [Cursor](https://postrippl.com/guides/cursor), [Claude](https://postrippl.com/guides/claude), [ChatGPT](https://postrippl.com/guides/chatgpt).

## Files

| Path | What it is |
|---|---|
| `.claude-plugin/plugin.json`, `.claude-plugin/marketplace.json` | Claude Code manifest and marketplace |
| `.cursor-plugin/plugin.json` | Cursor manifest |
| `.mcp.json` / `mcp.json` | The MCP server, for Claude Code / Cursor |
| `skills/` | The `schedule` and `announce` skills |

## License

MIT
