---
name: dypt-cli
description: Use when working with dypt tasks through the dypt CLI, including inspecting task context, choosing next work, updating task notes/status, managing dependencies, previewing cleanup operations, or coordinating AI-agent implementation workflows.
---

# dypt CLI

Use this skill when a user wants an agent to work with dypt tasks or when a repository uses dypt as its work tracker.

## Core Rules

- Use dypt tasks as the canonical work tracker.
- Search before creating new tasks to avoid duplicates.
- Read task notes, children, and dependencies before changing work.
- Use dypt notes for durable handoff context.
- Use dypt task links in notes: `[#123](#task-123)`.
- Do not set leaf tasks to `in progress`; in dypt this is a derived parent state.
- Preview cleanup operations before applying them.
- Do not create a parallel TODO tracker.

## References

- `references/cli-basics.md`: install, auth, and common commands.
- `references/task-workflow-rules.md`: dypt-specific agent rules.
- `references/playbook.md`: reusable CLI + agent workflow patterns.
- `references/examples.md`: concrete end-to-end scenarios.
