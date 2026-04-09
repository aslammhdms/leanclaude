# Validate After Changes

After completing any task that creates or modifies source files, you MUST run validation before considering the task done.

## Validation Workflow

1. Run the build for your stack:
   - Node: `npm run build` or `yarn build`
   - .NET: `dotnet build`
   - Python: `python -m pytest` or `mypy .`
2. Run relevant tests for changed modules
3. Check that imports/exports are intact — no missing references
4. Confirm no regressions in related modules

## Report Format

After validation, present results to the user:

```
VALIDATION REPORT
Build:   PASS / FAIL
Tests:   PASS / FAIL / SKIPPED
Issues:  [list any warnings or errors]
Status:  DONE / DONE_WITH_CONCERNS / BLOCKED
```

## Rules

- **Do NOT present changes as complete until validation passes**
- **Do NOT auto-fix issues without asking** — present the report and let the user decide
- If validation fails: report what failed, propose a fix, wait for user response
- Only after the user responds (fix or skip) is the task considered complete

## Scope Drift Check (run before validation)

Before running the validator, verify you built exactly what was requested:

1. Run `git diff --stat origin/main` and compare changed files against the original request
2. If unrelated files were changed: **remove them before proceeding**
3. If requirements from the original request were not addressed: **implement them before closing the task**

## Rule Evaluation (run alongside validation)

After completing any task, evaluate whether rules should be added or updated:

- Did you use a new reusable pattern not covered by existing rules?
- Did an existing rule prove wrong or incomplete?
- Did the user say "always do X" or "never do Y"?

If yes: propose the rule change before closing the task. See `adding-rules.md`.

## Exception

Changes to documentation-only files (`.md`) or config files (`.json`, `.yml`) that don't affect code behavior do not require a build validation — but do still require a scope drift check.
