# Codex dots

This repository is the source of truth for Xin and Ziki's durable user-level
Codex configuration. It is safe to keep public because authentication,
sessions, logs, caches, and other machine state are excluded.

## Select a profile

Each machine selects its profile once:

```bash
./dots profile xin
# or
./dots profile ziki
```

The selection is stored in the ignored `.dots-profile` file. Check it with:

```bash
./dots profile
```

Both profiles use GPT-5.6 Sol at medium reasoning effort.

Xin and Ziki share the same configuration and skills except for pull request
delivery. Xin has both workflows:

- `ship-it` verifies work, creates a commit and pull request, then drives CI to
  green.
- `yolo` verifies work, commits it, and pushes directly to the remote default
  branch.

Ziki has `yolo` only. Her profile does not use branches, worktrees, pull
requests, or PR-specific desktop preferences.

## Sync configuration

Run the launcher from this checkout:

```bash
./dots status
./dots backup
./dots restore
```

Each command opens Codex with the relevant [SYNC.md](SYNC.md) procedure for the
selected profile. `backup` stops before committing or pushing. `restore` backs
up replaced live files and asks for approval before writing outside the
repository.

Sign in to Codex and GitHub separately after restore. Never store credentials in
this repository.

## Stored configuration

Each directory under `profiles/` contains one user's allowlisted configuration:

| Profile path | Installed path | Purpose |
| --- | --- | --- |
| `dotagents/AGENTS.md` | `~/.agents/AGENTS.md` | Coding and communication rules |
| `dotagents/skills/` | `~/.agents/skills/` | Reusable Codex workflows |
| `dotcodex/config.toml` | `~/.codex/config.toml` | Durable Codex preferences |
| `dotcodex/model-instructions.md` | `~/.codex/model-instructions.md` | Codex operating instructions |

Codex reads `~/.codex/AGENTS.md`, which restore creates as a relative link to
the canonical shared instructions:

```text
~/.codex/AGENTS.md -> ../.agents/AGENTS.md
```

## Origin

This is a modified version of the `agents` branch from
[`jssblck/dots`](https://github.com/jssblck/dots), adapted on August 10, 2026.
The repository remains available under the terms in [LICENSE](LICENSE).
