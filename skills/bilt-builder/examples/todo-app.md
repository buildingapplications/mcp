# Example: build a todo app

This example shows the control flow an agent should follow. IDs and responses are abbreviated.

## 1. Check existing projects

```text
bilt_list_projects()
```

If no matching project exists, create one:

```text
bilt_create_project(
  name: "todo-app",
  description: "A mobile todo list with due dates and categories"
)
```

Retain the returned `id` as `projectId`.

## 2. Check session state

```text
bilt_get_session(projectId: "project-id")
```

If `hasActiveWorkflows` is true, wait before sending unrelated work. This includes queued workflows.

## 3. Send the build request

```text
bilt_send_message(
  projectId: "project-id",
  message: "Build a todo app with task creation, completion, deletion, optional due dates, and Work, Personal, and Shopping categories. Use a clean mobile layout with a blue accent."
)
```

The response may be completed, queued, paused, failed, or timed out.

## 4. Handle a paused question

If the response contains `pauseType: "questions"`, answer with the returned `workflowId`:

```text
bilt_send_message(
  projectId: "project-id",
  workflowId: "workflow-id",
  answers: "[{\"questionIndex\":0,\"selectedOptions\":[\"Use local storage\"]}]"
)
```

If it pauses for secrets or Supabase, give the user the returned `projectUrl` instead of putting credentials in an MCP call.

## 5. Inspect and iterate

```text
bilt_get_messages(projectId: "project-id")

bilt_send_message(
  projectId: "project-id",
  message: "Add a count badge to each category and use subtle shadows on task cards."
)
```

When the work is complete, share the returned `projectUrl`, which has the form `https://app.bilt.me/agent/{projectId}`.
