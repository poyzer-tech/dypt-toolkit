# dypt CLI Basics

Use the dypt CLI to inspect, create, update, organize, and complete dypt tasks
from a terminal. Prefer `--json` when the output will be parsed by an agent or
script.

## Install and Auth

Install the published CLI:

```bash
npm install -g @dypt/cli
```

The CLI defaults to the main dypt app. Users normally do not need to configure a
base URL.

Sign in and verify auth:

```bash
dypt auth login
dypt auth status
```

Useful account commands:

```bash
dypt auth status --json
dypt auth logout
dypt version
dypt --version
```

## Inspect Tasks

Use ids when available. Title refs work, but ambiguous titles can fail and ask
for a more precise ref.

```bash
dypt task list
dypt task list --limit 20 --show-path
dypt task get 123
dypt task get 123 --json
dypt task tree 123 --depth 3
dypt task tree 123 --full
dypt task summary 123
```

`summary` is branch-level context. It shows direct-child counts, total
descendant counts, status/priority split, note counts, due-date counts, and
health signals.

## Filter Tasks

`list` supports composable filters. Use these to get ids for bulk operations
rather than adding bespoke one-off commands.

```bash
dypt task list --scope "CLI" --status "not started" --priority high --show-path
dypt task list --parent "CLI" --archived active
dypt task list --scope "Work" --leaves --blocked only
dypt task list --scope "dypt" --deadline-to 2026-05-07 --show-path
dypt task list --title-contains "docs" --archived all
```

Common filters:

- `--status "not started"|"in progress"|"completed"`
- `--priority low|mid|medium|high`
- `--parent <ref>` for direct children
- `--scope <ref>` for all descendants
- `--archived active|archived|all`
- `--leaves`
- `--blocked show|hide|only`
- `--blocking show|hide|only`
- `--deadline-from <date>` and `--deadline-to <date>`
- `--expected-time-from <minutes>` and `--expected-time-to <minutes>`
- `--actual-time-from <minutes>` and `--actual-time-to <minutes>`
- `--limit <n>` and `--cursor <id>` for paging

## Search

Search when you do not know the exact task location.

```bash
dypt task search "CLI docs"
dypt task search "publish" --scope "CLI" --limit 8
dypt note search "agent workflow"
```

## Create and Update Tasks

Create tasks with useful metadata immediately when known.

```bash
dypt task create "Write CLI docs" --parent 70867 --priority high --deadline 2026-05-05 --expected-time 60
dypt task update 123 --priority high
dypt task update 123 --deadline 2026-05-05
dypt task update 123 --expected-time 45
dypt task update 123 --actual-time 50
dypt task update 123 --status completed
```

Do not set a leaf task to `in progress`. Complete leaf tasks only when done.
Parent `in progress` state is derived from children.

## Bulk Updates

Preview bulk updates first when there is any chance the selected ids are wrong.

```bash
dypt task update-many 101 102 103 --priority high --preview
dypt task update-many 101 102 103 --priority high
dypt task update-many 101 102 --parent "CLI"
dypt task update-many 101 102 --archive
dypt task update-many 101 102 --unarchive
dypt task delete-many 101 102 --yes
```

## Notes

Use notes for decisions, handoff context, implementation details, and
acceptance criteria. Use markdown formatting.

```bash
dypt note get 123
dypt note set 123 "## Context

- This task depends on [#456](#task-456).
- Keep the public docs aligned with the CLI README."
```

Task note aliases also exist:

```bash
dypt task note get 123
dypt task note set 123 "Updated note content"
dypt task note search "release"
```

## Planning Helpers

Use these when selecting or shaping work in a branch.

```bash
dypt task plan next-actions 70867
dypt task plan focus 70867
dypt task plan metadata 70867
dypt task plan metadata 70867 --leaves-only --priority --deadline --expected-time
```

- `next-actions` lists likely ready tasks and reasons.
- `focus` lists child branches with near-term signals.
- `metadata` finds tasks missing priority, deadline, or expected-time planning
  fields.

## Dependencies

Use dependencies when one task cannot start or finish until another task is
done.

```bash
dypt dep list 123
dypt dep add 123 456
dypt dep remove 123 456
```

`dypt dep add <task> <blocker>` means `<task>` is blocked by `<blocker>`.

## Cleanup

Prefer named cleanup workflows for normal task refs. Preview is the default for
`relocate-and-collapse`; add `--yes` only after the preview matches the intended
changes.

```bash
dypt task cleanup relocate-and-collapse --wrapper "Old wrapper" --tasks 42 43 --to "Canonical branch"
dypt task cleanup relocate-and-collapse --wrapper 100 --tasks 101 --to root --yes --json
```

The lower-level `cleanup preview` and `cleanup apply` commands are for explicit
cleanup plan JSON, not standalone task refs:

```bash
dypt task cleanup preview --plan '{"operations":[{"type":"archive","taskIds":[42]}]}'
dypt task cleanup apply --plan-file /tmp/cleanup-plan.json --yes
```
