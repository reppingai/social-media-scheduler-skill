# Social Media Scheduler Skill + MCP for Claude, Codex & AI Agents

**Schedule, cross-post and analyze social media posts on 20+ platforms from Claude Code, Claude.ai, Codex, Cursor, Gemini CLI, VS Code or ChatGPT.**
An open-source (MIT) AI agent skill plus a hosted MCP server, powered by [Repping.ai](https://www.repping.ai).

> Tell your AI: *"Schedule this Reel to Instagram, TikTok and YouTube Shorts tomorrow at 6 pm, then show me last month's top posts."*
> The skill finds your accounts, uploads the video, applies each platform's settings, picks the time, and sends you one review link.

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
![Claude Skill](https://img.shields.io/badge/Claude-Agent%20Skill-orange)
![MCP](https://img.shields.io/badge/MCP-Streamable%20HTTP-blue)
![Platforms](https://img.shields.io/badge/platforms-20%2B-purple)

---

## Why this skill

- **One prompt, every channel.** Cross-post to Instagram, TikTok, YouTube, X (Twitter), LinkedIn, Facebook, Threads, Pinterest, Bluesky, Reddit, Mastodon, Telegram, Discord and more.
- **Bulk scheduling.** Plan a full week or month: up to 50 posts × 20 accounts in one batch.
- **Analytics built in.** Top posts, engagement rate, follower growth, best time to post, per-platform rollups.
- **No hallucinated posts.** The skill reads live schemas and real account IDs. It never claims "scheduled" until the server confirms.
- **Human in control.** Every change is a proposal you approve in Repping (or turn on auto-approve for your own scheduling).
- **No API key to leak.** Secure OAuth sign-in. Tokens never enter the chat.

## What you can ask

| Task | Example prompt |
| --- | --- |
| Cross-post | "Post `launch.mp4` to my Instagram Reels, TikTok and YouTube Shorts Friday 7 pm EST." |
| Content calendar | "Build and schedule a 2-week LinkedIn + X calendar from these 6 blog links." |
| Best time | "When should I post on Instagram? Schedule my next 3 posts at the best times." |
| Analytics | "Which 5 posts performed best across all channels last 30 days, and why?" |
| Fix failures | "Why did yesterday's TikTok post fail? Retry it." |
| Queues | "Set a posting queue: Mon/Wed/Fri 9 am for my Threads account." |
| Bulk edits | "Push every post scheduled this week back by 2 hours." |

## Supported platforms

Instagram (Feed, Carousel, Reels, Stories) · TikTok · YouTube & Shorts · X / Twitter · LinkedIn · Facebook Pages · Threads · Pinterest · Bluesky · Mastodon · Telegram · Discord · Reddit · Slack · VK · Lemmy · Dev.to · Hashnode · Medium · WordPress

Details and limits: [`platforms.md`](skills/social-media-scheduler/references/platforms.md).

---

## Quick start (2 minutes)

1. **Create a Repping account** at [repping.ai](https://www.repping.ai) and connect your social accounts. MCP access is on the Creator and Pro plans ([pricing](https://www.repping.ai/pricing)).
2. **Add the MCP server** to your agent (below). URL: `https://www.repping.ai/api/mcp`
3. **Install the skill** (below).
4. Ask: *"Check my Repping connection and list my accounts."*

### Claude Code

Plugin (skill + MCP in one step):

```bash
/plugin marketplace add reppingai/social-media-scheduler-skill
/plugin install social-media-scheduler@repping-social
```

Or manually:

```bash
claude mcp add --transport http repping https://www.repping.ai/api/mcp
git clone https://github.com/reppingai/social-media-scheduler-skill
cp -r social-media-scheduler-skill/skills/social-media-scheduler ~/.claude/skills/
```

Then run `/mcp` and authenticate `repping`.

### Claude.ai and Claude Desktop

1. Settings → **Connectors** → **Add custom connector** → URL `https://www.repping.ai/api/mcp` → choose **Register automatically**.
2. Settings → **Capabilities → Skills** → upload a zip of `skills/social-media-scheduler`.

### OpenAI Codex CLI

```bash
codex mcp add repping --url https://www.repping.ai/api/mcp
codex mcp login repping
mkdir -p ~/.codex/skills && cp -r skills/social-media-scheduler ~/.codex/skills/
```

Codex also reads [`AGENTS.md`](AGENTS.md) when you work inside this repo.

### Cursor

`~/.cursor/mcp.json`:

```json
{ "mcpServers": { "repping": { "url": "https://www.repping.ai/api/mcp" } } }
```

Copy `skills/social-media-scheduler` into `.cursor/skills/` (or reference `SKILL.md` in a rule).

### VS Code (GitHub Copilot agent mode)

`.vscode/mcp.json`:

```json
{ "servers": { "repping": { "type": "http", "url": "https://www.repping.ai/api/mcp" } } }
```

### Gemini CLI

`~/.gemini/settings.json`:

```json
{ "mcpServers": { "repping": { "httpUrl": "https://www.repping.ai/api/mcp" } } }
```

Gemini reads [`GEMINI.md`](GEMINI.md) in this repo.

### Windsurf, OpenCode, Cline, other MCP clients

Add a **Streamable HTTP** MCP server named `repping` with URL `https://www.repping.ai/api/mcp` and OAuth. Load `skills/social-media-scheduler/SKILL.md` as the agent's instructions.

### ChatGPT

Plugins → **Add custom MCP server** → `https://www.repping.ai/api/mcp` (Business / Enterprise workspaces).

---

## How it works

```
You ──prompt──▶ AI agent + social-media-scheduler skill
                   │  repping_discover  (live schemas)
                   │  repping_read      (accounts, posts, analytics)
                   │  repping_upload_media
                   │  repping_prepare   (immutable proposal)
                   ▼
              Repping review link ──you approve──▶ published on 20+ platforms
```

| MCP tool | Purpose |
| --- | --- |
| `repping_profile` | Confirm the connection |
| `repping_discover` | List domains, operations, exact JSON schemas |
| `repping_read` | Accounts, posts, queues, analytics |
| `repping_upload_media` | Upload local images and videos |
| `repping_prepare` | Create a schedule / edit / cancel proposal |
| `repping_action_status` | Track a proposal |

## Repository layout

```
skills/social-media-scheduler/
├── SKILL.md               # main skill instructions
└── references/
    ├── platforms.md       # per-platform capabilities
    ├── workflows.md       # worked JSON examples
    ├── analytics.md       # analytics report template
    └── install.md         # connection steps per agent
.claude-plugin/            # Claude Code plugin + marketplace manifest
.mcp.json                  # MCP server config
AGENTS.md / GEMINI.md      # entry points for Codex, Gemini and other agents
```

## FAQ

**Is it free?** The skill is MIT and free. Scheduling runs on your Repping account; MCP access needs the Creator or Pro plan.

**Do I need an API key?** No. You sign in with OAuth. Nothing secret goes into prompts or config files.

**Can the AI post without me?** Not by default. Every action is a proposal you approve. You can turn on *Auto-approve agent scheduling* in Repping Settings for your own posts (≥5 minutes ahead, daily cap 50).

**Is this a Buffer / Hootsuite / Later / Postiz alternative?** Yes, for people who want to run social media from an AI agent. Repping also includes link in bio, media library and analytics.

**Which AI agents work?** Any MCP client with Streamable HTTP + OAuth: Claude Code, Claude.ai, Claude Desktop, Codex, Cursor, VS Code Copilot, Gemini CLI, Windsurf, ChatGPT.

## Related

- [reppingai/repping-mcp](https://github.com/reppingai/repping-mcp) — official Repping MCP server setup and the `creator-content-workflow` drafting skill.
- [repping.ai/mcp](https://www.repping.ai/mcp) — MCP documentation.

## Contributing

Issues and PRs are welcome: new workflows, platform tips, prompt examples, translations.

## License

[MIT](LICENSE) © Repping Labs LLC

<sub>Keywords: social media scheduler, AI social media manager, Claude skill, Claude Code skill, agent skills, MCP server, social media MCP, Codex skill, schedule Instagram posts with AI, TikTok scheduler, YouTube Shorts scheduler, LinkedIn scheduler, content calendar AI, cross-posting tool, social media analytics, best time to post, Buffer alternative, Hootsuite alternative, Postiz alternative.</sub>
