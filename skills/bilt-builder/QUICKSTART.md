# Bilt Builder quickstart

## Configure OpenClaw

Create an API token at [Bilt API keys](https://bilt.me/settings/api-keys), then add Bilt to `~/.openclaw/openclaw.json`:

```json
{
  "mcp": {
    "servers": {
      "bilt": {
        "url": "https://mcp.bilt.me/mcp",
        "transport": "streamable-http",
        "headers": {
          "Authorization": "Bearer bilt_live_YOUR_TOKEN_HERE"
        }
      }
    }
  }
}
```

Restart the gateway:

```bash
openclaw gateway restart
```

## Build a first project

Ask the agent:

```text
Use Bilt to create a todo app with task creation, completion, and deletion.
Use a clean mobile layout and give me the Bilt project link when it is ready.
```

The agent should list existing projects, create one if necessary, check its session, and call `bilt_send_message` with the new `projectId`.

## Common issues

| Problem | Resolution |
| --- | --- |
| Tools are missing | Restart the OpenClaw gateway and verify the server is under `mcp.servers` |
| Authentication fails | Generate a current token and keep the `Bearer ` prefix |
| Connection uses SSE | Set `transport` to `streamable-http` and use `/mcp`, not `/mcp/sse` |
| Workflow is queued | Call `bilt_get_session` and wait until `hasActiveWorkflows` is false |
| Workflow asks questions | Answer through `bilt_send_message` with `workflowId` and JSON-encoded `answers` |
| Workflow asks for secrets | Open the returned `projectUrl` and enter them there |

See [SKILL.md](SKILL.md) for the complete contract.
