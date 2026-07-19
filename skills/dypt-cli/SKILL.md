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
- If the right parent or scope is not clear from the user request, current
  session, or repository guidance such as `AGENTS.md` or `CLAUDE.md`, ask the
  user to confirm before creating or moving tasks.
- Read task notes, children, dependencies, and nearby branch context before
  changing work.
- Use dypt notes for durable handoff context, decisions, and implementation
  details.
- When referring to tasks in chat or handoff back to the user, include both the
  task id and a short title, for example `#123 Add CLI docs`.
- For task references in notes, use dypt task links as `[#123](#task-123)`. Do
  not repeat the task title after the link because dypt renders it
  automatically.
- Format longer notes with appropriate markdown so they are well presented and
  readable rather than one large paragraph.
- Do not set leaf tasks to `in progress`; dypt derives `in progress` for parent
  tasks from child state.
- When creating a task tree from an implementation plan, set task positions to
  match the recommended execution order. Use visible order for sequencing and
  dependencies only for true blocker relationships.
- Preview cleanup operations before applying them.
- Do not invent CLI subcommands, positional arguments, or option names. Before
  an unfamiliar command or a multi-command mutation batch, copy the exact form
  from `references/cli-basics.md` or run the specific command's `--help`. If a
  command fails, inspect the error and confirmed syntax before retrying; do not
  keep guessing variants.
- Track discovered follow-up work as dypt tasks only when the user request and
  governing workflow authorize task writes. A workflow-specific approval
  threshold or read-only boundary overrides this default.
- Keep task status and notes current before handing off.
- Do not create a parallel TODO tracker.

## Working Pattern

1. Inspect: find the relevant task, then read `get`, `summary`, `tree`, notes,
   and dependencies as needed.
2. Plan: use `plan next-actions`, `plan focus`, or `plan metadata` when the user
   asks what to do next or how to prioritise.
3. Act: implement or organize the work requested by the user. When organizing
   sibling tasks, use `task update --position` for one-off moves and
   `task reorder --preview` before applying a full sibling order.
4. Record: update task notes with durable context and mark tasks completed only
   when the work is actually done.
5. Handoff: when task writes are authorized, create dypt tasks for remaining
   work instead of leaving loose TODOs.

## References

- Read `references/cli-basics.md` for command syntax, auth, filters, JSON
  output, planning helpers, cleanup, and dependencies.
- Read `references/task-workflow-rules.md` before changing task state, notes,
  dependencies, hierarchy, or cleanup.
- Read `references/playbook.md` for reusable CLI + agent workflow patterns.
- Read `references/examples.md` for concrete end-to-end scenarios.
