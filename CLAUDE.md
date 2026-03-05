# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Tequila is an agent skill (not a traditional application) that manages tasks in a structured way, with a strong focus on engineering workflow. Forked from [Specrate](https://github.com/rickygao/specrate). It is installed into other repositories as a skill under `.claude/skills/tequila/` or `.augment/skills/tequila/`. There is no build system, no tests, and no runtime code — the project consists entirely of Markdown documents that define a skill protocol.

## Repository Structure

- `SKILL.md` — Skill entry point with YAML front matter; defines the decision tree that routes user intent to specific actions
- `references/PRINCIPLE.md` — Core conventions, folder structure specification, and task lifecycle state machine
- `references/actions/` — 10 action guides (SHOW-STATUS, PROPOSE-TASK, PLAN-TASK, IMPLEMENT-TASK, VALIDATE-TASK, ARCHIVE-TASK, AMEND-TASK, CREATE-PR, DOCUMENT-ISSUES, FREE-BIRD)
- `assets/templates/` — 7 Markdown templates for artifacts and reports (proposals, subtasks, PRs, validations, issues, status outputs)

## Key Concepts

- **Tasks** are the primary unit of work, each mapping to a Git PR
- **Subtasks** are individual steps within a task, each mapping to a Git commit. Each subtask directory contains a `commit_message` (used directly as the Git commit message), a `patch`, and a `state` file
- **Jira tickets** are optional metadata — multiple tasks can share the same ticket
- Tasks go through a lifecycle: `PROPOSED → PLANNED → IMPLEMENTED → VALIDATING → ARCHIVED` (validation failure automatically triggers DOCUMENT-ISSUES, setting the task to `FAILED`; once resolved, a failed task falls back to a prior active state)
- `pr.md` is created during the PLAN phase, not after archiving
- Validation is required before archiving; validation pass transitions directly to `ARCHIVED`

## Conventions

- Task IDs use a 4-digit auto-incrementing prefix followed by kebab-case verb-led description: `0001-add-multi-factor-auth`
- `.tequila/work` tracks the currently focused task ID
- All artifacts live under `.tequila/tasks/{task-id}/` in the target repository
- Do not create auxiliary documents outside `.tequila/` unless explicitly instructed
- Favor minimal implementations; avoid scope creep
