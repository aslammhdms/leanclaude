# Customizing leanclaude for Your Project

## Step 1: Start from the Template

Click **"Use this template"** on GitHub, or clone and re-init:

```bash
git clone https://github.com/aslammhdms/leanclaude.git my-project
cd my-project
rm -rf .git && git init && git checkout -b main
```

## Step 2: Update CLAUDE.md

Fill in the three placeholders at the top:

1. **Project overview** — 2-3 sentences: what the system does, who uses it, the tech stack
2. **Key file locations** — entry point, config, database layer
3. **Build & run commands** — your actual commands

Remove rows from the rules index table for rule files you delete (Step 3).

## Step 3: Pick Your Framework, Delete the Rest

Only keep the framework examples that match your stack:

```bash
# If you're on .NET, delete Node and Python examples:
rm -rf .claude/rules/examples/node/
rm -rf .claude/rules/examples/python/

# If you're on Node.js:
rm -rf .claude/rules/examples/dotnet/
rm -rf .claude/rules/examples/python/

# If you're on Python:
rm -rf .claude/rules/examples/dotnet/
rm -rf .claude/rules/examples/node/
```

Then update the "Framework-Specific Rules" section in `CLAUDE.md` to remove the deleted rows.

## Step 4: Customize the Universal Rules

The 10 universal rule files are starting points — adapt them to your team:

| Rule | Common customizations |
|---|---|
| `git-workflow.md` | Change branch naming format, PR size limit, merge strategy |
| `commit-format.md` | Add project-specific scopes, change allowed types |
| `debugging-investigation.md` | Add project-specific pattern analysis (known failure modes) |
| `escalation-protocol.md` | Adjust the 3-attempt limit, add project-specific BLOCKED recipients |
| `security-basics.md` | Add compliance requirements (GDPR, PCI-DSS, HIPAA) |

## Step 5: Set Up Memory

Copy templates and fill them in:

```bash
cp .claude/memory/templates/user.md .claude/memory/user.md
cp .claude/memory/templates/project.md .claude/memory/project.md
# etc.
```

Edit each file to reflect your actual profile and project context. Add entries to `MEMORY.md`.

## Step 6: Add Your Own Rules

As you work, patterns will emerge that aren't covered by the universal rules. Use `/add-rule` to add them:

```
/add-rule
```

Or follow the format in `.claude/rules/adding-rules.md` manually.

Good candidates for project-specific rules:
- Your database schema conventions
- Your API versioning strategy
- Your error handling patterns
- Your logging format
- Your specific auth/permission system
- Integration patterns with external services you use

## Step 7: Commit and Push

```bash
git add .
git commit -m "feat: initialize project from leanclaude template"
git remote add origin https://github.com/yourusername/your-project.git
git push -u origin main
```

## Ongoing Maintenance

**Add a rule when:**
- You correct Claude for the same mistake twice
- A new pattern is established that should be followed everywhere
- A new external system is integrated

**Update a rule when:**
- A code example in the rule is outdated
- A new edge case is discovered that the rule doesn't cover
- The rule turns out to be wrong

**Update memory when:**
- Your preferences change
- A project decision is made that affects future sessions
- A correction should persist across sessions

**Trim rules periodically:**
- Remove rules for patterns that no longer exist in the codebase
- Merge rules that have grown to cover the same topic
- A quarterly review of `.claude/rules/` takes 15 minutes and saves tokens year-round
