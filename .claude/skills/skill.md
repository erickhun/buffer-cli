---
name: buffer
description: Interactive social media posting. Use when the user says "/buffer" to draft and publish posts across their social channels.
user_invocable: true
---

# Buffer — Interactive Posting

## Setup

**Binary:** `buffer` (must be on your `$PATH` — see [buffer-cli](https://github.com/erickhun/buffer-cli) for install instructions)
**Config:** `~/.buffer-cli.json`

Read `~/.buffer-cli.json` at start. If it has `token`, `organizationId`, and `channels`, go straight to the posting flow.

If missing or incomplete, run first-time setup:
1. Ask for their Buffer access token (get one at developers.buffer.com)
2. Run `buffer get-account` to get orgs. If multiple, ask which one.
3. Run `buffer list-channels` to get channels. Filter out disconnected/locked.
4. Save to `~/.buffer-cli.json` (chmod 600):
```json
{
  "token": "...",
  "organizationId": "...",
  "organizationName": "...",
  "channels": [
    { "id": "...", "name": "...", "displayName": "...", "service": "twitter", "type": "profile" }
  ]
}
```

If auth fails (401), show the error and ask if they want to re-enter their token. Don't delete the config automatically.

## Posting Flow

Use `AskUserQuestion` for user interactions. Pick whichever AskUserQuestion features best fit the moment.

1. Ask "What do you want to post?" — use a plain text question with NO options, so the user can type freely
2. Let them pick which channels to post to
3. Draft content tailored to each platform's conventions, audience, and format. Consider tone, length, and structure for each channel (see platform rules below).
4. **Review drafts using markdown previews** — present one option per channel, with the draft content in the `markdown` field so the user can flip through each draft in the preview pane. The user can select any channel to edit it, or confirm when they're happy.
5. If the user edits a draft, show the updated version and re-confirm
6. Post confirmed drafts
7. Show summary

## CLI Reference

All commands use: `buffer <command> --auth-token <token>`

### get-account
Returns: account info (name, email, timezone) and organizations (id, name, limits).
```
buffer get-account
```

### list-channels
Returns: channels (id, name, displayName, service, type, avatar, isDisconnected, isLocked).
```
buffer list-channels --organization-id <org-id>
```

### create-post
```
buffer create-post \
  --channel-id <id> \
  --text "post content" \
  --mode <mode> \
  --scheduling-type <type>
```
Modes: `addToQueue` (default), `shareNow`, `shareNext`, `customScheduled`, `recommendedTime`

Scheduling types (required): `automatic` (auto-publish) or `notification` (manual approval)

For scheduled posts add: `--due-at "2025-03-15T17:00:00-05:00"`

Optional: `--assets <json>`, `--metadata <json>`, `--tag-ids <ids>`, `--save-to-draft`

Minimum requirements by service:
- Twitter/Mastodon/Threads/Bluesky: text only
- Instagram/TikTok: requires image or video asset
- Pinterest: requires image + metadata.pinterest.boardServiceId

### create-idea
```
buffer create-idea --organization-id <org-id> --content <json>
```

### list-posts
```
buffer list-posts --organization-id <org-id> [--channel-ids <ids>] [--status <statuses>] [--first <n>]
```
Status values: draft, needs_approval, scheduled, sending, sent, error

### get-post / get-channel
```
buffer get-post --post-id <id>
buffer get-channel --channel-id <id>
```

## Platform Rules

- **Twitter**: Max 280 chars. Hook-first. Text beats video by 30%.
- **LinkedIn**: Up to 3000 chars. Professional. First line is the hook. Line breaks for readability.
- **Threads**: Max 500 chars. Casual, conversational.
- **Bluesky**: Max 300 chars. Concise, tech-savvy audience.
- **Mastodon**: Max 500 chars.
- **Instagram**: Up to 2200 chars. Visual-first — note if image recommended.
- **Facebook**: Conversational. Questions drive engagement.

Adapt tone per platform. Front-load the hook.
