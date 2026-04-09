# How It Works

## What Claude Reads Every Session

When you open Claude Code in a project, it automatically reads `CLAUDE.md` from the project root and loads it into the context window before you type your first message. This happens every session, every time.

Whatever is in `CLAUDE.md` costs tokens — unconditionally.

## The Monolithic Problem

Most teams start with one `CLAUDE.md` and keep adding rules:

```markdown
# Project Instructions

## Coding Standards
[50 lines]

## Git Workflow
[30 lines]

## Database Rules
[40 lines]

## Security
[60 lines]

## Testing
[40 lines]

## API Conventions
[50 lines]
...
```

After a year, this file is 3,000–8,000 lines. Claude reads all of it — even when you're asking a simple question about a component name. You're paying for context you don't need.

## The leanclaude Solution

`CLAUDE.md` becomes a lean index — just a table pointing to rule files:

```markdown
| Rule file | Covers |
|---|---|
| `git-workflow.md` | Branching, commits, PRs |
| `security-basics.md` | OWASP Top 10, secrets |
...
```

The detailed rules live in focused `.claude/rules/` files. Claude reads the index (cheap), then accesses specific rule files when they're relevant to the current task (on demand).

## The Token Math

| File | Lines | ~Tokens |
|---|---|---|
| Monolithic CLAUDE.md (typical after 1 year) | 5,000 | ~13,000 |
| leanclaude CLAUDE.md (index only) | ~110 | ~300 |
| All rule files combined (loaded as needed) | ~1,700 | ~4,200 |
| **leanclaude total** | **~1,810** | **~4,500** |
| **Savings per session** | | **~8,500 tokens** |

At 20 sessions/day: **~170,000 tokens/day saved**.

## What Gets Loaded

```
Every session (automatic):
  ✅ CLAUDE.md — the lean index (~300 tokens)
  ✅ Memory files — MEMORY.md index + active memory files (~200–500 tokens)

On demand (when referenced or relevant):
  ✅ Rule files — loaded by Claude as needed
  ✅ Agents — loaded only when invoked
  ✅ Commands — loaded only when the slash command is run
```

## The Quality Benefit

Beyond token savings, focused rule files are:

- **Easier to maintain** — add a new rule without touching a 5,000-line file
- **Easier to review** — git diff on a single rule file is readable
- **Less likely to conflict** — rules in separate files don't accidentally overwrite each other
- **Easier to share** — copy one rule file to another project

## The Memory System

Memory files (`.claude/memory/`) persist user preferences, project context, and feedback across sessions. They are separate from rules because:

- **Rules** = how Claude should behave (universal, reusable)
- **Memory** = what Claude should know about you and this project (specific, evolving)

Mixing them inflates both. Keep them separate.
