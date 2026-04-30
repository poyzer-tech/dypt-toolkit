# dypt CLI Examples

Concrete examples for agents using dypt as the work tracker.

## Example: Choose Next CLI Task

User intent: "What's next in the CLI tree?"

Commands:

```bash
dypt task summary "CLI"
dypt task plan focus "CLI"
dypt task plan next-actions "CLI"
dypt task list --scope "CLI" --priority high --status "not started" --show-path
```

Expected behavior:

- Recommend one next task, using both id and short title.
- Explain why it is next based on blockers, priority, and delivery sequence.
- Do not change task hierarchy just to create focus.

## Example: Implement a Task

User intent: "Please implement #123."

Commands:

```bash
dypt task get 123
dypt note get 123
dypt task tree 123 --depth 2
dypt dep list 123
```

After implementation:

```bash
dypt note set 123 "## Implementation

- Added the requested behavior.
- Validated with pnpm test:unit.

## Follow-up

- [#456](#task-456)"
dypt task update 123 --status completed
```

Expected behavior:

- Inspect first, implement second.
- Update notes with durable context.
- Complete the task only after validation.
- Create/link follow-up tasks when remaining work exists.

## Example: Capture Follow-Up Work

User intent: "We should add a CLI version command later."

Commands:

```bash
dypt task search "version command" --scope "CLI"
dypt task create "Add CLI version command" --parent "CLI" --priority high
dypt note set <new-task-id> "## Goal

Expose the installed dypt CLI version so users and agents can report what they
are running.

## Acceptance criteria

- `dypt version` prints the current package version.
- `dypt --version` still works."
```

Expected behavior:

- Search before creating.
- Create the task in the proper branch.
- Add enough notes that another agent can implement it.

## Example: Ambiguous Follow-Up Task

User intent: "Create a follow-up task for improving this later."

Commands:

```bash
dypt task search "improving this later"
dypt task list --scope <likely-branch> --show-path
```

Expected behavior:

- Search first to avoid duplicating existing work.
- If the parent or scope is not clear from the user request, current session,
  repository instructions, or dypt hierarchy, ask the user where the task should
  go before creating it.
- Do not guess a durable task location when multiple plausible parents exist.

## Example: Reprioritise Without Restructuring

User intent: "Make the work I care about surface on the dashboard."

Commands:

```bash
dypt task plan metadata "dypt" --leaves-only
dypt task update 101 --priority high --deadline 2026-05-03 --expected-time 90
dypt task update 102 --priority high --expected-time 45
```

Expected behavior:

- Use metadata to surface work.
- Preserve the user's existing activity-based hierarchy.
- Explain any task that should remain lower priority.

## Example: Relocate and Collapse Wrapper

User intent: "Move these useful children to the right branch and archive the old
wrapper if it becomes empty."

Commands:

```bash
dypt task tree "Old wrapper" --depth 2
dypt task cleanup relocate-and-collapse --wrapper "Old wrapper" --tasks 42 43 --to "New home"
dypt task cleanup relocate-and-collapse --wrapper "Old wrapper" --tasks 42 43 --to "New home" --yes
```

Expected behavior:

- Preview first.
- Confirm selected tasks are active direct children of the wrapper.
- Apply only if the preview shows the intended moves and wrapper archive.

## Example: Bulk Archive Completed Leaves

User intent: "Archive this set of completed tasks."

Commands:

```bash
dypt task list --scope "CLI" --status completed --leaves --show-path
dypt task update-many 101 102 103 --archive --preview
dypt task update-many 101 102 103 --archive
```

Expected behavior:

- Use filters to find candidate ids.
- Preview the exact selected ids before mutating.
- Avoid archiving tasks merely because they are inside an archived branch unless
  that is the user's intent.
