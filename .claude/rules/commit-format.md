# Commit Format

This project uses [Conventional Commits](https://www.conventionalcommits.org/).

## Format

```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

## Types

| Type | Use for |
|---|---|
| `feat` | New feature or user-visible capability |
| `fix` | Bug fix |
| `refactor` | Code restructuring with no behavior change |
| `docs` | Documentation updates only |
| `chore` | Build, dependencies, config, maintenance |
| `test` | Adding or updating tests |
| `perf` | Performance improvement |
| `style` | Formatting, whitespace — no logic change |

## Rules

- Description starts with **lowercase**
- Description is **imperative** ("add" not "added", "fix" not "fixed")
- No period at the end
- Keep the first line under **72 characters**
- Focus on **WHY**, not just what changed

## Scope (optional)

The scope is the module, component, or area affected:
```
feat(auth): add OAuth2 login support
fix(api): handle null response from payment gateway
chore(deps): update lodash to 4.17.21
```

## Body (optional)

Use the body to explain motivation or context that isn't obvious:
```
fix(orders): prevent duplicate order submission on slow connections

Double-click or slow network could submit the form twice. Added
a client-side lock that disables the submit button immediately
on first click and re-enables only on error response.
```

## Breaking Changes

Add a footer with `BREAKING CHANGE:` for breaking changes:
```
feat(api): change response format for /users endpoint

BREAKING CHANGE: The `name` field has been split into `firstName`
and `lastName`. All consumers must be updated.
```

## Examples

### Good
```
feat(dashboard): add weekly summary chart
fix(auth): redirect to login when token expires
chore: upgrade ESLint to v9
docs: add deployment guide for Railway
refactor(services): extract email sending to dedicated service
```

### Bad
```
Fixed stuff                        # no type, vague
feat: Added new feature.           # past tense, has period
FEAT: Update the Dashboard page    # uppercase type, vague
WIP                                # never commit WIP to main
fix everything                     # too vague, multiple changes
```

## One Change Per Commit

Never bundle unrelated changes in one commit. If you fixed a bug AND refactored something, that's two commits:
```
fix(cart): correct tax calculation for EU customers
refactor(cart): extract tax logic to TaxService
```
