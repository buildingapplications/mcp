# Bilt Builder

**Give your OpenClaw agent the ability to build and deploy real mobile apps.**

Bilt Builder connects your agent to the [Bilt MCP server](https://bilt.me), which handles the entire app development lifecycle: creating projects, generating code, building binaries, and deploying to production. Your agent talks in natural language. Bilt does the rest.

The result? A live URL like `https://bilt.me/project/abc-a1b2/preview` that anyone can visit.

## What Can You Build?

Anything that works as a mobile app:

- **Productivity** - Todo lists, habit trackers, note-taking apps, project managers
- **Social** - Chat apps, social feeds, community platforms
- **Commerce** - Product catalogs, order trackers, loyalty apps
- **Health** - Fitness trackers, meal planners, meditation timers
- **Utilities** - Weather apps, calculators, unit converters, QR scanners
- **Education** - Flashcard apps, quiz builders, course trackers

Apps are built with React Native and Expo, targeting iOS and Android.

## Installation

### 1. Get a Bilt API Token

Sign up at [bilt.me/sign-up](https://bilt.me/sign-up) and generate an API key from your dashboard.

### 2. Add MCP Configuration

Add the Bilt server to `~/.openclaw/openclaw.json`:

```json
{
  "mcpServers": {
    "bilt": {
      "transport": {
        "type": "sse",
        "url": "https://mcp.bilt.me/mcp/sse",
        "headers": {
          "Authorization": "Bearer bilt_live_YOUR_TOKEN_HERE"
        }
      }
    }
  }
}
```

### 3. Restart the Gateway

```bash
openclaw gateway restart
```

Your agent now has access to 8 Bilt tools. That's all.

## Quick Start

Tell your agent:

```
Build me a todo app with Bilt. It should have categories, due dates, and a
clean minimal design. Deploy it when you're done.
```

Your agent will:
1. Create the project
2. Build the app with your specifications
3. Iterate on the design
4. Deploy to production
5. Return a live URL

Typical time: under 5 minutes.

## Why This Matters

Before Bilt Builder, an agent could write code but had no way to build it, run it, or put it somewhere people could use it. The code just sat there.

With this skill, your agent has end-to-end capability. It can take a vague idea ("I need a workout tracker") and produce a working, deployed application with a URL you can share. No human in the loop. No local environment setup. No CI/CD pipeline.

This is what agent autonomy actually looks like.

## Skill Structure

```
skills/bilt-builder/
  SKILL.md            # Full documentation for agents
  README.md           # This file
  QUICKSTART.md       # 5-minute getting started guide
  skill.json          # Skill metadata
  examples/
    todo-app.md       # Complete walkthrough example
```

## Support

- [Documentation](https://bilt.me/docs)
- [API Reference](https://bilt.me/docs/api-reference/overview)
- [Discord](https://discord.gg/3FqNgmSYdZ)
- [GitHub Issues](https://github.com/buildingapplications/mcp/issues)
- [Email](mailto:support@bilt.me)
