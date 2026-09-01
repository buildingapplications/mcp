# Bilt MCP Server

Connect an MCP-compatible AI client to Bilt to create and manage mobile app projects.

## Connection details

| Setting | Value |
| --- | --- |
| URL | `https://mcp.bilt.me/mcp` |
| Transport | Streamable HTTP |
| Authentication | `Authorization: Bearer <Bilt API token>` |

Create an API token at [Bilt API keys](https://bilt.me/settings/api-keys). The legacy `/mcp/sse` endpoint is no longer available.

## Client setup

### Claude Desktop

Claude Desktop launches local MCP commands from `claude_desktop_config.json`. Use [`mcp-remote`](https://github.com/punkpeye/mcp-remote) to bridge stdio to Bilt and attach the API-key header:

```json
{
  "mcpServers": {
    "bilt": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://mcp.bilt.me/mcp",
        "--transport",
        "http-only",
        "--body-timeout",
        "0",
        "--header",
        "Authorization:${BILT_AUTH_HEADER}"
      ],
      "env": {
        "BILT_AUTH_HEADER": "Bearer bilt_live_YOUR_TOKEN_HERE"
      }
    }
  }
}
```

Keep `Authorization:${BILT_AUTH_HEADER}` as one argument, without a space after the colon. On Windows, if Claude resolves `npx` through a broken `Program Files` path, set `command` to the absolute `npx.cmd` path or its 8.3 short-path equivalent.

Do not add Bilt through Claude's custom connector UI unless it supports a static Authorization header. Without that header, it may attempt OAuth, which Bilt does not currently support.

### Cursor

Add this to `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "bilt": {
      "url": "https://mcp.bilt.me/mcp",
      "headers": {
        "Authorization": "Bearer bilt_live_YOUR_TOKEN_HERE"
      }
    }
  }
}
```

### OpenClaw

Add this to `~/.openclaw/openclaw.json`:

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

Restart the OpenClaw gateway after changing the configuration.

For another client, use native Streamable HTTP only if the client supports custom Authorization headers. Otherwise, use the same `mcp-remote` stdio bridge shown for Claude Desktop. Client configuration field names are not universal.

## Available tools

Bilt exposes seven tools:

| Tool | Purpose |
| --- | --- |
| `bilt_list_projects` | List projects owned by the authenticated user |
| `bilt_get_project` | Get one project by `projectId` |
| `bilt_create_project` | Create a project from a `name` and optional `description` |
| `bilt_get_session` | Get session and workflow state for a `projectId` |
| `bilt_send_message` | Send build instructions or answer paused workflow questions |
| `bilt_cancel_workflow` | Cancel the active workflow for a `projectId` |
| `bilt_get_messages` | Get conversation history for a `projectId` |

All public input fields use camelCase. `bilt_send_message` accepts a `projectId` and either a `message` or structured question `answers`. Question answers are passed as a JSON string and require the paused response's `workflowId`:

```json
{
  "projectId": "project-id",
  "workflowId": "workflow-id",
  "answers": "[{\"questionIndex\":0,\"selectedOptions\":[\"Option A\"]}]"
}
```

There is no separate resume tool. If a workflow pauses for questions, answer them with `bilt_send_message`. If it pauses for secrets or a Supabase connection, open the returned `projectUrl`, complete the requested action there, and the workflow will resume automatically.

Queued workflows count as active. Before sending unrelated work, call `bilt_get_session` and wait until `hasActiveWorkflows` is false.

## Raw HTTP

MCP clients must send `Accept: application/json, text/event-stream`:

```bash
curl -N https://mcp.bilt.me/mcp \
  -H 'Authorization: Bearer bilt_live_YOUR_TOKEN_HERE' \
  -H 'Accept: application/json, text/event-stream' \
  -H 'Content-Type: application/json' \
  -H 'Mcp-Protocol-Version: 2025-03-26' \
  --data '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2025-03-26","capabilities":{},"clientInfo":{"name":"curl","version":"1.0.0"}}}'
```

Use an MCP client for normal operation; the raw request is only a transport check.

## Migration from legacy SSE

Existing configurations must:

1. Change `https://mcp.bilt.me/mcp/sse` to `https://mcp.bilt.me/mcp`.
2. Select Streamable HTTP and disable SSE fallback.
3. Keep the Bearer token in the Authorization header.
4. Update stale tool calls to the seven-tool camelCase contract above.

## Documentation and support

- [Bilt MCP documentation](https://bilt.me/docs/quickstart-mcp)
- [Integration examples](examples/)
- [GitHub issues](https://github.com/buildingapplications/mcp/issues)
- [support@bilt.me](mailto:support@bilt.me)

The hosted Bilt MCP server is proprietary. This repository contains documentation, examples, and agent skill metadata under the terms in [LICENSE](LICENSE).
