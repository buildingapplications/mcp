# Quickstart: Bilt Builder

Build and deploy your first app in under 5 minutes.

## Setup (3 Steps)

**Step 1:** Get an API token at [bilt.me/sign-up](https://bilt.me/sign-up).

**Step 2:** Add to `~/.openclaw/openclaw.json`:

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

**Step 3:** Restart the gateway:

```bash
openclaw gateway restart
```

Done. Your agent can now build apps.

## Build Your First App

Tell your agent:

```
Use Bilt to create a todo app. It should have:
- Add and delete tasks
- Mark tasks as complete
- Clean, modern UI

Deploy it when you're done and give me the URL.
```

Your agent handles the rest. You'll get a live URL in about 4 minutes.

## What Happens Behind the Scenes

```
bilt_create_project("todo-app", "Simple todo list")
  -> Project created

bilt_get_session()
  -> Session ready

bilt_send_message(session_id, "Create a todo app with add, delete, complete...")
  -> App built

bilt_send_message(session_id, "Deploy to production")
  -> https://todo-app-abc123.bilt.app
```

## Try These Next

**Simple** - "Build a countdown timer app with multiple timers and alarm sounds"

**Medium** - "Create a recipe manager with categories, search, and a grocery list that auto-generates from selected recipes"

**Complex** - "Build a personal finance dashboard with expense tracking, budget categories, monthly spending charts, and bill reminders"

## Common Issues

| Problem | Solution |
|---------|----------|
| Agent can't find Bilt tools | Restart: `openclaw gateway restart` |
| Auth error | Verify your token starts with `bilt_live_` |
| Build timeout | Use `bilt_get_messages()` to check progress, cancel and retry if stuck |
| Rate limited | Wait 60 seconds, then continue |

## Next Steps

- Read the full [SKILL.md](SKILL.md) for detailed tool documentation
- See the [todo app walkthrough](examples/todo-app.md) for a complete example
- Browse the [Bilt docs](https://bilt.me/docs) for advanced features
