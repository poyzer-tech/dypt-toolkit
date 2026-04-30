# Project Instructions

Use these snippets in repositories that track work with dypt.

Add the relevant snippet to `AGENTS.md`, `CLAUDE.md`, or the repository's
equivalent agent instruction file.

## General Snippet

```markdown
## Work Tracking with dypt

This project uses dypt tasks as the canonical work tracker.

When working on repo tasks:

- Use the dypt CLI to inspect tasks, notes, children, dependencies, and nearby
  branch context before changing work.
- Search before creating new tasks to avoid duplicates.
- If the right parent or scope for a new or moved task is not clear from the
  user request, current session, existing dypt hierarchy, or this file, ask the
  user to confirm before changing task structure.
- Capture durable context in dypt notes.
- Format longer dypt notes with appropriate markdown so they are readable and
  well presented.
- When referencing tasks in dypt notes, use dypt task links such as
  `[#123](#task-123)` and do not repeat the task title after the link.
- When referring to tasks back to the user, include both id and title, for
  example `#123 Add CLI docs`.
- Track follow-up work as dypt tasks.
- Do not create a parallel markdown TODO tracker.
- Do not set leaf tasks to `in progress`; dypt derives `in progress` for parent
  tasks from child state.
- Preview cleanup or bulk changes before applying them when preview is
  available.
- If the `dypt-cli` skill is available, use it for dypt task workflows.
```

## Optional Scope Hint

If a repository usually tracks work under a specific dypt branch, make that
explicit:

```markdown
## dypt Scope

Use the dypt task branch `<branch name or task id>` as the default scope for
this repository.

If a task does not clearly belong under that branch, ask before creating or
moving it.
```

## Useful Commands

```bash
dypt task search "<query>"
dypt task get <ref>
dypt task summary <ref>
dypt task tree <ref> --depth 2
dypt note get <ref>
dypt dep list <ref>
dypt task plan next-actions <ref>
dypt task plan focus <ref>
dypt task plan metadata <ref> --leaves-only
```
