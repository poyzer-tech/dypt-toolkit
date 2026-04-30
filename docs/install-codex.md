# Install for Codex

Use this guide to install the `dypt-cli` skill for Codex from the public
`poyzer-tech/dypt-toolkit` repository.

## Prerequisites

- A dypt account with CLI access enabled.
- The dypt CLI installed and authenticated:

```bash
npm install -g @dypt/cli
dypt auth login
dypt auth status
```

## Install the Skill Manually

Codex user skills live in `$CODEX_HOME/skills`. If `CODEX_HOME` is not set,
Codex uses `~/.codex`.

Install the latest public version:

```bash
codex_home="${CODEX_HOME:-$HOME/.codex}"
tmp_dir="$(mktemp -d)"
git clone https://github.com/poyzer-tech/dypt-toolkit.git "$tmp_dir/dypt-toolkit"
mkdir -p "$codex_home/skills"
rm -rf "$codex_home/skills/dypt-cli"
cp -R "$tmp_dir/dypt-toolkit/skills/dypt-cli" "$codex_home/skills/dypt-cli"
rm -rf "$tmp_dir"
```

Restart Codex after installing or updating the skill.

## Install Through Codex

If your Codex session has the skill installer available, you can also ask Codex:

```text
Install the dypt-cli skill from poyzer-tech/dypt-toolkit at skills/dypt-cli.
```

Codex should install the skill into `$CODEX_HOME/skills/dypt-cli`.

## Verify the Skill

Start a new Codex session and ask for a dypt task workflow:

```text
Use dypt to inspect my current tasks and suggest the next useful task to work on.
```

Expected behavior:

- Codex should use the dypt CLI when it needs task context.
- Task references back to you should include both id and title, for example
  `#123 Add CLI docs`.
- If Codex needs to create or move a task and the parent/scope is unclear, it
  should ask you to confirm the destination.

You can also verify the skill files exist:

```bash
codex_home="${CODEX_HOME:-$HOME/.codex}"
ls "$codex_home/skills/dypt-cli"
```

## Use With Project Instructions

For repositories that use dypt as the work tracker, add dypt guidance to the
repo's `AGENTS.md`.

Use the snippet in [project-instructions.md](project-instructions.md), or adapt
it to match the repository's conventions.

## Update the Skill

Run the install commands again to replace your local copy with the latest
version from GitHub, then restart Codex.
