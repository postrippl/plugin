---
name: schedule
description: Schedule, publish or draft social media posts with Postrippl (Instagram, TikTok, X, YouTube, LinkedIn, Facebook, Threads, Pinterest, Bluesky, Discord). Use when the user wants to post, schedule, plan or draft content for social media, attach local images or videos to a post, or check, list or delete posts they scheduled.
---

# Scheduling posts with Postrippl

Postrippl publishes to the social accounts the user connected at postrippl.com. You reach it through the `postrippl` MCP server bundled with this plugin.

## If the tools are missing or ask for sign-in

The first call opens a browser sign-in. If the Postrippl tools are not available or say the user is not authenticated, tell the user to run `/mcp`, pick `postrippl`, and sign in. There is no API key.

## Order of calls

1. `list_social_accounts`: the `id` of each account is what `accountIds` takes. If the list is empty, send the user to https://postrippl.com/sns?from=agent to connect one, and stop.
2. `get_scheduling_limits`: checks the plan and how many posts each platform can still take around the target time. Call it before posting.
3. `create_post` for one post (to one or several accounts), or `create_bulk_posts` for a series (up to 20, all scheduled, all or nothing). Pass a `batchKey` to `create_bulk_posts` so a retry never duplicates the batch.
4. `get_post` or `list_posts` to check what happened. `delete_post` removes a post; it does not delete one that is already live on the network.

## Before you post

- Show the user the exact text, accounts, media and time, and get a yes. Posting is public and can't be taken back. If they haven't said when, ask, or offer a draft (`isDraft: true`).
- Times are ISO 8601 with an offset, in the user's timezone unless they say otherwise: `2026-05-04T09:00:00+02:00`. Use `postNow: true` only when they ask to post right away.
- Some connections are drafts only. If a result says the post was saved as a draft, tell the user they schedule it in Postrippl.

## Writing for each platform

Write a caption that fits each platform rather than one text everywhere. Put the shared caption in `caption` and per-account versions in `platformCaptions`, keyed by account id.

| Platform | Caption limit | Notes |
|---|---|---|
| X | 280 (4,000 with Premium) | Short, one idea. No hashtag stacks. |
| Bluesky | 300 | Like X. |
| Threads | 500 | Conversational. |
| Pinterest | 800 | Needs an image or video and `pinterestBoardId`; ask the user for the board. |
| Discord | 2,000 | Plain text, Markdown works. |
| Instagram | 2,200 | Needs an image or video. Hashtags at the end. |
| TikTok | 2,200 | Needs a video, or images (sent as a photo carousel). |
| LinkedIn | 3,000 | First two lines carry it; line breaks help. |
| YouTube | 5,000 | Needs a video. The first line becomes the title (100 characters max). |
| Facebook | 63,206 | |

## Media

A post takes up to 4 images, or 1 video. Never mix them.

- **Public URL:** pass it in `mediaUrls`.
- **Local file** (anything on the user's disk):
  1. Get each file's size in bytes and MIME type (`image/jpeg`, `image/png`, `video/mp4`, ...).
  2. Call `request_media_upload` with `files: [{ name, type, size }]`.
  3. Upload each file with a raw PUT to the returned `uploadUrl`. The `Content-Type` header must equal the `type` you sent:
     ```bash
     curl -sS -X PUT -H "Content-Type: video/mp4" --data-binary @clip.mp4 "$UPLOAD_URL"
     ```
     Don't use form-data or base64. Upload URLs expire after 10 minutes; request new ones if needed.
  4. Pass the returned `mediaUrl` values in `mediaUrls`.

## When something fails

- A limit error: read `get_scheduling_limits` again and suggest another time or fewer accounts.
- No plan: send the user to https://postrippl.com/pricing.
- A post shows `failed`: `get_post` gives the reason per platform. Reconnecting the account at https://postrippl.com/sns fixes most token errors.
