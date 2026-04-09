# Debugging and Investigation

Every bug fix must start with understanding the root cause. Fixing symptoms creates whack-a-mole debugging and makes the next bug harder to find.

## The Iron Law

**NO FIX WITHOUT ROOT CAUSE INVESTIGATION FIRST.**

Never write a fix until you can state: "The root cause is X at file:line, proven by Y."

## 5-Phase Workflow

### Phase 1: Root Cause Investigation

Before touching any code:
- Read the full error message and stack trace
- Trace the code path from symptom back to potential causes
- Check recent changes: `git log --oneline -20 -- <affected-files>` — if it's a regression, the cause is in the diff
- Reproduce the bug deterministically before forming any hypothesis

Output a specific, testable claim: **"Root cause hypothesis: [X] at [file:line]"**

### Phase 2: Pattern Analysis

Check which known pattern applies:
- **Race condition** — intermittent, timing-dependent, async/await issue
- **Null propagation** — NullReferenceException / undefined, missing null guard
- **State corruption** — ORM tracking issue, transaction not committed
- **Permission bypass** — missing auth check, wrong role string
- **Integration failure** — external API timeout, connection issue
- **Configuration drift** — works locally, fails in another environment

### Phase 3: Hypothesis Testing

- Add a temporary log or assertion at the suspected root cause and reproduce the bug
- If the hypothesis is wrong: form a new one based on evidence, not guessing

**3-Strike Rule:** If 3 hypotheses all fail, STOP. Do not form a fourth guess. Instead:
1. Add diagnostic logging and wait for more evidence, OR
2. Escalate to the user (see `escalation-protocol.md`)

**Red flags — stop immediately if:**
- Writing a fix before tracing the data flow
- Each fix reveals a new problem elsewhere (fixing the wrong layer)
- Reaching for "quick fix for now" — there is no "for now"

### Phase 4: Implementation

- Fix the **root cause**, not the symptom
- Minimal diff — fewest files, fewest lines
- Do not refactor adjacent code during a bug fix
- Write a regression test that **fails** before the fix and **passes** after it
- **If the fix touches more than 5 files:** Stop and confirm blast radius with user

### Phase 5: Verification

Fresh verification is mandatory:
- Reproduce the original bug scenario and confirm it no longer occurs
- Run the full build and test suite
- Never say "this should fix it" — prove it

## Debug Report Format

After every fix, output:

```
DEBUG REPORT
Symptom:         [what the user reported]
Root cause:      [file:line — exact cause]
Fix:             [what was changed and why]
Evidence:        [how you verified the root cause]
Regression test: [file:line of the new test, or "none — not testable"]
Status:          DONE / DONE_WITH_CONCERNS / BLOCKED
```

## Hard Rules

- Never apply a fix you cannot verify
- Never say "this should fix it" — verify and prove it
- 3+ failed hypotheses: STOP, escalate
- Fix touches more than 5 files: ask user before continuing
- Always run the build after the fix before reporting done
