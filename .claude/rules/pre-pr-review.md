# Pre-PR Review Checklist

Run this before opening any PR. Catches scope drift, specialist concerns, and cross-cutting patterns that automated validators miss.

## Step 1: Scope Drift Check

Did you build exactly what was requested — no more, no less?

Run: `git diff --stat origin/main`

Check against the original request:
- **SCOPE CREEP**: unrelated files changed, unrequested features added, "while I was in there" refactors
- **MISSING REQUIREMENTS**: requirements not addressed, partial implementations
- **CLEAN**: changed files match stated intent

If SCOPE CREEP or MISSING: resolve before creating the PR.

## Step 2: Plan Completion Audit

If a plan or task list existed for the work, cross-reference every item against the diff:

| Status | Meaning |
|---|---|
| DONE | Clear evidence in the diff |
| PARTIAL | Some work exists but incomplete |
| NOT DONE | No evidence in the diff |
| CHANGED | Different approach taken, same goal achieved |

If any HIGH-priority items are NOT DONE: implement them, or explicitly confirm with user they are intentionally deferred.

## Step 3: Specialist Checklist

### Security
- Every new POST/PUT/DELETE action has CSRF protection (`ValidateAntiForgeryToken` or equivalent)
- Every new controller/route has authentication middleware applied
- Every state-changing handler validates permissions server-side before acting
- No user input passed to SQL/queries without parameterization
- No user input rendered as raw HTML (XSS)

### Data / Migrations
- New entities have proper indexes where needed
- No raw SQL workarounds instead of migrations
- Foreign keys added where appropriate

### Performance
- No N+1 queries in loops (use `.include()`, `select_related`, or batch queries)
- No unbounded queries (no `findAll()` / `ToList()` without filters on large tables)
- No synchronous blocking I/O in async handlers

### Testing
- New bug fix has a regression test
- New feature has at least a smoke test (build passes, no runtime exception on load)

## Step 4: Confidence Gates

For any finding, rate your confidence before reporting it:

| Score | Meaning | Action |
|---|---|---|
| 9-10 | Verified by reading specific code | Report normally |
| 7-8 | High confidence pattern match | Report normally |
| 5-6 | Moderate — could be false positive | Report with caveat |
| 3-4 | Low confidence | Only report if critical |
| 1-2 | Speculation | Do not report |

Never say "this looks fine" without evidence.

## Step 5: PR Quality Score

```
PR Quality = 10 - (critical_issues × 2) - (medium_issues × 0.5)
```

- **8+**: ready to ship
- **5-7**: fix critical issues first, note medium issues in PR description
- **Below 5**: do not create PR — fix first

## What to Fix vs What to Ask

- **Auto-fix**: mechanical issues — missing CSRF token, missing null guard, dead code
- **Ask first**: anything that changes behavior, touches shared components, or has blast radius beyond the current PR
