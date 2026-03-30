---
name: twitter
description: |
  Use PROACTIVELY when the user mentions Twitter, X, tweets, social media monitoring, social listening,
  influencer tracking, trending topics, or wants to post/publish to X. Covers: user profiles, timelines,
  mentions, followers, tweet search, trends, lists, communities, Spaces, and OAuth-based posting.
version: 1.0.0
---

# Twitter Command Center

Twitter/X data access and automation powered by AIsa. One API key — full Twitter intelligence.

## Prerequisites

```bash
export AISA_API_KEY="your-key"
```

Requires: `curl` or `python3`

## Read Operations (no login required)

All read endpoints use `GET` with `Authorization: Bearer $AISA_API_KEY`.

### User Data

```bash
# User profile
curl "https://api.aisa.one/apis/v1/twitter/user/info?userName=elonmusk" \
  -H "Authorization: Bearer $AISA_API_KEY"

# User about (country, verification, username history)
curl "https://api.aisa.one/apis/v1/twitter/user_about?userName=elonmusk" \
  -H "Authorization: Bearer $AISA_API_KEY"

# Recent tweets
curl "https://api.aisa.one/apis/v1/twitter/user/last_tweets?userName=elonmusk" \
  -H "Authorization: Bearer $AISA_API_KEY"

# Mentions
curl "https://api.aisa.one/apis/v1/twitter/user/mentions?userName=elonmusk" \
  -H "Authorization: Bearer $AISA_API_KEY"

# Followers / followings
curl "https://api.aisa.one/apis/v1/twitter/user/followers?userName=elonmusk" \
  -H "Authorization: Bearer $AISA_API_KEY"

curl "https://api.aisa.one/apis/v1/twitter/user/followings?userName=elonmusk" \
  -H "Authorization: Bearer $AISA_API_KEY"

# Check follow relationship
curl "https://api.aisa.one/apis/v1/twitter/user/check_follow_relationship?source_user_name=elonmusk&target_user_name=BillGates" \
  -H "Authorization: Bearer $AISA_API_KEY"

# Search users
curl "https://api.aisa.one/apis/v1/twitter/user/search?query=AI+researcher" \
  -H "Authorization: Bearer $AISA_API_KEY"
```

### Tweet Search & Details

```bash
# Search (queryType: Latest or Top)
curl "https://api.aisa.one/apis/v1/twitter/tweet/advanced_search?query=AI+agents&queryType=Latest" \
  -H "Authorization: Bearer $AISA_API_KEY"

# Tweet by ID
curl "https://api.aisa.one/apis/v1/twitter/tweets?tweet_ids=1895096451033985024" \
  -H "Authorization: Bearer $AISA_API_KEY"

# Replies / quotes / retweeters / thread
curl "https://api.aisa.one/apis/v1/twitter/tweet/replies?tweetId=1895096451033985024" \
  -H "Authorization: Bearer $AISA_API_KEY"
```

### Trends, Lists, Communities & Spaces

```bash
# Worldwide trends
curl "https://api.aisa.one/apis/v1/twitter/trends?woeid=1" \
  -H "Authorization: Bearer $AISA_API_KEY"

# Community info / members / tweets
curl "https://api.aisa.one/apis/v1/twitter/community/info?community_id=1708485837274263614" \
  -H "Authorization: Bearer $AISA_API_KEY"

# Space detail
curl "https://api.aisa.one/apis/v1/twitter/spaces/detail?space_id=1dRJZlbLkjexB" \
  -H "Authorization: Bearer $AISA_API_KEY"
```

## Posting (OAuth Relay)

No passwords or cookies. Uses AIsa's OAuth relay.

**Important:** These POST endpoints use `aisa_api_key` in the JSON body, NOT the `Authorization` header.

```bash
# 1. Get authorization URL
curl -X POST "https://api.aisa.one/apis/v1/twitter/auth_twitter" \
  -H "Content-Type: application/json" \
  -d "{\"aisa_api_key\":\"$AISA_API_KEY\"}"

# 2. User opens URL in browser and authorizes

# 3. Publish
curl -X POST "https://api.aisa.one/apis/v1/twitter/post_twitter" \
  -H "Content-Type: application/json" \
  -d "{\"aisa_api_key\":\"$AISA_API_KEY\",\"content\":\"Hello from Claude Code!\"}"
```

## Agent Instructions (posting)

When the user asks to publish to X/Twitter:

1. Ensure `AISA_API_KEY` is set.
2. If authorization is required, call `auth_twitter` and return the `authorization_url` for the user to open in their browser.
3. Never ask for Twitter passwords or use cookie/proxy login flows.
4. Do not claim the post succeeded until `post_twitter` returns success.

## Python Client

A bundled Python client is available at `${CLAUDE_PLUGIN_ROOT}/skills/twitter/scripts/twitter_client.py`:

```bash
# User operations
python3 "${CLAUDE_PLUGIN_ROOT}/skills/twitter/scripts/twitter_client.py" user-info --username elonmusk
python3 "${CLAUDE_PLUGIN_ROOT}/skills/twitter/scripts/twitter_client.py" tweets --username elonmusk
python3 "${CLAUDE_PLUGIN_ROOT}/skills/twitter/scripts/twitter_client.py" search --query "AI agents"
python3 "${CLAUDE_PLUGIN_ROOT}/skills/twitter/scripts/twitter_client.py" trends --woeid 1

# Posting
python3 "${CLAUDE_PLUGIN_ROOT}/skills/twitter/scripts/twitter_client.py" authorize
python3 "${CLAUDE_PLUGIN_ROOT}/skills/twitter/scripts/twitter_client.py" post --text "Hello from Claude Code"
```

## API Endpoints Reference

### Read (GET, Bearer auth)

| Endpoint | Description | Key Params |
|----------|-------------|------------|
| `/twitter/user/info` | User profile | `userName` |
| `/twitter/user_about` | User about | `userName` |
| `/twitter/user/last_tweets` | Recent tweets | `userName`, `cursor` |
| `/twitter/user/mentions` | Mentions | `userName`, `cursor` |
| `/twitter/user/followers` | Followers | `userName`, `cursor` |
| `/twitter/user/followings` | Followings | `userName`, `cursor` |
| `/twitter/user/search` | Search users | `query`, `cursor` |
| `/twitter/tweet/advanced_search` | Tweet search | `query`, `queryType` (Latest/Top) |
| `/twitter/tweets` | Tweets by ID | `tweet_ids` |
| `/twitter/tweet/replies` | Replies | `tweetId`, `cursor` |
| `/twitter/tweet/quotes` | Quotes | `tweetId`, `cursor` |
| `/twitter/trends` | Trends | `woeid` (1=worldwide) |
| `/twitter/community/info` | Community | `community_id` |
| `/twitter/spaces/detail` | Space | `space_id` |

### Post (POST, body auth)

| Endpoint | Description | Body Params |
|----------|-------------|-------------|
| `/twitter/auth_twitter` | OAuth URL | `aisa_api_key` |
| `/twitter/post_twitter` | Publish | `aisa_api_key`, `content`, `media_ids` |

## Pricing

~$0.0004 per read query. Every response includes `usage.cost` and `usage.credits_remaining`.

Get your API key at [aisa.one](https://aisa.one).
