# Action: Document Issues

When documenting issues found in a task, follow these steps:

1. Locate the folder `.tequila/tasks/{task-id}/` with the specified `task-id`.
   - Identify the `task-id` based on the user's description if not provided accurately, and confirm it with the user.
   - If the folder does not exist, inform the user and abort the current action.
   - If the task is in the `ARCHIVED` state, inform the user that archived tasks cannot have issues documented, and abort the current action.
2. Read the relevant task files (`proposal.md`, `subtasks.md`, subtask `commit_message` files under `subtasks/`, etc.) and the codebase to understand the task and identify the issues.
3. Check if `issues.md` already exists inside the task folder.
   - If it exists, append the new issues to the existing file.
   - Otherwise, create the file using the [DOCUMENT-ISSUES.md](../../assets/templates/DOCUMENT-ISSUES.md) template.
4. For each issue identified or described by the user, document it with:
   - A clear issue title and description.
   - The root cause explaining why the issue exists.
   - A bullet list of suggested fixes describing possible approaches to resolve it.
5. Update the `state` file to contain the text `FAILED` only.
6. Summarize the documented issues and suggest next steps if applicable (e.g., amending the task, re-planning, re-implementing subtasks).
