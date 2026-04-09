---
name: Project Context
description: Project-specific context, constraints, active work area, and key decisions
type: project
---

> Lead each entry with the fact or decision, then **Why:** and **How to apply:** lines.
> Convert relative dates to absolute dates (e.g., "Thursday" → "2026-04-10").
> Project memories decay fast — review and prune monthly.

## Project Overview

- **Name**: [Project name]
- **Purpose**: [One sentence — what this system does and for whom]
- **Stack**: [Language, framework, database, hosting]
- **Repo**: [URL or local path]

## Key Constraints

<!-- Document hard constraints that affect every decision:

- **Compliance**: [e.g., GDPR — no PII in logs]
- **Performance SLA**: [e.g., API responses must be < 200ms p95]
- **Backward compatibility**: [e.g., API v1 must remain stable until 2027-01]
- **Deployment window**: [e.g., deploys only on Thursdays, 2AM–4AM UTC]

-->

## Active Work Area

<!-- What's currently being worked on — update each session:

**Current sprint goal**: [e.g., complete the payment module by 2026-04-30]
**Active feature**: [e.g., Stripe integration — PR #142 in review]
**Known blockers**: [e.g., waiting on design approval for the checkout flow]

-->

## Key Decisions

<!-- Record architectural or process decisions and their rationale:

---
**We use server-side rendering for the dashboard, not a SPA**
Why: SEO requirements from marketing team; SPA was considered and rejected in 2026-01.
How to apply: Don't suggest React/Vue rewrites for the dashboard — it's intentional.

---
**Database is read-only for the reporting service**
Why: Prevents accidental writes from a high-traffic read service.
How to apply: Reporting queries go through a read replica connection string.

-->

## Technical Debt to Avoid

<!-- Areas to leave alone unless specifically tasked:

- [e.g., The legacy `LegacyReportService` — do not refactor, scheduled for replacement Q3]
- [e.g., The `UserSync` background job — fragile, touch only if explicitly asked]

-->
