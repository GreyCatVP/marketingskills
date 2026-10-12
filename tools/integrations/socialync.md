# Socialync

Cross-posting and scheduling for Instagram, TikTok, YouTube, Facebook Pages, X, LinkedIn, Threads and Bluesky through a hosted MCP server, including scheduled video to TikTok and YouTube.

## Capabilities

| Integration | Available | Notes |
|-------------|-----------|-------|
| API | - | No public REST API. The MCP server accepts an API key for headless clients |
| MCP | ✓ | Hosted, streamable HTTP at `https://mcp.socialync.io/mcp`, 22 tools, OAuth 2.1 with dynamic client registration. Official MCP Registry: `io.github.JackVitick/socialync` |
| CLI | - | No official CLI. Generic MCP CLIs can call the server |
| SDK | - | Not available |

## Authentication

- **Type**: OAuth 2.1 with dynamic client registration and PKCE (preferred), or an API key
- **Header** (API key only): `Authorization: Bearer {api_key}`
- **Get key**: https://www.socialync.io/api-keys. OAuth clients need no key; the user signs in to Socialync in the browser and picks which brands the agent may manage.
- **Plans**: MCP is included on the free plan (5 posts a month, no card, X excluded). Paid plans start at $20 a month for 5 connected accounts and add unlimited posts, X, AI captions and analytics. https://www.socialync.io/pricing

## Setup

```bash
# Claude Code
claude mcp add --transport http socialync https://mcp.socialync.io/mcp
```

Claude (web and desktop): Socialync is listed in Claude's connector directory (Settings, Connectors, Browse connectors, search Socialync), or add a custom connector with `https://mcp.socialync.io/mcp`. Setup for Codex, Cursor, ChatGPT, Gemini CLI and others: https://github.com/JackVitick/socialync-mcp#connect

## Approval model

The server's instructions tell the agent to show the user the exact post (platforms, text, media, time) and get an explicit yes in chat before calling `publish_now`, `publish_scheduled` or `approve_draft`. Users can also switch on a confirmation dialog in Socialync settings, which clients that support MCP elicitation show before each publish. `create_post_draft` and `schedule_post_draft` save a draft the user approves in the Socialync app instead. The four publishing tools carry `destructiveHint: true`, so clients that honour tool annotations ask before running them. Each publish is logged with the credential that requested it.

## Common Agent Operations

### Find the profile and connected accounts

```
list_profiles      {}
list_connections   { "profileId": "<id>" }
check_quota        {}
```

`check_quota` returns posts remaining, the daily cap for AI-assisted posts (10 per profile per UTC day on paid plans, 5 on free), the scheduling window and whether AI generation is available.

### Validate before posting

```
validate_post {
  "profileId": "<id>",
  "platforms": ["linkedin", "twitter"],
  "caption": "LinkedIn version of the post",
  "platformSpecificSettings": { "twitter": { "customCaption": "Shorter X version" } },
  "firstComment": "Full post: https://example.com/post",
  "scheduledFor": "2026-11-03T14:00:00Z"
}
```

Read-only. Returns errors (certain publish failures) and warnings per platform.

### Schedule a post (after the user approves it)

```
publish_scheduled { ...same arguments as validate_post... }
```

Returns scheduled post IDs and a `groupId`. `get_scheduled_posts` lists the queue.

### Schedule one video to TikTok, Reels and Shorts

```
list_media {}
publish_scheduled {
  "platforms": ["tiktok", "instagram", "youtube"],
  "mediaId": "upload:abc123",
  "title": "YouTube title",
  "caption": "Shared caption",
  "platformSpecificSettings": { "tiktok": { "customCaption": "TikTok caption" } },
  "tiktokPrivacyLevel": "<the user's choice>",
  "scheduledFor": "2026-11-04T23:00:00Z"
}
```

TikTok needs the user's own choice of who can view the video: `PUBLIC_TO_EVERYONE`, `MUTUAL_FOLLOW_FRIENDS`, `FOLLOWER_OF_CREATOR` (private accounts only) or `SELF_ONLY`. Ask the user; never pick one. One answer can cover a batch the user approves together.

Video is scheduled only, one per post. The user uploads the file in Socialync (an agent cannot pass video bytes through chat) or the agent passes a direct public `videoUrl`. Files the agent generated go through `create_media_upload`, an HTTP PUT per file, then `finalize_media_upload`.

### Read performance

```
get_analytics   { "period": "week", "breakdown": "by_platform" }
get_top_posts   { "sort": "engagement", "limit": 10 }
get_post_history {}
```

Analytics cover TikTok, YouTube, Instagram, Facebook, Threads and Bluesky on paid plans. X and LinkedIn have no metrics here.

### Cancel a scheduled post

```
delete_scheduled_post { "groupId": "<groupId>" }
```

## Limits

- TikTok and YouTube take video only; YouTube needs a title.
- Instagram needs an image or a video.
- Up to 10 images per post (X and Bluesky publish 4, LinkedIn 9).
- Pinterest is not available through MCP.
- There is no tool to edit a scheduled post's content; cancel and schedule again.

## Links

- Tool reference: https://github.com/JackVitick/socialync-mcp/blob/main/docs/tools.md
- Setup guide: https://www.socialync.io/mcp-connector-guide
- Discovery document: https://mcp.socialync.io/.well-known/mcp
- Agent skills: https://github.com/JackVitick/socialync-mcp/tree/main/skills
