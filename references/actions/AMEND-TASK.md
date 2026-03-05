# Action: Amend Task

When amending an existing task, follow these steps:

1. Locate the folder `.tequila/tasks/{task-id}/` with the specified `task-id`.
   - Identify the `task-id` based on the user's description if not provided accurately, and confirm it with the user.
   - If the folder does not exist, inform the user and suggest creating a new task proposal instead, then abort the current action.
2. Identify the artifacts to be amended based on the user's request (e.g., `proposal.md`, `subtasks.md`, subtask directories under `subtasks/`, etc.).
3. Make the necessary amendments to the specified artifacts based on the user's instructions.
   - Repeat this step until all requested amendments are made.
   - Check for consistency across related files if applicable to avoid contradictions.
   - Remove directly instead of marking items as deleted if requested.
4. Check whether the amendments invalidate the current state's entry conditions (see [PRINCIPLE.md](../PRINCIPLE.md) Task Lifecycle). For example:
   - Adding a new subtask directory without a `patch` file to an `IMPLEMENTED` or later task (all subtasks must have patches).
   - If the state is no longer consistent, inform the user and ask whether to roll back the state to the last valid one.
5. Summarize the amendments made to the task and provide any next steps if applicable.
