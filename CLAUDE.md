# Working in this repo

This repository backs up allowlisted user-level Codex configuration for Xin and
Ziki. Each machine selects one profile, and sync runs in both directions between
that profile and the live user configuration.

Before backup or restore, read [SYNC.md](SYNC.md). It defines profile selection,
directory mapping, the allowlist, sanitization, and merge behavior.

Never commit authentication, sessions, logs, SQLite databases, caches, or other
machine state. `.gitignore` is a safety net; the allowlist in `SYNC.md` is the
primary control.

This file guides agents working on the repository. The installed instructions
live under `profiles/<profile>/dotagents/AGENTS.md`.
