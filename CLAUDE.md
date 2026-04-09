# Project Instructions

> This file is intentionally short. All rules live in `.claude/rules/` — keeping this index lean saves ~65% of context tokens per session compared to a monolithic CLAUDE.md.

## Project Overview

[Replace this with a 2-3 sentence description of your project, tech stack, and purpose.]

## Key File Locations

| What | Where |
|---|---|
| [Entry point] | `[path]` |
| [Config] | `[path]` |
| [Database/data layer] | `[path]` |

## Build & Run

```bash
# [Replace with your build command]
# [Replace with your run command]
```

## Rules Index

All coding standards and conventions are in `.claude/rules/`:

| Rule file | Covers |
|---|---|
| `git-workflow.md` | Branching, commits, PRs, merge strategy |
| `commit-format.md` | Conventional commit format and examples |
| `debugging-investigation.md` | Root cause first, 3-strike rule, debug report |
| `escalation-protocol.md` | When to stop and ask, BLOCKED format |
| `long-term-thinking.md` | Build for scale, no short-term patches |
| `adding-rules.md` | When and how to add or update rule files |
| `pre-pr-review.md` | Scope drift check, specialist checklist |
| `validate-after-changes.md` | Validate after every code change |
| `security-basics.md` | OWASP Top 10, secrets, input validation |
| `code-review-checklist.md` | PR review checklist |

### Framework-Specific Rules (pick your stack, delete the rest)

| Rule file | Covers |
|---|---|
| `examples/dotnet/namespace-conventions.md` | Namespace structure |
| `examples/dotnet/ef-core-migrations.md` | EF Core migration discipline |
| `examples/dotnet/aspnetcore-patterns.md` | Controller/Service/Repository pattern |
| `examples/node/project-structure.md` | Express/Next.js folder conventions |
| `examples/node/async-patterns.md` | async/await, error handling |
| `examples/node/env-config.md` | dotenv, 12-factor config |
| `examples/python/django-patterns.md` | Django app structure and conventions |
| `examples/python/fastapi-patterns.md` | FastAPI schema and routing discipline |
| `examples/python/virtual-env.md` | venv/poetry, dependency pinning |

## Agents

| Agent | Use when |
|---|---|
| `code-reviewer` | Review code before opening a PR |
| `debugger` | Systematic bug investigation |
| `refactor-specialist` | Architecture-safe refactoring |
| `security-auditor` | OWASP-based security review |

## Commands

| Command | Does |
|---|---|
| `/review` | Run code-reviewer agent on current diff |
| `/debug` | Start structured debugging session |
| `/pre-pr` | Run pre-PR checklist before opening a PR |
| `/add-rule` | Guide through adding a new rule file |
