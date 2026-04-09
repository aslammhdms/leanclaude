# Escalation Protocol

Bad work is worse than no work. Knowing when to stop and ask is a core skill — not a weakness.

## The 3-Attempt Rule

If the same task has been attempted 3 times without success: **STOP and escalate.** Do not attempt a fourth approach without user input.

This applies to:
- Debugging: 3 failed hypotheses (see `debugging-investigation.md`)
- Build errors: 3 failed fix attempts
- Any repeated failure pattern

## When to Stop and Ask (vs Proceed)

**Stop and ask when:**
- You've hit the 3-attempt limit
- A fix would touch more than 5 files unexpectedly
- The scope of work is significantly larger than what was requested
- You're uncertain about a security-sensitive change (auth, permissions, data access)
- The task requires a destructive operation (DROP, DELETE, force push, reset --hard)
- You discover unexpected state — unfamiliar files, schema mismatches, conflicting config

**Proceed without asking when:**
- The path forward is clear and matches an established pattern in the rules
- The change is small, reversible, and local
- You have verified the root cause and the fix is minimal

## BLOCKED Format

When escalating, always use this format — never just say "I'm stuck":

```
STATUS: BLOCKED
REASON: [1-2 sentences — what specifically is preventing progress]
ATTEMPTED: [what you tried, in order]
RECOMMENDATION: [what the user should do next or decide]
```

## Status Vocabulary

Use these exact terms when reporting task completion:

| Status | Meaning |
|---|---|
| `DONE` | All steps completed, every claim has evidence |
| `DONE_WITH_CONCERNS` | Completed, but there are issues the user should know about |
| `BLOCKED` | Cannot proceed — user input required |
| `NEEDS_CONTEXT` | Missing information that only the user can provide |

Never say "this should work" or "I think this is fine" without evidence.

## Blast Radius Rule

Before taking any action that affects more than what was asked:

1. State what was requested
2. State what the actual scope of change turned out to be
3. Ask for confirmation before proceeding

Example: user asks to fix a button label → fix requires changing a shared component used in 8 views → stop and confirm before touching the shared component.
