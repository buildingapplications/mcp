# Bilt Builder Skill

You can build and deploy production mobile applications autonomously using the Bilt MCP server. No local setup, no manual steps, no waiting on humans.

Bilt handles the entire lifecycle: project creation, code generation, building, and deployment. You talk to it through 8 MCP tools. It gives you back live URLs.

## Setup (One-Time)

Add the Bilt MCP server to `~/.openclaw/openclaw.json`:

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

Get your token at [bilt.me/sign-up](https://bilt.me/sign-up). Restart the gateway: `openclaw gateway restart`.

## Available MCP Tools

### Project Management

| Tool | What It Does |
|------|-------------|
| `bilt_create_project(name, description)` | Create a new project. Returns a project ID. |
| `bilt_list_projects()` | List all your projects. |
| `bilt_get_project(project_id)` | Get details for a specific project (status, URLs, metadata). |

### Building & Deploying

| Tool | What It Does |
|------|-------------|
| `bilt_get_session()` | Get the current workflow session. You need this before sending messages. |
| `bilt_send_message(session_id, message)` | Send a natural language instruction to build, modify, or deploy your app. |
| `bilt_get_messages(session_id)` | Retrieve the message history for a session. |

### Workflow Control

| Tool | What It Does |
|------|-------------|
| `bilt_resume_workflow(session_id)` | Resume a paused workflow. |
| `bilt_cancel_workflow(session_id)` | Cancel a running workflow. |

## Standard Workflow

Every app follows the same pattern:

```
1. Create project    bilt_create_project("my-app", "A fitness tracker")
                     -> { id: "proj_abc123" }

2. Get session       bilt_get_session()
                     -> { session_id: "sess_xyz789" }

3. Build             bilt_send_message(sess_id, "Create a workout tracker with...")
                     -> Builds the app (takes 2-4 minutes)

4. Iterate           bilt_send_message(sess_id, "Add a dark mode toggle")
                     -> Updates the app

5. Deploy            bilt_send_message(sess_id, "Deploy to production")
                     -> { url: "https://my-app-abc123.bilt.app" }
```

That's it. Five steps from nothing to a live app with a shareable URL.

## Example Prompts

These are prompts you can give directly to `bilt_send_message`:

**Simple:**
- "Create a todo list with add, delete, and complete functionality"
- "Build a weather app that shows the 5-day forecast"
- "Make a simple notes app with categories"

**Medium:**
- "Build a recipe app with search, favorites, and a shopping list generator"
- "Create a habit tracker with streaks, daily reminders, and progress charts"
- "Make a flashcard study app with spaced repetition"

**Complex:**
- "Build a social fitness app with workout logging, friend challenges, and leaderboards"
- "Create a personal finance tracker with expense categories, budget alerts, and monthly reports"
- "Build a project management tool with kanban boards, team assignments, and deadline tracking"

## Tips for Agents

**Always get a session before sending messages.** Call `bilt_get_session()` after creating a project. You need the `session_id` for all subsequent calls.

**Be specific in your messages.** Instead of "make it look better", say "use a blue and white color scheme with rounded cards and subtle shadows". Bilt responds better to concrete instructions.

**Iterate in small steps.** Build the core features first, then add polish. Send separate messages for each feature rather than one giant prompt.

**Check the build status.** If a build seems stuck, use `bilt_get_messages()` to check progress. Use `bilt_resume_workflow()` if it's paused, or `bilt_cancel_workflow()` and start fresh if something went wrong.

**Deploy when satisfied.** You can deploy at any point. The URL is permanent and updates when you redeploy.

**Don't create duplicate projects.** Use `bilt_list_projects()` to check if the project already exists before creating a new one.

## Troubleshooting

| Issue | Fix |
|-------|-----|
| "Authentication failed" | Check your API token. It should start with `bilt_live_`. |
| "Session not found" | Call `bilt_get_session()` again. Sessions can expire. |
| Build hangs | Use `bilt_get_messages()` to check status. Cancel and retry if needed. |
| "Rate limit exceeded" | Wait 60 seconds. The limit is 100 requests/minute. |
| MCP connection error | Verify the server URL: `https://mcp.bilt.me/mcp/sse`. Restart the gateway. |

## Resources

- [Bilt Documentation](https://bilt.me/docs)
- [API Reference](https://bilt.me/docs/api-reference/overview)
- [MCP GitHub Repo](https://github.com/buildingapplications/mcp)
- [Example Projects](https://github.com/buildingapplications)
- [Discord Community](https://discord.gg/3FqNgmSYdZ)
- [Support](mailto:support@bilt.me)
