Run the pre-PR checklist before opening a pull request.

Usage:
  /pre-pr              — run the full checklist on current branch vs main
  /pre-pr feature/xyz  — run against a specific branch

Steps:
1. Run `git diff --stat origin/main` (or the specified base branch) and display the changed files
2. Ask the user to confirm the original task/ticket this PR addresses
3. Work through each gate in `.claude/rules/pre-pr-review.md`:
   - Step 1: Scope drift check — do changed files match the stated task?
   - Step 2: Plan completion — are all requirements addressed?
   - Step 3: Specialist checklist — security, data, performance, testing
   - Step 4: Confidence gates — rate confidence on any findings
   - Step 5: PR quality score

4. Output the final verdict:

```
PRE-PR REVIEW
=============
Branch: [branch name]
Files changed: [count] ([+lines] / [-lines])

Scope:       CLEAN / SCOPE CREEP / MISSING REQUIREMENTS
Security:    PASS / [N issues found]
Performance: PASS / [N issues found]
Tests:       PASS / MISSING
Quality score: [N]/10

Verdict: ✅ Ready to open PR / ❌ Fix these issues first:
  - [list of blocking items]
```

5. If verdict is ✅, offer to create the PR with `gh pr create`
