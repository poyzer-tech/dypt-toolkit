# dypt toolkit

Reusable resources for working with [dypt](https://dypt.app) from AI agents and command-line workflows.

This repository is the public home for dypt-adjacent guidance such as skills, prompts, examples, and agent configuration snippets. The first package is a `dypt-cli` skill that helps agents use the dypt CLI safely and consistently.

## What is included

- `skills/dypt-cli`: agent skill package for dypt CLI workflows.
- `docs/install-claude.md`: manual installation notes for Claude.
- `docs/install-codex.md`: manual installation notes for Codex.
- `docs/project-instructions.md`: repo-level instruction snippets for projects that use dypt.

## V1 support

V1 focuses on Claude and Codex skill-style installs. Cursor, Gemini, marketplace packaging, and CLI-assisted installation are planned later.

## dypt CLI

Install the dypt CLI from npm:

```bash
npm install -g @dypt/cli
```

Then authenticate:

```bash
dypt auth login
```

## License

MIT
