# Example: Building a Todo App

A complete walkthrough of an agent building and deploying a todo app using the Bilt Builder skill.

## User Prompt

```
Build me a todo app with Bilt. I want it to have:
- Add tasks with a title and optional due date
- Organize tasks by category (Work, Personal, Shopping)
- Swipe to delete, tap to mark complete
- Show completed tasks in a separate tab
- Clean, minimal design with a blue accent color

Deploy it when it's ready.
```

## Agent Flow

### Step 1: Check for Existing Projects

```
Tool: bilt_list_projects()

Response:
{
  "projects": [
    { "id": "proj_old123", "name": "weather-app", "status": "deployed" }
  ]
}
```

No existing todo app. Proceed with creation.

### Step 2: Create the Project

```
Tool: bilt_create_project(
  name: "todo-app",
  description: "Todo list with categories, due dates, and swipe actions"
)

Response:
{
  "id": "proj_td456",
  "name": "todo-app",
  "status": "created",
  "created_at": "2026-02-16T10:00:00Z"
}
```

### Step 3: Get a Session

```
Tool: bilt_get_session()

Response:
{
  "session_id": "sess_abc789",
  "project_id": "proj_td456",
  "status": "active"
}
```

### Step 4: Build the App

```
Tool: bilt_send_message(
  session_id: "sess_abc789",
  message: "Create a todo list app with the following features:
    1. Add tasks with a title and optional due date
    2. Three categories: Work, Personal, Shopping - shown as tabs
    3. Swipe left to delete a task
    4. Tap a task to toggle its completed status
    5. Separate 'Completed' tab showing finished tasks
    6. Clean minimal design with #2563EB as the primary accent color
    7. Use SF-style rounded cards for each task
    8. Show due dates in a subtle gray below the task title
    9. Empty states with helpful text when no tasks exist"
)

Response:
{
  "status": "building",
  "message": "Building your todo app... This will take 2-3 minutes."
}
```

Agent waits for the build to complete, checking status periodically.

### Step 5: Check Build Progress

```
Tool: bilt_get_messages(session_id: "sess_abc789")

Response:
{
  "messages": [
    {
      "role": "assistant",
      "content": "I've created your todo app with all the requested features. Here's what I built:\n\n- Task input with title and date picker\n- Category tabs: Work, Personal, Shopping, Completed\n- Swipe-to-delete with red background indicator\n- Tap-to-complete with strikethrough animation\n- Blue (#2563EB) accent throughout\n- Rounded card design\n- Empty state illustrations\n\nThe app is ready for review.",
      "status": "completed"
    }
  ]
}
```

### Step 6: Iterate (Optional)

The agent decides the design could use a small improvement.

```
Tool: bilt_send_message(
  session_id: "sess_abc789",
  message: "Add a task count badge on each category tab showing how many active tasks are in that category. Also add a subtle shadow to the task cards."
)

Response:
{
  "status": "building",
  "message": "Updating the app with badges and shadows..."
}
```

### Step 7: Deploy

```
Tool: bilt_send_message(
  session_id: "sess_abc789",
  message: "Deploy to production"
)

Response:
{
  "status": "deployed",
  "url": "https://todo-app-td456.bilt.app",
  "message": "Your app is live!"
}
```

### Step 8: Confirm to User

Agent responds:

> Your todo app is live at **https://todo-app-td456.bilt.app**
>
> Here's what I built:
> - Add tasks with titles and optional due dates
> - Three category tabs (Work, Personal, Shopping) with task count badges
> - Swipe left to delete, tap to mark complete
> - Completed tab for finished tasks
> - Clean minimal design with blue accent and card shadows
>
> Open the link on your phone or desktop to try it out.

## Outcome Metrics

| Metric | Value |
|--------|-------|
| Total time | ~4 minutes |
| MCP tool calls | 7 |
| Iterations | 1 (added badges and shadows) |
| Result | Live deployed app with shareable URL |
| Human intervention required | None |

## Key Takeaways

1. **Check first** - The agent checked for existing projects before creating a new one.
2. **Be specific** - The build message included concrete details (colors, layout, interactions), which produced a better result.
3. **Iterate separately** - Rather than cramming everything into one message, the agent sent a follow-up for polish.
4. **Deploy last** - Build and iterate first, deploy when satisfied.
5. **Report the URL** - Always give the user their live URL and a summary of what was built.
