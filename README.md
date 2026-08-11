# Codex dots

This repository backs up durable user-level Codex configuration for one macOS
machine. It is safe to keep public because authentication, sessions, logs,
caches, and other machine state are excluded.

## Layout

| Repo path | Installed path | Purpose |
| --- | --- | --- |
| `dotagents/AGENTS.md` | `~/.agents/AGENTS.md` | Personal coding and communication rules |
| `dotagents/skills/` | `~/.agents/skills/` | Reusable Codex workflows |
| `dotcodex/config.toml` | `~/.codex/config.toml` | Durable Codex and desktop preferences |
| `dotcodex/model-instructions.md` | `~/.codex/model-instructions.md` | Codex-specific operating instructions |

Codex reads `~/.codex/AGENTS.md`, which should be a relative symlink to the
canonical shared instructions:

```text
~/.codex/AGENTS.md -> ../.agents/AGENTS.md
```

## Defaults

- Use GPT-5.6 Sol at medium reasoning effort.
- Allow edits and commands inside the active workspace.
- Allow network access for package installation, GitHub, and CI.
- Require approval before writing outside the workspace.
- Keep memories and multi-agent tools disabled.
- Open files in VS Code.
- Use `codex/` branches and squash merges.
- Confirm a pull request's reason before opening it.
- Never merge a pull request without an explicit request.

## Skills

The selected skills cover TypeScript and React code quality, frontend design,
web animation, codebase improvement, documentation, durable prose, and shipping
a change through a green GitHub pull request.

## Sync policy

[SYNC.md](SYNC.md) is the source of truth for backup and restore. The routine is
agent-driven so it can curate `config.toml` and review every diff.

Run the launcher from this checkout:

```bash
./dots status
./dots backup
./dots restore
```

Each command opens Codex with the relevant `SYNC.md` procedure. `backup` stops
before committing or pushing. `restore` backs up replaced live files and asks
for approval before writing outside the repository.

Sign in to Codex and GitHub separately after restore. Never store credentials in
this repository.

## Origin

This is a modified version of the `agents` branch from
[`jssblck/dots`](https://github.com/jssblck/dots), adapted on August 10, 2026.
The repository remains available under the terms in [LICENSE](LICENSE).
