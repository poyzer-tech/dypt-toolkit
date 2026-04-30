# dypt CLI Playbook

Reusable patterns for using dypt with coding agents.

## Pick Next Work From a Branch

Use when the user asks "what's next?" or wants a delivery order inside a
branch.

```bash
dypt task summary <branch>
dypt task plan focus <branch>
dypt task plan next-actions <branch>
dypt task list --scope <branch> --priority high --status "not started" --show-path
```

Recommended agent behavior:

- Start with the user's stated goal, not only the highest-scored CLI output.
- Prefer unblocked leaf tasks with clear notes and high/urgent metadata.
- Explain the recommended next task with both id and abbreviated title.
- If the branch structure is stale, propose note/metadata updates rather than
  reshaping hierarchy by default.

## Inspect Task Before Implementation

Use before changing code or task state.

```bash
dypt task get <task>
dypt note get <task>
dypt task tree <task> --depth 2
dypt dep list <task>
```

Recommended agent behavior:

- Validate that the task still makes sense.
- Identify blockers, ambiguous requirements, and missing acceptance criteria.
- Ask only if the ambiguity blocks safe execution; otherwise make a reasonable
  assumption and record it in the task note.

## Task-Driven Implementation Loop

Use when a user asks an agent to implement a dypt task.

1. Inspect the task and nearby context.
2. Confirm or refine the implementation shape in the task note.
3. Implement the change.
4. Run relevant validation.
5. Update the task note with implementation details and validation.
6. Mark the task complete when finished.
7. Create follow-up dypt tasks for remaining work.

Do not mark a leaf task `in progress`; if the work needs staged tracking, create
subtasks.

## Cleanup Preview and Apply Loop

Use when consolidating duplicate branches, moving children to a better home, or
collapsing an empty wrapper.

```bash
dypt task tree "Old wrapper" --depth 2
dypt task cleanup relocate-and-collapse --wrapper "Old wrapper" --tasks 42 43 --to "Canonical branch"
dypt task cleanup relocate-and-collapse --wrapper "Old wrapper" --tasks 42 43 --to "Canonical branch" --yes
```

Recommended agent behavior:

- Check the wrapper's active direct children before cleanup.
- Preview first and inspect every planned move/archive.
- Apply only after the preview matches the user's intent.
- Update notes if the cleanup changes the meaning of a branch.

## Reprioritization Pass

Use when a user wants the dashboard or planning views to surface current focus
without changing the underlying activity hierarchy.

```bash
dypt task summary <branch>
dypt task plan metadata <branch> --leaves-only
dypt task list --scope <branch> --show-path --archived active
dypt task update <task> --priority high
dypt task update <task> --deadline 2026-05-07
dypt task update <task> --expected-time 45
```

Recommended agent behavior:

- Treat priority, deadline, and expected time as focus metadata.
- Do not create a "this week" or focus hierarchy unless the user explicitly
  wants that structure.
- Keep notes aligned with the new prioritisation if the reason is important.

## Workflow Capture and Productisation

Use when repeated CLI + agent behavior looks like it could become a native dypt
feature, command, or documented workflow.

```bash
dypt task create "Product candidate: native next best task recommendations" --parent <workflow-parent>
dypt note set <task> "## Candidate

Capture the repeated workflow, user goal, CLI steps, missing app affordance, and
possible product shape."
```

Recommended agent behavior:

- Capture repeatable workflows as tasks under the relevant productisation parent.
- Link related tasks with `[#id](#task-id)` instead of duplicating titles.
- Separate "playbook we can use now" from "product candidate we may build
  later".

## Docs and Marketing Iteration

Use when implementing public-facing docs or marketing pages from dypt tasks.

```bash
dypt note get <task>
dypt task list --scope <task> --show-path
dypt task update <task> --status completed
```

Recommended agent behavior:

- Keep copy changes grounded in the user's positioning.
- Validate rendered pages when possible.
- Capture remaining docs gaps as tasks, especially when another workstream owns
  the broader docs refresh.
