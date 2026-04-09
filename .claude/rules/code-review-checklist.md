# Code Review Checklist

A structured checklist for reviewing any PR. Work through each section before approving.

## 1. Correctness

- [ ] Does the code do what the ticket/task describes?
- [ ] Are edge cases handled? (null, empty, zero, max values, concurrent access)
- [ ] Are error cases handled and surfaced appropriately?
- [ ] Does it handle the unhappy path, not just the happy path?

## 2. Security

Cross-reference `security-basics.md` for each change:

- [ ] No user input concatenated into SQL queries
- [ ] No user input rendered as raw HTML
- [ ] No secrets hardcoded or logged
- [ ] Authentication and authorization checked on every state-changing endpoint
- [ ] Input validated at the boundary

## 3. Performance

- [ ] No N+1 queries (loading related data in a loop)
- [ ] No unbounded queries (fetching entire tables without filters/pagination)
- [ ] No synchronous blocking I/O in async code paths
- [ ] No expensive operations inside loops that could be batched

## 4. Code Quality

- [ ] Naming is clear and consistent with the rest of the codebase
- [ ] No dead code or commented-out code left behind
- [ ] No magic numbers or hardcoded strings — use constants or enums
- [ ] Logic is in the right layer (business logic in service layer, not controllers/routes)
- [ ] No duplicate logic that already exists elsewhere

## 5. Tests

- [ ] New logic has tests
- [ ] Bug fixes have a regression test (test that fails before fix, passes after)
- [ ] Tests are meaningful — testing behavior, not implementation details
- [ ] No tests that can only pass in one specific environment

## 6. Breaking Changes

- [ ] Are any public API contracts changed? (endpoint signatures, response shapes, event formats)
- [ ] Are database schema changes backward-compatible?
- [ ] Are breaking changes documented in the PR description?

## 7. Documentation

- [ ] Public APIs have usage documentation
- [ ] Complex or non-obvious logic has inline comments explaining WHY, not just WHAT
- [ ] README updated if new setup steps or dependencies were added

## Verdict

| Verdict | Meaning |
|---|---|
| ✅ LGTM | All checks pass — ready to merge |
| 🔶 LGTM with nits | Minor suggestions, not blocking |
| ❌ Request Changes | Blocking issues found — list them clearly |

When requesting changes: be specific. Point to the exact line and explain what needs to change and why — not just that it's wrong.
