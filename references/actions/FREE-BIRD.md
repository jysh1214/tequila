# Action: Free Bird

Free Bird mode runs the full Tequila flywheel automatically for a given task: plan → implement → validate → archive. On validation failure it documents issues, amends, and retries until the task passes or the reincarnation counter reaches zero.

## Required Inputs

- `{task-id}`: The task to run. Must be in the `PROPOSED` state with a valid `proposal.md`.
- `validation: {description}`: A clear, unambiguous, and reproducible description of how to validate the task. This is supplied upfront so validation can run without prompting.

## Procedure

1. Locate the folder `.tequila/tasks/{task-id}/`.
   - If the folder does not exist, inform the user and abort.
   - If the task is not in the `PROPOSED` state, inform the user and abort.
   - Confirm that `proposal.md` exists and is well-formed.
   - Update `.tequila/work` to `{task-id}` so the focused task stays in sync.
2. Create the file `.tequila/tasks/{task-id}/reincarnation` containing `15` (the default retry budget). If the file already exists, use its current value.
3. **Plan** — Follow [PLAN-TASK.md](./PLAN-TASK.md) to create `subtasks.md` (and optionally `design.md`) and move the task to `PLANNED`.
4. **Implement** — Follow [IMPLEMENT-TASK.md](./IMPLEMENT-TASK.md) to complete all subtasks and move the task to `IMPLEMENTED`.
5. **Validate** — Follow [VALIDATE-TASK.md](./VALIDATE-TASK.md) using the validation description provided by the user.
   - If validation **passes**, proceed to step 6.
   - If validation **fails**, proceed to step 7.
6. **Archive** — Follow [ARCHIVE-TASK.md](./ARCHIVE-TASK.md). Set the task state to `ARCHIVED`. The flywheel is complete — summarize the result and stop.
7. **Amend loop** — Validation failed.
   a. Read the current value from `.tequila/tasks/{task-id}/reincarnation` and decrement it by 1. Write the new value back.
   b. If the value has reached `0`, leave the task in `FAILED` state, summarize the unresolved issues to the user, and stop. This indicates the current direction or approach is likely wrong.
   c. Follow [DOCUMENT-ISSUES.md](./DOCUMENT-ISSUES.md) to record what went wrong in this iteration. Issues are appended to the existing `issues.md`, accumulating context across retries.
   d. Follow [AMEND-TASK.md](./AMEND-TASK.md) to address the documented issues — amend subtasks, design, or implementation as needed.
   e. Set the task state back to `PLANNED`.
   f. Return to step 4 (re-implement and re-validate).
