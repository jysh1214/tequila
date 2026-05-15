# Action: Show Subtasks

When showing the subtasks of a task, follow these steps:

1. Locate the folder `.tequila/tasks/{task-id}/` with the specified `task-id`.
   - If `task-id` is not provided, default to the task id in `.tequila/work`.
   - Identify the `task-id` based on the user's description if not provided accurately, and confirm it with the user.
   - If the folder does not exist, inform the user and abort the current action.
   - If the `subtasks/` directory does not exist (i.e., the task has not been planned yet), inform the user that the task has no subtasks and abort.
2. Fill out the [OUTPUTS-SUBTASKS.md](../../assets/templates/OUTPUTS-SUBTASKS.md) template using the subtask directories under `.tequila/tasks/{task-id}/subtasks/`.
3. Present the filled template as the subtasks report.
