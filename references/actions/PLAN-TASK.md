# Action: Plan Task

When planning a proposed task, follow these steps:

1. Locate the folder `.tequila/tasks/{task-id}/` with the specified `task-id`.
   - Identify the `task-id` based on the user's description if not provided accurately, and confirm it with the user.
   - If the folder does not exist, inform the user and suggest proposing a new task instead, then abort the current action.
   - If the task is not in the `PROPOSED` state, inform the user and abort the current action.
2. Create the following inside the task folder:
   - `subtasks.md`: using the [TASKS-SUBTASKS.md](../../assets/templates/TASKS-SUBTASKS.md) template. This is the task-level overview listing all subtasks.
   - `subtasks/` directory: containing one subdirectory per subtask, named `{index}-{subtask-name}`, where `{index}` is the 1-based index zero-padded to 3 digits and `{subtask-name}` is kebab-case, verb-led (same convention as task ids). Inside each subdirectory, create a `commit_message` file containing a draft commit message for the subtask based on the planned intent.
3. Fill out `subtasks.md` with the subtask list using task list grammar (e.g., `- [ ]` for pending subtasks).
4. Update the `state` file to contain the text `PLANNED` only.
5. Summarize the planned task and provide any next steps if applicable.
