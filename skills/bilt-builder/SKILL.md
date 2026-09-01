# Bilt Builder skill

Use the Bilt MCP tools to create and manage mobile app projects. Keep project identifiers from tool responses and use the camelCase argument names below.

## Tools

| Tool | Inputs | Use |
| --- | --- | --- |
| `bilt_list_projects` | none | List the authenticated user's projects |
| `bilt_get_project` | `projectId` | Get one project |
| `bilt_create_project` | `name`, optional `description` | Create a project |
| `bilt_get_session` | `projectId` | Get session state, workflows, and `hasActiveWorkflows` |
| `bilt_send_message` | `projectId`, optional `message`, `answers`, `workflowId` | Start work, send follow-up instructions, or answer paused questions |
| `bilt_cancel_workflow` | `projectId` | Cancel the active workflow |
| `bilt_get_messages` | `projectId` | Get the project's conversation history |

`bilt_send_message` requires at least one of `message` or `answers`. For a fresh request, provide `projectId` and `message`.

When a workflow pauses for questions, call `bilt_send_message` again with its `workflowId`. Encode the answers as a JSON string:

```json
{
  "projectId": "project-id",
  "workflowId": "workflow-id",
  "answers": "[{\"questionIndex\":0,\"selectedOptions\":[\"Option A\"]}]"
}
```

There is no `bilt_resume_workflow` tool. Question answers resume through `bilt_send_message`.

## Workflow

1. Call `bilt_list_projects` before creating a project. Reuse a matching project when the user asks to modify existing work.
2. Create a project when needed and retain its `id` as `projectId`.
3. Call `bilt_get_session` with `projectId`.
4. If `hasActiveWorkflows` is true, do not send unrelated work. A `queued` workflow is active; wait and check the session again.
5. Call `bilt_send_message` with a specific instruction.
6. Handle the result by status:
   - `queued`: keep the `workflowId` and check `bilt_get_session` until the queued or running workflow clears.
   - `paused` with `pauseType: questions`: send structured answers through `bilt_send_message`.
   - `paused` with `pauseType: secrets` or `supabase`: share `projectUrl` with the user and ask them to complete the action there. The workflow resumes automatically afterward.
   - `completed`: report the result and `projectUrl`.
   - `error` or `timeout`: report the safe error and inspect `bilt_get_messages` before retrying.
7. Share `projectUrl` with the user. Project links use `https://app.bilt.me/agent/{projectId}`.

## Guidance

- Use camelCase fields exactly: `projectId`, `workflowId`, `questionIndex`, and `selectedOptions`.
- Send concrete product and design requirements. Separate unrelated follow-up changes.
- Do not send secrets through MCP messages or question answers.
- Do not create duplicate projects without checking the project list.
- Do not claim that Bilt pushes progress over a separate persistent SSE connection. Use the returned status, `bilt_get_session`, and `bilt_get_messages`.

## Connection

Bilt is available at `https://mcp.bilt.me/mcp` over Streamable HTTP with `Authorization: Bearer <token>`. The legacy `/mcp/sse` endpoint is unavailable.
