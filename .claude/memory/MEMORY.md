# Memory Index

> This file is the index for all persistent memory across Claude Code sessions.
> Keep entries concise — one line each, under 150 characters.
> Memory files live in `.claude/memory/`. Templates are in `.claude/memory/templates/`.

## Active Memory Files

<!-- Add entries here as you create memory files. Example format:
- [user.md](user.md) — Communication style, expertise level, tool preferences
- [feedback.md](feedback.md) — Corrections and lessons learned this project
- [project.md](project.md) — Project context, constraints, active work area
- [reference.md](reference.md) — Glossary, key file locations, external endpoints
-->

## How to Use This System

### Adding a Memory
1. Copy the relevant template from `.claude/memory/templates/`
2. Fill it in and save to `.claude/memory/`
3. Add a one-line entry to this index

### Updating a Memory
Edit the file directly. Update the `Last updated` date in the file.

### Removing a Memory
Delete the file and remove its entry from this index.

### When Claude Should Access Memory
- When starting a new session on a known project
- When the user references prior-conversation work
- When behavior should be informed by past corrections (`feedback.md`)

### Memory Types

| Type | File | What to store |
|---|---|---|
| `user` | `user.md` | Preferences, expertise, communication style |
| `feedback` | `feedback.md` | Corrections, what NOT to repeat, validated approaches |
| `project` | `project.md` | Project context, constraints, active work, key decisions |
| `reference` | `reference.md` | Glossary, file locations, external endpoints, team contacts |

## Important Notes

- Memory records can become stale. Verify against current code before acting on them.
- If a memory conflicts with current code state, trust the code — update the memory.
- Do NOT store things in memory that are derivable from reading the code or git history.
- Do NOT store things already documented in CLAUDE.md or rule files.
