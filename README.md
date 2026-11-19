# Photoproduct.io for AI agents

Make images, video, music, voiceovers and 3D from your AI agent with more than 1,000
generation models, including Veo, Kling, Seedance, Nano Banana Pro and FLUX.
This repository holds the Photoproduct.io plugin for Claude Code and the
`photoproduct` skill that teaches any agent how to use the Photoproduct.io MCP
server well: product photos, consistent characters, cinematic scenes and video.

Every run is paid from the credits of your own Photoproduct.io account, and you set a
daily limit for each app you connect. Setup for Claude, ChatGPT, Codex, Cursor and
other clients: https://photoproduct.io/agents

## What is inside

| Path | What it is |
|---|---|
| `.claude-plugin/plugin.json` | The Claude Code plugin manifest |
| `.claude-plugin/marketplace.json` | A one-plugin marketplace, so Claude Code can install it from this repository |
| `.mcp.json` | The Photoproduct.io MCP server, `https://photoproduct.io/mcp` (sign-in with OAuth) |
| `skills/photoproduct/SKILL.md` | The skill: which workflow fits a request, how to run and follow a job, and the ground rules |

## Install

### Claude Code

```
/plugin marketplace add photoproduct/skills
/plugin install photoproduct@photoproduct
```

Restart Claude Code if it asks, run `/mcp`, choose `photoproduct` and sign in to
Photoproduct.io in the browser window that opens.

### Other agents (skills only)

Agents that read Agent Skills can install just the skill:

```
npx skills add photoproduct/skills
```

Connect the MCP server separately, as https://photoproduct.io/agents describes for
your app.

## How it works

The skill does not copy prompt recipes into your project. It tells your agent to ask
the server's `prompt_guide` tool for the current structure of each task, so it always
follows the latest version.

## Links

- Setup for every app: https://photoproduct.io/agents
- Models and prices: https://photoproduct.io/models
- Support: https://photoproduct.io/support
- Privacy: https://photoproduct.io/privacy
- Terms: https://photoproduct.io/terms

## License

MIT, see [LICENSE](LICENSE).
