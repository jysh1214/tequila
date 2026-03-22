# Action: Plan Task

When planning a proposed task, follow these steps:

1. Locate the folder `.tequila/tasks/{task-id}/` with the specified `task-id`.
   - Identify the `task-id` based on the user's description if not provided accurately, and confirm it with the user.
   - If the folder does not exist, inform the user and suggest proposing a new task instead, then abort the current action.
   - If the task is not in the `PROPOSED` state, inform the user and abort the current action.
2. **Brainstorm** before committing to a plan. Review the proposal and identify:
   - **Ambiguities**: anything unclear or open to interpretation in the requirements.
   - **Edge cases**: scenarios the requirements don't address.
   - **Approach options**: if multiple implementation strategies exist, list them with trade-offs.
   - If the requirements are straightforward (single obvious approach, no ambiguity, no meaningful edge cases), note this briefly and proceed.
   - Otherwise, present findings to the user and ask for decisions before continuing. Do not guess — get alignment first.
   - Ask the user whether to include test subtasks. If yes, derive test cases from the proposal's Given-When-Then acceptance criterion and include them as subtasks in the plan.
3. Create the following inside the task folder:
   - `subtasks.md`: using the [TASKS-SUBTASKS.md](../../assets/templates/TASKS-SUBTASKS.md) template. This is the task-level overview listing all subtasks.
   - `subtasks/` directory: containing one subdirectory per subtask, named `{index}-{subtask-name}`, where `{index}` is the 1-based index zero-padded to 3 digits and `{subtask-name}` is kebab-case, verb-led (same convention as task ids). Inside each subdirectory, create a `commit_message` file containing a draft commit message for the subtask based on the planned intent.
4. Fill out `subtasks.md` with the subtask list using task list grammar (e.g., `- [ ]` for pending subtasks).
5. Follow [CREATE-PR.md](./CREATE-PR.md) to create `pr.md` inside the task folder.
6. Update the `state` file to contain the text `PLANNED` only.
7. Summarize the planned task and provide any next steps if applicable.
