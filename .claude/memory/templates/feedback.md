---
name: Feedback and Corrections
description: Past mistakes, corrections, and validated approaches — prevents repeating the same errors
type: feedback
---

> Lead each entry with the rule itself, then **Why:** and **How to apply:** lines.
> Record both corrections AND confirmations of non-obvious approaches that worked.
> Keep this rolling — remove entries older than ~3 months if they're no longer relevant.

## Active Corrections

<!-- Example entries:

---
**Don't use merge commits in this repo — rebase only**
Why: Team agreed on linear history for easier bisect and rollback.
How to apply: Always use `git rebase` not `git merge`, and `--squash` on PRs.

---
**Don't mock the database in integration tests**
Why: We got burned when mocked tests passed but the real DB migration failed.
How to apply: Integration tests must use a real test database, not mocks.

---
**Stop summarizing what was just done at the end of every response**
Why: User can read the diff — the summary is redundant noise.
How to apply: End responses with the result or next step, not a recap.

-->

## Validated Approaches (non-obvious things that worked)

<!-- Example entries:

---
**Single bundled PR for this refactor was the right call**
Why: Splitting it would have caused merge conflicts between the pieces.
How to apply: For tightly coupled refactors in this codebase, bundle into one PR.

-->
