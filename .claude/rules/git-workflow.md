# Git Workflow

Every change must follow this workflow. No exceptions.

## Branch Naming

```
feature/short-description    # New features
fix/short-description        # Bug fixes
chore/short-description      # Maintenance, deps, config
docs/short-description       # Documentation only
refactor/short-description   # Code restructuring, no behavior change
```

- Use kebab-case, keep it under 5 words
- One branch per feature or fix — never mix unrelated changes
- Branch from `main` (or `master`) — never from another feature branch

## Workflow Steps

### 1. Create branch
```bash
git checkout main
git pull origin main
git checkout -b feature/my-feature
```

### 2. Make changes
Work on the feature or fix. Stage only relevant files — never `git add .` blindly.

### 3. Verify before committing
Before every commit:
- Run the build (`npm run build`, `dotnet build`, `python -m pytest`, etc.)
- Run relevant tests
- Fix any issues before committing — never commit broken code

### 4. Commit
Follow `commit-format.md`. One logical change per commit.

### 5. Push
```bash
git push -u origin feature/my-feature
```

### 6. Open PR
- Title matches the commit format
- Description explains WHY, not just WHAT
- Link to relevant issue or ticket

### 7. Merge
- Prefer **squash merge** for features (keeps main history clean)
- Prefer **rebase merge** for small, atomic fixes
- Never use merge commits unless required by team policy
- Delete the branch after merge

### 8. Return to main
```bash
git checkout main
git pull origin main
```

## Rules

- **NEVER commit directly to `main`/`master`**
- **NEVER force-push to `main`/`master`**
- **NEVER skip the build/test check before committing**
- Keep PRs small — under 400 changed lines is a good target
- Stale branches (no activity for 2+ weeks) should be deleted or rebased

## PR Size Guide

| Lines changed | Signal |
|---|---|
| < 200 | Easy to review — ideal |
| 200–400 | Acceptable — add good description |
| 400–800 | Consider splitting |
| 800+ | Split before opening PR |
