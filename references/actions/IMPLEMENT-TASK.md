# Action: Implement Task

When implementing a planned task, follow these steps:

1. Locate the folder `.tequila/tasks/{task-id}/` with the specified `task-id`.
   - Identify the `task-id` based on the user's description if not provided accurately, and confirm it with the user.
   - If the folder does not exist, inform the user and suggest proposing a new task instead, then abort the current action.
   - If the task is not in the `PLANNED` state, inform the user and abort the current action.
2. Read `subtasks.md` for the overall picture and the subtask `commit_message` files under `subtasks/` to understand the subtasks to be completed.
3. For each subtask directory under `subtasks/`, perform the following:
   - Do the subtask as described in its `commit_message` file and per the user's instructions.
   - If more information or clarification is needed, ask the user, and then amend relevant files if necessary.
   - Mark the subtask as completed in `subtasks.md` using task list grammar (e.g., `- [x]` for completed subtasks).
   - Update the subtask's `commit_message` file with the final commit message describing the actual change.
   - Save a Git patch file as `patch` inside the subtask directory. Generate the patch by diffing the working tree changes introduced by this subtask (e.g., `git diff` for the relevant files).
   - Keep the workspace consistent after each subtask (avoid leaving it half-broken unless the user explicitly accepts that).
   - Repeat this step until all subtasks are completed.
4. Update the `state` file to contain the text `IMPLEMENTED` only.
5. Summarize the implemented task and provide any next steps if applicable.
