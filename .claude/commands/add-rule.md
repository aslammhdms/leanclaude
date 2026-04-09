Guide through adding a new rule file to the project following the adding-rules.md protocol.

Usage:
  /add-rule            — start an interactive rule creation session
  /add-rule "topic"    — start with a pre-filled topic

Steps:
1. Ask the user:
   - What pattern or situation prompted this rule?
   - Is this truly reusable, or is it specific to one screen/module?
   - Does an existing rule already cover this? (check `.claude/rules/` first)

2. If an existing rule should be updated instead of creating a new one, show the user the relevant section and propose an edit.

3. If a new rule is warranted, collect:
   - Rule name (kebab-case, 2-4 words): e.g., `api-versioning.md`
   - One-sentence description of what it governs
   - The rule itself (what to DO and what NOT to do)
   - Why this rule exists (the failure or situation that prompted it)
   - An example (code snippet or config showing correct usage)

4. Generate the rule file content using this template:

```markdown
# [Rule Title]

[One sentence describing what this rule governs and why it matters.]

## Rule
[What to DO and what NOT to do]

## Why
[The failure, bug, or situation that prompted this rule]

## Example
[Code or config snippet showing correct usage]
```

5. Write the file to `.claude/rules/[rule-name].md`

6. Add a row to the rules index table in `CLAUDE.md`:
   `| \`[rule-name].md\` | [one-line description] |`

7. Confirm: "Rule added. Remember to commit it together with the work that prompted it."
