# Action: Archive Task

When archiving an implemented task, follow these steps:

1. Locate the folder `.tequila/tasks/{task-id}/` with the specified `task-id`.
   - Identify the `task-id` based on the user's description if not provided accurately, and confirm it with the user.
   - If the folder does not exist, inform the user and suggest proposing a new task instead, then abort the current action.
   - If the task is not in the `PASS` state, inform the user and abort the current action.
2. Verify that all subtasks in `subtasks.md` are marked as completed.
3. Verify that `validation.md` exists and contains a `PASS` result.
4. Update the `state` file to contain the text `ARCHIVED` only.
5. Summarize the archived task and provide any next steps if applicable.
6. Ask the user whether they want to commit the changes.
   - If the user agrees, create Git commits for the task. Try to split the changes into separate commits that align with the subtasks as much as possible — each commit should correspond to one subtask. When subtask boundaries cannot be cleanly separated (e.g., multiple subtasks modified the same file), combine them into a single commit and note the covered subtasks in the commit message.
   - If the user declines, leave the changes uncommitted.
