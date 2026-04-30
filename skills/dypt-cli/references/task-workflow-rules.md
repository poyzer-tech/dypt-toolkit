# dypt Task Workflow Rules

Use these rules when an agent is changing dypt tasks, notes, hierarchy,
dependencies, or status.

## Canonical Tracker

- dypt is the source of truth for tracked work.
- Do not create markdown TODO lists, scratch issue lists, or another tracker for
  work that should survive the session.
- If follow-up work is real, create or update a dypt task.
- Search first with `dypt task search`, `dypt note search`, or filtered
  `dypt task list` before creating a new task.
- Before creating or moving a task, identify the correct parent or scope from
  the user request, current session context, existing dypt hierarchy, or
  repository guidance files such as `AGENTS.md` or `CLAUDE.md`.
- If the correct parent or scope remains ambiguous, ask the user to confirm.
  Do not guess a durable task location when the hierarchy is part of the user's
  workflow.

## Before Changing Work

Read enough context to avoid damaging structure:

```bash
dypt task get <ref>
dypt task summary <ref>
dypt task tree <ref> --depth 2
dypt note get <ref>
dypt dep list <ref>
```

For large branches, use targeted commands:

```bash
dypt task plan next-actions <ref>
dypt task plan focus <ref>
dypt task plan metadata <ref> --leaves-only
```

## Status Rules

- Mark a leaf task `completed` only when the task is genuinely complete.
- Do not set a leaf task to `in progress`; dypt uses `in progress` as a derived
  parent state when a task has child progress.
- If work needs multiple stages before completion, create subtasks and update
  those subtasks instead of forcing a leaf into `in progress`.
- Do not mark a parent complete unless its relevant child work is complete or
  intentionally archived/removed.

## Notes Rules

- Use markdown headings and bullets for readable notes.
- For notes longer than a sentence or two, structure the note with markdown
  headings, short paragraphs, and bullets. Do not write a single dense block of
  text.
- Capture durable context: decisions, acceptance criteria, implementation notes,
  risks, validation, and handoff.
- When referencing other tasks in notes, use dypt task links as
  `[#123](#task-123)`.
- Do not write `[#123](#task-123) Task title`; dypt renders the title
  automatically and duplicated titles are noisy.
- When updating a note, preserve useful existing context unless the user asked
  to rewrite it.

## Hierarchy and Cleanup Rules

- Preserve the user's activity-based hierarchy unless they ask to restructure it.
- Prefer metadata such as priority, deadline, and expected time to surface focus
  work rather than moving tasks into artificial focus folders.
- For bulk changes, generate the task ids with `task list`, `task search`, or
  planning helpers, then preview the bulk operation where supported.
- Preview cleanup before applying.
- Use `relocate-and-collapse` when moving surviving direct children out of a
  wrapper and archiving the wrapper only if it becomes empty.

## Dependencies

- Use `dypt dep add <task> <blocker>` when task completion depends on another
  task.
- Check dependencies before starting or completing work.
- Remove dependencies when they are no longer true.

## Handoff Rules

At the end of work:

- Update relevant task notes with what changed and any remaining risks.
- Mark completed tasks completed.
- Create dypt tasks for any follow-up work that should not be lost.
- If code was changed, mention validation performed in the handoff note or chat
  response.
