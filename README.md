# Bilt MCP Server

<div align="center">

![Bilt Logo](https://bilt.me/logo.png)

**Enable AI agents to autonomously build and deploy full-stack mobile applications**

[![MCP Compatible](https://img.shields.io/badge/MCP-Compatible-brightgreen)](https://modelcontextprotocol.io)
[![License](https://img.shields.io/badge/License-Proprietary-blue)](LICENSE)
[![Documentation](https://img.shields.io/badge/Docs-bilt.me%2Fdocs-purple)](https://bilt.me/docs)

[Documentation](https://bilt.me/docs) • [Sign Up](https://bilt.me/sign-up) • [Examples](examples/) • [Support](#support)

</div>

---

## What is Bilt MCP Server?

Bilt's Model Context Protocol (MCP) server gives AI agents **8 powerful tools** to autonomously create, build, and deploy production-ready mobile applications through natural language.

### Key Features

- 🤖 **Agent-First Design** - Built specifically for AI autonomy
- 🚀 **Zero Setup** - Remote SSE server, no local installation
- 🏗️ **Complete Lifecycle** - From project creation to production deployment
- ⚡ **Real-Time Updates** - SSE transport for live progress tracking
- 🔒 **Secure** - Bearer token authentication, rate limiting
- 📱 **Mobile Focus** - Optimized for mobile app development

---

## Quick Start

### 1. Get API Token

Sign up at [bilt.me/sign-up](https://bilt.me/sign-up) and generate an API key.

### 2. Configure Your MCP Client

<details>
<summary><b>Claude Desktop</b></summary>

Edit `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS):

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

Restart Claude Desktop.

</details>

<details>
<summary><b>OpenClaw</b></summary>

Edit `~/.openclaw/openclaw.json`:

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

Restart: `openclaw gateway restart`

</details>

<details>
<summary><b>Cursor IDE</b></summary>

Create `.cursor/mcp_config.json` in your project:

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

Reload Cursor window: `Cmd/Ctrl+Shift+P` → "Reload Window"

</details>

### 3. Test It

Ask your AI agent:

```
"Use Bilt to create a todo app with authentication"
```

Watch it autonomously create, build, and deploy a production app! 🎉

---

## Available Tools

The Bilt MCP server provides **8 tools** for complete application lifecycle management:

| Tool | Purpose |
|------|---------|
| `bilt_list_projects` | List all projects for authenticated user |
| `bilt_get_project` | Get detailed project information |
| `bilt_create_project` | Create a new project |
| `bilt_get_session` | Get current workflow session status |
| `bilt_send_message` | Execute build/deploy instructions |
| `bilt_resume_workflow` | Resume paused workflows |
| `bilt_cancel_workflow` | Cancel running workflows |
| `bilt_get_messages` | Retrieve workflow message history |

[📖 Full API Reference](https://bilt.me/docs/api-reference/overview)

---

## Example Workflow

```
Agent: bilt_create_project("todo-app")
→ Returns: { id: "proj_abc123" }

Agent: bilt_get_session()
→ Returns: { session_id: "sess_xyz789" }

Agent: bilt_send_message(
  session_id: "sess_xyz789",
  message: "Create a todo list with add, delete, and complete features"
)
→ Builds app...

Agent: bilt_send_message(
  session_id: "sess_xyz789",
  message: "Deploy to production"
)
→ Returns: { url: "https://todo-app-abc123.bilt.app" }
```

[📚 More Examples](examples/)

---

## Documentation

- **[Introduction](https://bilt.me/docs/introduction)** - What is Bilt MCP?
- **[Quick Start](https://bilt.me/docs/quickstart)** - 5-minute setup guide
- **[API Reference](https://bilt.me/docs/api-reference/overview)** - Complete tool documentation
- **[Integration Guides](https://bilt.me/docs/integrations/claude-desktop)** - Client-specific setup
- **[Examples](https://bilt.me/docs/examples/create-deploy-app)** - Step-by-step workflows

---

## Why MCP?

Model Context Protocol is the emerging standard for AI tool integration. Benefits:

✅ **Universal Compatibility** - Works with any MCP-compatible client
✅ **Standardized Interface** - No custom integrations needed
✅ **Auto-Discovery** - Agents find and use tools automatically
✅ **Session Management** - Built-in state handling
✅ **Real-Time Updates** - SSE transport for progress tracking

---

## Technical Details

### Connection Details

- **Server URL:** `https://mcp.bilt.me/mcp/sse`
- **Transport:** SSE (Server-Sent Events)
- **Alternative:** HTTP at `https://mcp.bilt.me/mcp`
- **Authentication:** Bearer token (`bilt_live_...`)
- **Rate Limit:** 100 requests/minute

### Requirements

- MCP-compatible client (Claude Desktop, OpenClaw, Cursor, etc.)
- Bilt account with API token
- Internet connection (remote server)

### Supported Platforms

- ✅ macOS
- ✅ Windows
- ✅ Linux
- ✅ Any platform with MCP client support

---

## Use Cases

### 🤖 Autonomous App Development
AI agents build entire applications from scratch - from initial requirements to production deployment.

### ⚡ Rapid Prototyping
Generate working prototypes in minutes for testing ideas and gathering feedback.

### 🔧 Feature Development
Add new features to existing projects through natural language instructions.

### 👥 Multi-Agent Collaboration
Multiple agents work on different aspects of a project simultaneously.

---

## Examples

See the [examples/](examples/) directory for:

- Complete workflow examples
- Multi-step builds
- Error handling patterns
- Advanced use cases

---

## FAQ

<details>
<summary><b>Is this open source?</b></summary>

The Bilt MCP server is a proprietary service. This repository contains documentation and integration examples. The server itself runs on Bilt's infrastructure.

</details>

<details>
<summary><b>How much does it cost?</b></summary>

Bilt offers a free tier for experimentation. See [bilt.me/pricing](https://bilt.me/pricing) for details.

</details>

<details>
<summary><b>Can I self-host?</b></summary>

Currently, Bilt MCP is cloud-only. We may offer self-hosted options for enterprise customers in the future.

</details>

<details>
<summary><b>What kind of apps can I build?</b></summary>

Bilt specializes in mobile applications built with React Native and Expo. Supports iOS and Android.

</details>

<details>
<summary><b>Is my data secure?</b></summary>

Yes. All communication uses HTTPS. API tokens are required for authentication. Your code and data remain private.

</details>

---

## Support

- 📚 **Documentation:** [bilt.me/docs](https://bilt.me/docs)
- 💬 **Discord:** [Join our community](https://discord.gg/3FqNgmSYdZ)

- 📧 **Email:** [support@bilt.me](mailto:support@bilt.me)
- 🐛 **Issues:** [GitHub Issues](https://github.com/buildingapplications/mcp/issues)

---

## Community

- [Awesome MCP Servers](https://github.com/punkpeye/awesome-mcp-servers)
- [Model Context Protocol](https://modelcontextprotocol.io)
- [MCP Discord](https://discord.gg/mcp)
- [r/mcp on Reddit](https://reddit.com/r/mcp)

---

## License

Copyright © 2024-2026 Bilt. All rights reserved.

The Bilt MCP server is proprietary software. This repository contains documentation and examples under the MIT License. See [LICENSE](LICENSE) for details.

---

## Acknowledgments

Built with ❤️ by the [Bilt](https://bilt.me) team.

MCP specification by [Anthropic](https://anthropic.com).

---

<div align="center">

**[Get Started →](https://bilt.me/docs)**

Made with [Model Context Protocol](https://modelcontextprotocol.io)

</div>

