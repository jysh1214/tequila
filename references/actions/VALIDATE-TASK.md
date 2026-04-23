# Action: Validate Task

When validating an implemented task, follow these steps:

1. Locate the folder `.tequila/tasks/{task-id}/` with the specified `task-id`.
   - Identify the `task-id` based on the user's description if not provided accurately, and confirm it with the user.
   - If the folder does not exist, inform the user and abort the current action.
   - If the task is not in the `IMPLEMENTED` state, inform the user and abort the current action.
2. Update the `state` file to contain the text `VALIDATING` only.
3. Read the task files (`proposal.md`, `subtasks.md`, subtask `commit_message` files, etc.) to understand what the task is supposed to achieve.
4. Determine the validation method:
   - If the user has provided a clear, unambiguous, and reproducible validation method, use it.
   - If the user has not provided a validation method, or if it is ambiguous, ask the user to specify one. Suggest concrete options based on the task (e.g., running specific tests, verifying specific behavior, checking specific outputs) so the user can choose or refine.
   - The validation method **MUST** be clear (well-defined pass/fail criteria), unambiguous (one interpretation only), and reproducible (anyone can follow it and get the same result).
   - Verification activities that compile an external artifact, run an end-to-end test, or collect empirical measurements belong here — **not** as subtasks (per [PRINCIPLE.md](../PRINCIPLE.md)'s "subtask must produce a code change" rule). Record the exact command, the expected outcome, and the observed result in `validation.md`. If PLAN-TASK surfaced a "verify", "benchmark", or "audit" subtask, fold it into this step and drop the subtask.
5. Check if `validation.md` already exists inside the task folder.
   - If it exists, update it with the new validation method and results.
   - Otherwise, create the file using the [VALIDATION.md](../../assets/templates/VALIDATION.md) template.
6. Execute or guide the user through the validation steps and record the outcome:
   - If the validation passes, set the `state` file in every subtask directory under `subtasks/` to `APPROVED`. Then set the status to `PASS` in `validation.md` and update the task `state` file to contain the text `ARCHIVED` only.
   - If the validation fails, set the `state` file in every subtask directory under `subtasks/` to `REJECTED`. Then set the status to `FAIL` in `validation.md` and automatically trigger the [DOCUMENT-ISSUES.md](./DOCUMENT-ISSUES.md) action to record what went wrong (this will set the task state to `FAILED`).
7. Summarize the validation result and provide next steps if applicable.
