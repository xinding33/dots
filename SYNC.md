# Codex sync policy

This repository backs up selected user-level Codex configuration for one macOS
machine.

- **Backup** means home directory to repository.
- **Restore** means repository to home directory.

Copy only the paths in the allowlist below. Review every backup diff before
committing it.

## Directory mapping

| Repo path | User configuration path |
| --- | --- |
| `dotagents/AGENTS.md` | `~/.agents/AGENTS.md` |
| `dotagents/skills/` | `~/.agents/skills/` |
| `dotcodex/config.toml` | `~/.codex/config.toml` |
| `dotcodex/model-instructions.md` | `~/.codex/model-instructions.md` |

Keep `~/.agents/AGENTS.md` canonical. Restore this relative compatibility link:

```text
~/.codex/AGENTS.md -> ../.agents/AGENTS.md
```

If a symlink is unavailable, write a byte-identical copy. Before backup, stop
and reconcile any difference between the compatibility file and the canonical
file.

## Allowlist

Sync only these files:

- `~/.agents/AGENTS.md`
- Every skill directory directly under `~/.agents/skills/`
- `~/.codex/config.toml`, curated as described below
- `~/.codex/model-instructions.md`

Mirror the shared skill set exactly in either direction. Remove nested `.git/`
directories from installed skills. Preserve harness metadata such as
`agents/openai.yaml`.

Do not back up `~/.codex/AGENTS.md`; it is the compatibility link or copy. Do
not copy user skills into `~/.codex/skills/`. User skills live in
`~/.agents/skills/`. Preserve the app-managed `~/.codex/skills/.system/`
directory during restore.

## Never sync

Never sync authentication, sessions, history, logs, SQLite databases, caches,
telemetry, generated media, worktrees, secrets, or other machine-local state.

Notable exclusions include:

- `~/.codex/auth.json`
- `~/.codex/sessions/` and `~/.codex/archived_sessions/`
- `~/.codex/sqlite/` and every `*.sqlite*` file
- `~/.codex/cache/`, `~/.codex/tmp/`, and `~/.codex/.tmp/`
- `~/.codex/browser/` and `~/.codex/computer-use/`
- `~/.codex/generated_images/`, `~/.codex/pets/`, and `~/.codex/worktrees/`
- `~/.codex/marketplaces/`, runtime plugin caches, and MCP authentication
- `~/.codex/automations/`, which may contain machine-local task state

The `.gitignore` is a safety net. This allowlist is the primary control.

## Curate `config.toml`

The committed file contains durable preferences only. During backup, remove:

- `notify`
- `[marketplaces.*]`
- `[projects.*]`
- `[mcp_servers.*]`
- `[hooks.state.*]`
- `[tui.model_availability_nux]`
- Runtime-bundled plugin and marketplace cache entries
- Any host-generated identifiers or local paths

Keep `model_instructions_file = "model-instructions.md"` so the path resolves
beside the live config.

During restore, merge durable repository values into the live config without
overwriting machine-local sections. If no live config exists, copy the curated
file as-is.

## Backup procedure

1. Compare `~/.codex/AGENTS.md` with `~/.agents/AGENTS.md` when the former is not
   the expected symlink. Stop on any unexplained difference.
2. Copy `~/.agents/AGENTS.md` to `dotagents/AGENTS.md`.
3. Mirror `~/.agents/skills/` to `dotagents/skills/`.
4. Copy `~/.codex/model-instructions.md` to `dotcodex/model-instructions.md`.
5. Curate the live `config.toml` and write the durable values to
   `dotcodex/config.toml`.
6. Review the exact allowlisted diff. Do not force-add an ignored file.

## Restore procedure

1. Write `dotagents/AGENTS.md` to `~/.agents/AGENTS.md`.
2. Mirror `dotagents/skills/` to `~/.agents/skills/`.
3. Create `~/.codex/AGENTS.md -> ../.agents/AGENTS.md`.
4. Copy `dotcodex/model-instructions.md` to
   `~/.codex/model-instructions.md`.
5. Merge the durable `dotcodex/config.toml` values into the live config.
6. Preserve `~/.codex/skills/.system/`. Remove only confirmed legacy user-skill
   copies from `~/.codex/skills/`.
7. Sign in to Codex and GitHub separately. Never restore authentication.
