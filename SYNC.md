# Codex profile sync policy

This repository backs up selected user-level Codex configuration for Xin and
Ziki. Each machine selects one profile with `./dots profile <name>`.

- **Backup** means home directory to the selected repository profile.
- **Restore** means the selected repository profile to the home directory.

Copy only the paths in the allowlist below. Review every backup diff before
committing it.

## Profile selection

The launcher stores `xin` or `ziki` in `.dots-profile`. This ignored file is
machine state. Never infer a profile from Git identity, operating-system user,
or existing home configuration.

Stop backup, restore, and status operations when no valid profile is selected.
Resolve every repository path below relative to `profiles/<profile>/`.

## Directory mapping

| Selected profile path | User configuration path |
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

Mirror the selected profile's skill set exactly in either direction. Remove
nested `.git/` directories from installed skills. Preserve harness metadata
such as `agents/openai.yaml`.

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

Each committed profile contains durable preferences only. During backup, remove:

- `notify`
- `[marketplaces.*]`
- `[projects.*]`
- `[mcp_servers.*]`
- `[hooks.state.*]`
- `[tui.model_availability_nux]`
- Runtime-bundled plugin and marketplace cache entries
- Any host-generated identifiers or local paths

For the Ziki profile, also remove these PR-only desktop keys:

- `git-pull-request-merge-method`
- `git-show-sidebar-pr-icons`
- `git-pr-instructions`

Keep `model_instructions_file = "model-instructions.md"` so the path resolves
beside the live config.

During restore, merge durable repository values into the live config without
overwriting machine-local sections. When restoring Ziki's profile, remove the
three PR-only desktop keys listed above. If no live config exists, copy the
curated profile file as-is.

## Backup procedure

1. Confirm the selected profile and its directory.
2. Compare `~/.codex/AGENTS.md` with `~/.agents/AGENTS.md` when the former is not
   the expected symlink. Stop on any unexplained difference.
3. Copy `~/.agents/AGENTS.md` to the selected profile's `dotagents/AGENTS.md`.
4. Mirror `~/.agents/skills/` to the selected profile's `dotagents/skills/`.
5. Copy `~/.codex/model-instructions.md` to the selected profile's
   `dotcodex/model-instructions.md`.
6. Curate the live `config.toml` and write the durable values to the selected
   profile's `dotcodex/config.toml`.
7. Review only the selected profile's allowlisted diff. Do not force-add an
   ignored file.

## Restore procedure

1. Confirm the selected profile and its directory.
2. Back up every replaced live file and skill under `~/.codex/backups/`.
3. Write the selected `dotagents/AGENTS.md` to `~/.agents/AGENTS.md`.
4. Mirror the selected `dotagents/skills/` to `~/.agents/skills/`.
5. Create `~/.codex/AGENTS.md -> ../.agents/AGENTS.md`.
6. Copy the selected `dotcodex/model-instructions.md` to
   `~/.codex/model-instructions.md`.
7. Merge the selected durable `dotcodex/config.toml` values into the live
   config, applying the Ziki-specific removals when applicable.
8. Preserve `~/.codex/skills/.system/`. Remove only confirmed legacy user-skill
   copies from `~/.codex/skills/`.
9. Sign in to Codex and GitHub separately. Never restore authentication.
