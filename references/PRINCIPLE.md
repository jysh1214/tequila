# Principle

## Key Conventions

- Favor straightforward, minimal implementations first and add complexity only when it is requested or clearly required.
- Keep tasks tightly scoped to the requested outcome. Avoid scope creep.
- Tasks are the primary unit of work. Each task represents a coherent piece of work that maps to a Git PR.
- Subtasks are the individual steps within a task. Each subtask maps to a Git commit. Changes are not committed during implementation — after archiving, the user is asked whether to commit, and the diff is split into subtask-aligned commits.
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
  Possible states are literally `PROPOSED`, `PLANNED`, `IMPLEMENTED`, `VALIDATING`, `PASS`, `FAILED`, and `ARCHIVED`.
  See policies in the **Task Lifecycle** section of this document.
- `.tequila/tasks/{task-id}/ticket`: (optional)
  The ticket file for the task `{task-id}`, containing the Jira ticket index only (e.g., `PROJ-123`).
  Created when the user provides a Jira ticket index during the proposal.
- `.tequila/tasks/{task-id}/proposal.md`: (required, follows [TASKS-PROPOSAL.md](../assets/templates/TASKS-PROPOSAL.md))
  The main proposal document for the task `{task-id}`, outlining the motivation, goals, and high-level approach.
- `.tequila/tasks/{task-id}/subtasks.md`: (required after `PLANNED`, follows [TASKS-SUBTASKS.md](../assets/templates/TASKS-SUBTASKS.md))
  The subtask list for the task `{task-id}`.
  Subtasks are actionable items to be completed as part of implementing the task.
  Use task list grammar (e.g., `- [ ]` for pending subtasks and `- [x]` for completed subtasks) to track progress.
- `.tequila/tasks/{task-id}/design.md`: (optional, follows [TASKS-DESIGN.md](../assets/templates/TASKS-DESIGN.md))
  The design document for the task `{task-id}`.
  This document provides a detailed design and rationale for the task.
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
- `.tequila/tasks/{task-id}/*`: (optional)
  Any additional files related to the task `{task-id}`, such as diagrams or supporting documents.

## Task Lifecycle

Each task goes through a defined lifecycle with specific states. The state must be included in the `state` file within each task folder. The possible states are: `PROPOSED`, `PLANNED`, `IMPLEMENTED`, `VALIDATING`, `PASS`, `FAILED`, and `ARCHIVED`. The normal progression is `PROPOSED → PLANNED → IMPLEMENTED → VALIDATING → PASS → ARCHIVED`, and a task cannot skip any state in the forward direction. A task can enter the `FAILED` state from any active state (`PROPOSED`, `PLANNED`, `IMPLEMENTED`, `VALIDATING`, or `PASS`) when issues are found. Once resolved, a `FAILED` task can be moved back to any previous active state depending on the nature of the failure.

### Proposed

In this state, the state file contains the text `PROPOSED` only.

The state indicates that the task has been proposed and is ready for planning.
The task folder must contain at least the `state` and `proposal.md` files in this state.

By reading the `proposal.md` file, stakeholders can understand the motivation and goals. Then, designated engineers can plan the implementation and create the `subtasks.md` and `design.md` files accordingly to move the task to the next state of `PLANNED`.

### Planned

In this state, the state file contains the text `PLANNED` only.

The state indicates that the task has been planned with a defined set of subtasks and is ready for implementation.
The task folder must contain the `subtasks.md` file in this state and optionally the `design.md` file for a more comprehensive approach, in addition to the files required in the `PROPOSED` state.

By reading the `subtasks.md` and `design.md` files, engineers can understand the specific subtasks to be completed and the design approach. Then, designated engineers can proceed with the implementation to move the task to the next state of `IMPLEMENTED`.

### Implemented

In this state, the state file contains the text `IMPLEMENTED` only.

The state indicates that the task has been implemented and is ready for validation.
The task folder must contain the `subtasks.md` file where all subtasks are marked completed in this state. Task list grammar is used for subtasks, e.g., `- [x]` for completed subtasks and `- [ ]` for pending subtasks.

By reviewing the implementation, designated engineers can proceed with validation to move the task to the next state of `VALIDATING`.

### Validating

In this state, the state file contains the text `VALIDATING` only.

The state indicates that the task is undergoing validation.
The task folder must contain the `validation.md` file documenting the validation method, steps, expected outcome, and result.

If validation passes, the task moves to `PASS`. If validation fails, the task moves to `FAILED`.

### Pass

In this state, the state file contains the text `PASS` only.

The state indicates that the task has passed validation and is ready for archiving.
The task folder must contain the `validation.md` file with a `PASS` result.

By confirming the validation result, designated engineers can proceed with archiving to move the task to the final state of `ARCHIVED`.

### Failed

In this state, the state file contains the text `FAILED` only.

The state indicates that issues have been found in the task that need to be addressed before proceeding.
The task folder must contain the `issues.md` file documenting the issues, their root causes, and suggested fixes.

A `FAILED` task can be moved back to its previous state (e.g., `PROPOSED`, `PLANNED`, `IMPLEMENTED`, `VALIDATING`, or `PASS`) once the issues have been resolved and the `issues.md` file has been updated accordingly.

### Archived

In this state, the state file contains the text `ARCHIVED` only.

The state indicates that the task has been completed and archived for historical reference.
