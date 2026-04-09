# Long-Term Thinking

Every fix and addition must be implemented as if the project will grow significantly. Short-term patches that work today but create future pain are not acceptable.

## The Rule

When fixing a bug or adding a feature, always choose the approach that fits the project's long-term architecture — even if a simpler shortcut would work right now. If the proper solution is significantly larger in scope, **flag it to the user before proceeding**.

## What This Means in Practice

### Architecture
- Business logic belongs in the **service/domain layer**, not in controllers or route handlers
- Never duplicate logic across modules — extract a shared utility or service method
- Don't use inline DB queries in controllers when a service method should own that query

### Database / Schema
- Design entities with the full data model in mind — add proper foreign keys and indexes from the start
- Name columns and tables correctly now — renaming later cascades everywhere
- Never work around schema issues with raw SQL patches or application-level hacks

### Permissions & Access Control
- Every new endpoint gets the full auth/permission checklist from day one
- Never hardcode role strings where a permission system should be used
- Branch/tenant filtering must be applied from day one, not retrofitted later

### No Hardcoded / Magic Values
- Status values belong in enums or constants, not inline strings (`"active"`, `"pending"`)
- No hardcoded IDs, environment-specific values, or role names in business logic

## Completeness Principle

When multiple implementation approaches exist, rate each option:

| Score | Meaning |
|---|---|
| 10 | Handles all edge cases, fully integrated, production-ready |
| 7 | Happy path works, known gaps are documented |
| 5 | Shortcut — defers significant work to a future session |
| 3 | Partial — breaks under foreseeable conditions |

**Options rated 5 or below must be flagged to the user before proceeding.**

## No-Shortcut Exceptions

A minimal fix **is** acceptable when:
- It is a genuine single-value or single-condition correction
- It follows existing patterns exactly
- No architecture, schema, or permission shortcuts are involved

If meeting those conditions requires a shortcut, implement the full proper solution instead.

## Before Every Fix or Addition, Ask

> "If this module had 10× more users and records — would this implementation still hold up cleanly?"

If no: implement the proper solution. If that's significantly larger than expected, tell the user and align on scope before starting.
