# Action: Create PR

When preparing a PR for an implemented task, follow these steps:

1. Locate the folder `.tequila/tasks/{task-id}/` with the specified `task-id`.
   - Identify the `task-id` based on the user's description if not provided accurately, and confirm it with the user.
   - If the folder does not exist, inform the user and suggest proposing a new task instead, then abort the current action.
   - If the task is not in the `IMPLEMENTED` state, inform the user and abort the current action.
2. Gather context for the PR description:
   - Read `proposal.md` for motivation and summary.
   - Read `subtasks.md` for the overview and subtask `commit_message` files under `subtasks/` for detail.
   - Review committed changes relevant to this task (e.g., git log and diff).
3. Create `pr.md` inside the task folder using the [CREATE-PR.md](../../assets/templates/CREATE-PR.md) template, filled with the gathered context.
4. Summarize the prepared PR and provide any next steps if applicable.
