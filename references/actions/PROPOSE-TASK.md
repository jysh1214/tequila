# Action: Propose Task

When proposing a new task, follow these steps:

1. Ensure `.tequila/tasks/` exists; create it if missing.
2. Create a new folder `.tequila/tasks/{task-id}/` with a unique `task-id` that describes the task.
   - Scan `.tequila/tasks/` for existing task directories and extract the highest numeric prefix (the leading 4-digit number) from existing task ids. Assign the next sequential index, zero-padded to 4 digits. If no tasks exist, start at `0001`.
   - Construct the task id as `{index}-{descriptive-part}` (e.g., `0001-add-multi-factor-auth`).
   - Propose a `task-id` based on the user's description if not provided, and confirm it with the user.
   - If the folder already exists, inform the user, suggest amending the existing task instead, and abort the current action.
3. Inside the new folder, create the following required files:
   - `state`: containing the text `PROPOSED` only.
   - `proposal.md`: using the [TASKS-PROPOSAL.md](../../assets/templates/TASKS-PROPOSAL.md) template.
   - `ticket`: (optional) if the user provides a Jira ticket index, create this file containing the ticket index only (e.g., `PROJ-123`).
4. Fill out the `proposal.md` file with the relevant information about the task. The acceptance criterion must be a single Given-When-Then statement that defines what success looks like. If the task requires multiple GWT statements, it should be split into multiple tasks.
5. Optionally, add any additional files related to the task, such as diagrams or supporting documents only if requested by the user.
6. Update `.tequila/work` with the new `task-id` to set it as the currently focused task.
7. Summarize the created task proposal and provide any next steps if applicable.
