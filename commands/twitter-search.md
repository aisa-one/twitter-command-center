---
description: "Search Twitter/X for tweets, users, or trends via AIsa API"
argument-hint: "<query> [--type Latest|Top]"
allowed-tools: [Read, Bash, Glob]
---

Search Twitter/X using the AIsa API. Requires `AISA_API_KEY` environment variable.

## Instructions

1. Parse the user's query and determine the search type:
   - Tweet search: use `/twitter/tweet/advanced_search` with `queryType=Latest` (default) or `Top`
   - User search: use `/twitter/user/search`
   - Trends: use `/twitter/trends?woeid=1`

2. Execute the search using `curl` or the bundled Python client at `${CLAUDE_PLUGIN_ROOT}/skills/twitter/scripts/twitter_client.py`.

3. Present results in a clear, readable format with key metrics (likes, retweets, followers).

## Examples

```
/twitter-search AI agents --type Top
/twitter-search @anthropic
/twitter-search trends
```
