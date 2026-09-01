# Bilt Builder

Bilt Builder gives an OpenClaw agent access to Bilt's seven MCP tools for creating and managing mobile app projects.

## Setup

1. Create an API token at [Bilt API keys](https://bilt.me/settings/api-keys).
2. Copy the Streamable HTTP configuration from [QUICKSTART.md](QUICKSTART.md) into `~/.openclaw/openclaw.json`.
3. Restart the OpenClaw gateway.

The MCP server returns a project page at `https://app.bilt.me/agent/{projectId}`. A user may need to open that page when a workflow asks for secrets or a Supabase connection.

Read [SKILL.md](SKILL.md) for the tool contract and workflow rules. See [examples/todo-app.md](examples/todo-app.md) for a complete example.

## Support

- [Bilt MCP documentation](https://docs.bilt.me/quickstart-mcp)
- [GitHub issues](https://github.com/buildingapplications/mcp/issues)
- [support@bilt.me](mailto:support@bilt.me)
