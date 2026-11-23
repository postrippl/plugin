---
name: announce
description: Turn a release, merged pull request, changelog entry or shipped feature into social media posts and schedule them with Postrippl. Use when the user asks to announce, share or post about something they shipped.
argument-hint: "[tag, PR number, commit range or feature]"
---

# Announce what you shipped

Write launch posts from the code the user just shipped, then schedule them with Postrippl. Follow the `schedule` skill for the Postrippl calls and per-platform limits.

## 1. Find what shipped

Use what the user named (`$ARGUMENTS`). If they named nothing, start from the latest tag or the last merged pull request:

- `git describe --tags --abbrev=0` and `git log <previous-tag>..HEAD --oneline` for a release.
- The pull request title and description for a single change (`gh pr view <n>` if `gh` is available).
- `CHANGELOG.md` or release notes when the project keeps them.

Read enough of the diff to know what a user of the product can now do. Skip refactors, dependency bumps and internal fixes unless they're the point.

## 2. Pick the story

One announcement, one idea: the change a user would care about most. Lead with what they can do now, not how it was built. Keep version numbers and file names out of it unless the audience is developers.

## 3. Draft per platform

Call `list_social_accounts`, then draft one version for each account the user wants:

- **X / Bluesky / Threads:** one or two sentences and a link. No hashtag stacks.
- **LinkedIn:** 3 to 6 short lines: the problem, what changed, who it helps, the link.
- **Discord:** a short changelog-style note with the bullet points.
- **Instagram / TikTok / YouTube / Pinterest:** these need an image or video. Ask for a screenshot or clip (or offer to skip those accounts).

Use the link the user gives (release page, docs, landing page). Don't invent features, numbers or quotes; everything must come from the diff, the PR or the changelog.

## 4. Confirm and schedule

Show every draft with its account and time, and wait for a yes. Then call `create_post` (one time for all accounts, with `platformCaptions`) or `create_bulk_posts` (a series spread over several days). If the user isn't sure, save drafts with `isDraft: true` so they can finish them at postrippl.com.
