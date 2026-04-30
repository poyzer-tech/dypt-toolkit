# Install for Claude

Use this guide to install the `dypt-cli` skill for Claude Code from the public
`poyzer-tech/dypt-toolkit` repository.

## Prerequisites

- A dypt account with CLI access enabled.
- The dypt CLI installed and authenticated:

```bash
npm install -g @dypt/cli
dypt auth login
dypt auth status
```

## Install the Skill

Claude Code personal skills live in `~/.claude/skills`.

Install the latest public version:

```bash
tmp_dir="$(mktemp -d)"
git clone https://github.com/poyzer-tech/dypt-toolkit.git "$tmp_dir/dypt-toolkit"
mkdir -p ~/.claude/skills
rm -rf ~/.claude/skills/dypt-cli
cp -R "$tmp_dir/dypt-toolkit/skills/dypt-cli" ~/.claude/skills/dypt-cli
rm -rf "$tmp_dir"
```

Restart Claude Code after installing or updating the skill.

## Verify the Skill

In Claude Code, ask for a dypt task workflow:

```text
Use dypt to inspect my current tasks and suggest the next useful task to work on.
```

Expected behavior:

- Claude should use the dypt CLI when it needs task context.
- Task references back to you should include both id and title, for example
  `#123 Add CLI docs`.
- If Claude needs to create or move a task and the parent/scope is unclear, it
  should ask you to confirm the destination.

You can also verify the skill files exist:

```bash
ls ~/.claude/skills/dypt-cli
```

## Use With Project Instructions

For repositories that use dypt as the work tracker, add dypt guidance to the
repo's `CLAUDE.md`.

Use the snippet in [project-instructions.md](project-instructions.md), or adapt
it to match the repository's conventions.

## Update the Skill

Run the install commands again to replace your local copy with the latest
version from GitHub, then restart Claude Code.
