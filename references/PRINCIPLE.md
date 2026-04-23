# Principle

## Key Conventions

- Favor straightforward, minimal implementations first and add complexity only when it is requested or clearly required.
- Keep tasks tightly scoped to the requested outcome. Avoid scope creep.
- Tasks are the primary unit of work. Each task represents a coherent piece of work that maps to a Git PR.
- Subtasks are the individual steps within a task. Each subtask maps to a Git commit. Changes are not committed during implementation — after archiving, the user is asked whether to commit, and the diff is split into subtask-aligned commits.
- A subtask must produce a code change. Pure verification, benchmarking, audit, or observation activities do **not** belong as subtasks — their outcomes live in `validation.md`. Rule of thumb: if the subtask's `patch` file would be empty, it's a validation step, not a subtask.
- Each subtask should address one coherent concern. Symptoms a subtask is too large:
  - The `commit_message` body lists 2+ bullet points for unrelated changes.
  - The patch mixes new source code with new tests, or touches multiple modules.
  - You need the word "also" or "and" to describe it.

  Split along concern boundaries — typically: implementation, tests (possibly one subtask per test category), and docs. A 50-line subtask is fine; prefer smaller commits over larger ones.
- A Jira ticket link is optional. Multiple tasks can reference the same ticket.

## Folder Structure

All tequila-managed artifacts are organized in the `.tequila/` folder at the project root. The key folder under it is `tasks/`.

- `.tequila/work`: (required) Contains the task-id of the currently focused task (e.g., `0001-add-multi-factor-auth`). This file tracks which task is actively being worked on. Actions should default to operating on this task.

### Tasks

The folder `.tequila/tasks/` contains the tasks that describe units of work to be done.

Each task is stored in its own subfolder named after its unique task id (`.tequila/tasks/{task-id}/`).
Task ids start with a 4-digit zero-padded auto-incrementing index prefix, followed by a kebab-case, verb-led descriptive part, e.g., `0001-add-multi-factor-auth`, `0002-improve-payment-latency`, or `0003-add-project-dashboard`. Prefer verb prefixes like `add-`, `remove-`, `update-`, `improve-`, *etc.*, to indicate the action being proposed. The index is determined by scanning existing task directories under `.tequila/tasks/`, finding the highest numeric prefix, and incrementing by 1. If no tasks exist, start at `0001`.
The corresponding task names are of the same wording (without the index prefix) but in Title Case, e.g., `Add Multi-Factor Auth`, `Improve Payment Latency`, or `Add Project Dashboard`.

- `.tequila/tasks/{task-id}/state`: (required)
  The state document for the task `{task-id}`, containing its current state only.
  Possible states are literally `PROPOSED`, `PLANNED`, `IMPLEMENTED`, `VALIDATING`, `FAILED`, and `ARCHIVED`.
  See policies in the **Task Lifecycle** section of this document.
- `.tequila/tasks/{task-id}/ticket`: (optional)
  The ticket file for the task `{task-id}`, containing the Jira ticket index only (e.g., `PROJ-123`).
  Created when the user provides a Jira ticket index during the proposal.
- `.tequila/tasks/{task-id}/proposal.md`: (required, follows [TASKS-PROPOSAL.md](../assets/templates/TASKS-PROPOSAL.md))
  The main proposal document for the task `{task-id}`, outlining the motivation and a single Given-When-Then acceptance criterion. If multiple GWT statements are needed, split into multiple tasks.
- `.tequila/tasks/{task-id}/subtasks.md`: (required after `PLANNED`, follows [TASKS-SUBTASKS.md](../assets/templates/TASKS-SUBTASKS.md))
  The subtask overview for the task `{task-id}`, listing all subtasks at a glance.
  Use task list grammar (e.g., `- [ ]` for pending subtasks and `- [x]` for completed subtasks) to track progress.
- `.tequila/tasks/{task-id}/subtasks/`: (required after `PLANNED`)
  Contains one subdirectory per subtask. Each subdirectory is named `{index}-{subtask-name}`, where `{index}` is the 1-based index of the subtask zero-padded to 3 digits and `{subtask-name}` follows the same kebab-case, verb-led convention as task ids (e.g., `001-add-login-endpoint`).
  Each subtask directory contains:
  - `commit_message`: The commit message for this subtask. Created during planning as a draft based on the planned intent; updated during implementation to describe the actual change. Used directly as the Git commit message when committing. Format: a short summary line (under 72 characters), followed by a blank line, then optional body text with context, rationale, or details.
  - `patch`: The Git patch file capturing the diff produced by this subtask, enabling reviewability, reproducibility (`git apply`), and natural commit mapping at archive time.
  - `state`: The post-implementation review state, containing either `APPROVED` or `REJECTED` only.
- `.tequila/tasks/{task-id}/validation.md`: (required after `IMPLEMENTED`, follows [VALIDATION.md](../assets/templates/VALIDATION.md))
  The validation document for the task `{task-id}`.
  This document describes how to validate the task, the expected outcome, and the result (PASS or FAIL).
  The validation method must be clear, unambiguous, and reproducible.
- `.tequila/tasks/{task-id}/reincarnation`: (optional)
  The reincarnation counter for the task `{task-id}`, containing a single integer.
  Used by Free Bird mode to track the remaining retry budget. Initialized to `15` by default, decremented after each amend loop iteration, and stops Free Bird when it reaches `0`.
- `.tequila/tasks/{task-id}/issues.md`: (required if `FAILED`, follows [DOCUMENT-ISSUES.md](../assets/templates/DOCUMENT-ISSUES.md))
  The issues document for the task `{task-id}`.
  This document records the issues found, their root causes, and suggested fixes.
- `.tequila/tasks/{task-id}/pr.md`: (required after `PLANNED`, follows [PR.md](../assets/templates/PR.md))
  The pull request document for the task `{task-id}`.
  This document summarizes the changes for the Git PR, generated from the proposal, subtasks, design, and commit history.
- `.tequila/tasks/{task-id}/*`: (optional)
  Any additional files related to the task `{task-id}`, such as diagrams or supporting documents.

## Task Lifecycle

Each task goes through a defined lifecycle with specific states. The state must be included in the `state` file within each task folder. There are two lifecycle tracks:

### Standard Track

The possible states are: `PROPOSED`, `PLANNED`, `IMPLEMENTED`, `VALIDATING`, `FAILED`, and `ARCHIVED`. The normal progression is `PROPOSED → PLANNED → IMPLEMENTED → VALIDATING → ARCHIVED`, and a task cannot skip any state in the forward direction. If validation fails, the task enters `FAILED` and issues are documented; once resolved, the task can be moved back to a prior active state for retry.

### Merge/Rebase Track

Used for conflict resolution during merge or rebase operations. The possible states are: `RESOLVING`, `VALIDATING`, `FAILED`, `ARCHIVED`, and `ABORTED`. The normal progression is `RESOLVING → VALIDATING → ARCHIVED`. If validation fails, the task enters `FAILED`; once resolved, it can return to `RESOLVING`. If the user abandons the operation, the task enters `ABORTED`.

### Proposed

In this state, the state file contains the text `PROPOSED` only.

The state indicates that the task has been proposed and is ready for planning.
The task folder must contain at least the `state` and `proposal.md` files in this state.

By reading the `proposal.md` file, stakeholders can understand the motivation and goals. Then, designated engineers can plan the implementation and create `subtasks.md` and the `subtasks/` directory accordingly to move the task to the next state of `PLANNED`.

### Planned

In this state, the state file contains the text `PLANNED` only.

The state indicates that the task has been planned with a defined set of subtasks and is ready for implementation.
The task folder must contain `subtasks.md`, the `subtasks/` directory (with at least one subtask subdirectory, each containing a `commit_message` file), and `pr.md` in this state, in addition to the files required in the `PROPOSED` state.

By reading `subtasks.md` for the whole picture and the subtask `commit_message` files for detail, engineers can understand the specific subtasks to be completed. Then, designated engineers can proceed with the implementation to move the task to the next state of `IMPLEMENTED`.

### Implemented

In this state, the state file contains the text `IMPLEMENTED` only.

The state indicates that the task has been implemented and is ready for validation.
All subtasks in `subtasks.md` must be marked completed (`- [x]`) and all subtask directories under `subtasks/` must contain a `patch` file in this state.

By reviewing the implementation, designated engineers can proceed with validation to move the task to the next state of `VALIDATING`.

### Validating

In this state, the state file contains the text `VALIDATING` only.

The state indicates that the task is undergoing validation.
The task folder must contain the `validation.md` file documenting the validation method, steps, expected outcome, and result.

If validation passes, the task moves directly to `ARCHIVED`. If validation fails, the DOCUMENT-ISSUES action is triggered automatically, which moves the task to `FAILED`.

### Failed

In this state, the state file contains the text `FAILED` only.

The state indicates that validation has failed and issues have been documented.
The task folder must contain the `issues.md` file documenting the issues, their root causes, and suggested fixes.

A `FAILED` task can be moved back to a prior active state (e.g., `PLANNED` or `IMPLEMENTED`) once the issues have been resolved and the task has been amended accordingly.

### Archived

In this state, the state file contains the text `ARCHIVED` only.

The state indicates that the task has been completed and archived for historical reference.

### Resolving (Merge/Rebase Track)

In this state, the state file contains the text `RESOLVING` only.

The state indicates that the task is actively resolving merge or rebase conflicts. Each conflicting file is a subtask, resolved and reviewed one by one. For rebase, new subtasks may be appended as successive commits reveal additional conflicts.

### Aborted (Merge/Rebase Track)

In this state, the state file contains the text `ABORTED` only.

The state indicates that the merge or rebase operation was abandoned (`git merge --abort` or `git rebase --abort`). The task and its subtasks remain as a record of partial progress.
