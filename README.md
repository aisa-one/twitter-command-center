# Twitter Command Center

Search, monitor, and publish on X/Twitter from Claude Code — powered by AIsa.

## What it does

Full Twitter/X data access through a single API key. No passwords, no cookies, no proxies.

**Read (no login required):**
- User profiles, timelines, mentions, followers, followings
- Tweet search (Latest / Top), replies, quotes, retweeters, threads
- Trending topics, Lists, Communities, Spaces

**Write (OAuth relay):**
- Publish tweets after one-time browser authorization
- No credentials stored — uses AIsa's OAuth relay

## Installation

```bash
### 通过市场安装（推荐）
```bash
# 添加市场源（仅需一次）
/plugin marketplace add https://github.com/aisa-one/twitter-command-center
# 安装插件
/plugin install twitter-command-center
```

## Setup

```bash
export AISA_API_KEY="your-key"
```

Get your API key at [aisa.one](https://aisa.one). ~$0.0004 per read query, pay-as-you-go.

## Components

```
twitter-command-center/
├── .claude-plugin/
│   └── plugin.json
├── commands/
│   └── twitter-search.md        # /twitter-search slash command
├── skills/
│   └── twitter/
│       ├── SKILL.md              # Auto-activated skill
│       └── scripts/
│           └── twitter_client.py # Python client (curl + python3)
└── README.md
```

## Usage

**Automatic skill activation** — Claude detects when you're working with Twitter/X and loads the skill context:

- "What's trending on Twitter right now?"
- "Get @anthropic's latest tweets"
- "Search for tweets about AI agents"
- "Post this to X: we shipped v2.0"

**Slash command:**

```
/twitter-search AI agents --type Top
/twitter-search @elonmusk
/twitter-search trends
```

## License

MIT
