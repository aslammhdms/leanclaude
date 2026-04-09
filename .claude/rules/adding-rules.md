# Adding and Updating Rules

Rules are the backbone of lean context. Add them proactively — but only when they earn their token cost.

## When to Add a NEW Rule

Add a new rule file when ANY of these are true:

- A bug or mistake happened because a pattern wasn't documented
- A new reusable pattern is implemented (new integration, auth flow, DB pattern, UI pattern)
- The user says "remember this", "always do X", or "never do Y"
- The same pattern appears in 2+ places within one session
- A framework limitation or workaround is discovered that will apply again
- A new external system, API, or service is integrated

## When to UPDATE an Existing Rule

Update an existing rule file when:

- A new module or component is added → update the relevant rule's examples
- A code example in an existing rule is wrong, incomplete, or outdated
- An existing rule doesn't cover a new edge case just handled
- A rule contradicts what was just built (the rule is wrong — fix it)

## When NOT to Add a Rule

- One-off fixes specific to a single screen with no reuse potential
- Module-specific business logic (use memory files instead)
- Patterns already fully covered by an existing rule
- Historical notes or session progress logs

## Rule File Format

```markdown
# Rule Title

One sentence describing what this rule governs and why it matters.

## Rule
[The actual rule — what to DO and what NOT to do]

## Why
[The failure, bug, or situation that prompted this rule]

## Example
[Code or config snippet showing correct usage]
```

## Naming Conventions

- Use kebab-case: `my-new-pattern.md`
- Name by topic, not by when it was added: `select2-dropdowns.md` not `jan-2026-dropdown-fix.md`
- Keep names short and scannable (2-4 words)

## Process

When a trigger is spotted mid-session:

1. Propose the rule to the user:
   > "I noticed a pattern worth [adding / updating in `rule-file.md`]:
   > **[rule name]** — [one-line summary]
   > Should I add this?"

2. Wait for confirmation before creating the file

3. After approval:
   - Create or edit the rule file in `.claude/rules/`
   - Add a row to the `CLAUDE.md` rules index table
   - Commit together with the main work at the end of the session

## After Every Session

Ask: **"Would knowing X save 5+ minutes in a future session?"**

If yes → save to memory (see `.claude/memory/`)
If it's a reusable pattern → add a rule file

Rules cover reusable patterns. Memory covers session-specific discoveries and user preferences.
