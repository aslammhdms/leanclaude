# leanclaude

**Stop wasting tokens on a monolithic CLAUDE.md.**

`leanclaude` is a Claude Code project template that organizes your context into focused rule files instead of one giant instruction file — reducing per-session token consumption by ~65%.

## The Problem

Most developers start with a single `CLAUDE.md` file and keep adding to it. Over time it grows to thousands of lines. Claude reads **the entire file every single session**, even when 90% of the rules are irrelevant to the current task.

| Approach | Lines | ~Tokens/session |
|---|---|---|
| Monolithic CLAUDE.md | 5,000+ | ~13,000 |
| **leanclaude (this template)** | **~110 (index) + ~1,700 (rules)** | **~4,500** |
| **Savings** | | **~8,500 tokens/session (~65%)** |

At 20 sessions/day that's **~170,000 tokens saved daily** — and you get better responses because Claude isn't drowning in irrelevant context.

## How It Works

```
CLAUDE.md                  ← Short index only (~300 tokens)
.claude/
  rules/                   ← One file per topic, loaded as needed
    git-workflow.md
    debugging-investigation.md
    security-basics.md
    ... (10 universal rules)
    examples/              ← Pick your framework, delete the rest
      dotnet/
      node/
      python/
  agents/                  ← Specialized agents per task type
  commands/                ← Slash commands (/review, /debug, etc.)
  memory/                  ← Persistent memory across sessions
    MEMORY.md              ← Index of active memory files
    templates/             ← user, feedback, project, reference
```

**The key insight:** `CLAUDE.md` stays small forever because it's just an index. All the actual rules live in focused files named by topic. When you add a new convention, you add a new rule file — not another 50 lines to the main file.

## Quick Start

### Option 1: Use as GitHub Template (recommended)

1. Click **"Use this template"** → **"Create a new repository"**
2. Clone your new repo
3. Open it in Claude Code

### Option 2: Clone manually

```bash
git clone https://github.com/aslammhdms/leanclaude.git my-project
cd my-project
rm -rf .git
git init
```

### Setup Steps

1. **Update `CLAUDE.md`** — fill in your project overview, key file locations, and build commands
2. **Delete framework examples you don't need** — e.g. delete `.claude/rules/examples/node/` and `.claude/rules/examples/python/` if you're on .NET
3. **Update the rules index in `CLAUDE.md`** — remove rows for deleted rule files
4. **Customize rule files** — edit the universal rules to match your team's conventions
5. **Set up memory** — copy `.claude/memory/templates/` files into `.claude/memory/` and fill them in

## What's Included

### Universal Rules (language-agnostic)

| File | Covers |
|---|---|
| `git-workflow.md` | Branch naming, PR size, merge strategy |
| `commit-format.md` | Conventional Commits format + examples |
| `debugging-investigation.md` | Root cause first, 3-strike rule, debug report format |
| `escalation-protocol.md` | When Claude must stop and ask, BLOCKED format |
| `long-term-thinking.md` | Build for scale, no short-term patches |
| `adding-rules.md` | How to grow this rule system over time |
| `pre-pr-review.md` | Scope drift check, specialist checklist, confidence gates |
| `validate-after-changes.md` | Post-edit validation workflow |
| `security-basics.md` | OWASP Top 10 reference + mitigation patterns |
| `code-review-checklist.md` | Structured PR review checklist |

### Framework Examples

Pick the folder for your stack — delete the others.

| Folder | Covers |
|---|---|
| `examples/dotnet/` | Namespaces, EF Core migrations, ASP.NET Core patterns |
| `examples/node/` | Project structure, async patterns, environment config |
| `examples/python/` | Django patterns, FastAPI patterns, virtual environments |

### Agents

| Agent | What it does |
|---|---|
| `code-reviewer` | Full structured code review using the checklist rule |
| `debugger` | Systematic bug investigation — root cause first, escalates at 3 failures |
| `refactor-specialist` | Architecture-safe refactoring with validation plan |
| `security-auditor` | OWASP-based security scan with severity-ranked findings |

### Slash Commands

| Command | What it does |
|---|---|
| `/review` | Runs code-reviewer agent on current diff |
| `/debug` | Starts structured debugging session |
| `/pre-pr` | Runs pre-PR checklist before you open a PR |
| `/add-rule` | Guides you through adding a new rule file correctly |

### Memory System

Four memory types that persist across Claude Code sessions:

| Type | What to store |
|---|---|
| `user.md` | Your communication style, preferences, expertise level |
| `feedback.md` | Corrections — what Claude did wrong and what's right |
| `project.md` | Project context, constraints, active work area |
| `reference.md` | Glossary, file locations, external service endpoints |

See `docs/customizing.md` for how to use the memory system effectively.

## Token Savings — The Math

See `docs/token-savings.md` for the full breakdown. Summary:

- Claude reads CLAUDE.md **every session**, in full, before responding
- A 5,000-line file ≈ 13,000 tokens consumed before you type a single message
- This template's total context load ≈ 4,500 tokens
- Savings compound — 20 sessions/day × 8,500 tokens = **170,000 tokens/day**

## Customizing

See `docs/customizing.md` for step-by-step instructions on adapting this template to your project.

## Contributing

Found a universal rule that should be in this template? Open an issue using the **Rule Suggestion** template.

Found a bug or improvement? Open a **Bug Report**.

PRs welcome — see `.github/PULL_REQUEST_TEMPLATE.md` for the checklist.

## License

MIT — see [LICENSE](LICENSE)
