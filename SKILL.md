---
name: tequila
description: Skill to manage tasks and subtasks. Use this when working with tequila-managed documents.
license: Apache-2.0
---

# Tequila

## Overview

The tequila system is designed to manage tasks in a structured manner, with a strong focus on engineering workflow. It provides a framework for proposing, planning, implementing, validating, and archiving tasks.

- [PRINCIPLE.md](./references/PRINCIPLE.md) must be read first to understand how tequila works.
- [actions](./references/actions/) folder defines detailed steps for each tequila action.
- [templates](./assets/templates/) folder contains templates for various tequila artifacts and reports.

## Decisions

Based on the user's request, decide which action to take.

- If the user intends to show the current status of tasks,
  follow [SHOW-STATUS.md](./references/actions/SHOW-STATUS.md) action.
- If the user intends to propose a new task,
  follow [PROPOSE-TASK.md](./references/actions/PROPOSE-TASK.md) action.
- If the user intends to amend an existing task,
  follow [AMEND-TASK.md](./references/actions/AMEND-TASK.md) action.
- If the user intends to plan a proposed task,
  follow [PLAN-TASK.md](./references/actions/PLAN-TASK.md) action.
- If the user intends to implement a planned task,
  follow [IMPLEMENT-TASK.md](./references/actions/IMPLEMENT-TASK.md) action.
- If the user intends to archive a pass task,
  follow [ARCHIVE-TASK.md](./references/actions/ARCHIVE-TASK.md) action.
- If the user intends to prepare a PR for an implemented task,
  follow [CREATE-PR.md](./references/actions/CREATE-PR.md) action.
- If the user intends to validate an implemented task,
  follow [VALIDATE-TASK.md](./references/actions/VALIDATE-TASK.md) action.
- If the user intends to document issues found in a task,
  follow [DOCUMENT-ISSUES.md](./references/actions/DOCUMENT-ISSUES.md) action.
- If the user intends to run Free Bird mode for a task (run the full flywheel automatically),
  follow [FREE-BIRD.md](./references/actions/FREE-BIRD.md) action.
- If the user's intent mixes multiple actions,
  break down the intent into individual actions, ask for confirmation, and execute them one by one.
- If the user's intent is ambiguous (e.g., "update the task"),
  ask a clarifying question with possible options to determine the specific action needed.

## Disciplines

- Always ask for clarifications if needed, and ensure the workspace remains consistent after each step. When asking for clarifications, provide concise options to the user if applicable.
- All tequila-managed artifacts that this skill creates/updates in the user's repository **MUST** reside in the `.tequila/` folder at the repository root.
- Do **NOT** create auxiliary documents (README, index, etc.) outside `.tequila/` unless explicitly instructed by the user.
