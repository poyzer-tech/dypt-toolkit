---
name: dypt-cli
description: Use when working with dypt tasks through the dypt CLI, including inspecting task context, choosing next work, updating task notes/status, managing dependencies, previewing cleanup operations, or coordinating AI-agent implementation workflows.
---

# dypt CLI

Use this skill when a user wants an agent to work with dypt tasks, or when a
repository names dypt as its work tracker.

## Core Rules

- Use dypt tasks as the canonical work tracker.
- Search before creating new tasks to avoid duplicates.
- Read task notes, children, dependencies, and nearby branch context before
  changing work.
- Use dypt notes for durable handoff context, decisions, and implementation
  details.
- Use dypt task links in notes as `[#123](#task-123)`. Do not repeat the task
  title after the link because dypt renders it automatically.
- Do not set leaf tasks to `in progress`; dypt derives `in progress` for parent
  tasks from child state.
- Preview cleanup operations before applying them.
- Track discovered follow-up work as dypt tasks.
- Keep task status and notes current before handing off.
- Do not create a parallel TODO tracker.

## Working Pattern

1. Inspect: find the relevant task, then read `get`, `summary`, `tree`, notes,
   and dependencies as needed.
2. Plan: use `plan next-actions`, `plan focus`, or `plan metadata` when the user
   asks what to do next or how to prioritise.
3. Act: implement or organize the work requested by the user.
4. Record: update task notes with durable context and mark tasks completed only
   when the work is actually done.
5. Handoff: create dypt tasks for remaining work instead of leaving loose TODOs.

## References

- Read `references/cli-basics.md` for command syntax, auth, filters, JSON
  output, planning helpers, cleanup, and dependencies.
- Read `references/task-workflow-rules.md` before changing task state, notes,
  dependencies, hierarchy, or cleanup.
- Read `references/playbook.md` for reusable CLI + agent workflow patterns.
- Read `references/examples.md` for concrete end-to-end scenarios.
