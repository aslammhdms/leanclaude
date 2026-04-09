# Token Savings — The Full Breakdown

## Measurement Methodology

Token counts are estimated using the cl100k_base tokenizer (used by GPT-4, close enough to Claude's tokenizer for estimation purposes). Actual Claude token counts may vary ±10%.

Rule of thumb: **1 token ≈ 4 characters** for English prose, **1 token ≈ 3 characters** for code.

## Baseline: Monolithic CLAUDE.md

A typical project that has accumulated rules for 12 months:

| Section | Lines | ~Tokens |
|---|---|---|
| Project overview | 50 | 300 |
| Coding standards | 200 | 1,200 |
| Git workflow | 80 | 480 |
| Database/ORM rules | 150 | 900 |
| Security rules | 120 | 720 |
| API conventions | 100 | 600 |
| Testing standards | 80 | 480 |
| Deployment instructions | 60 | 360 |
| UI/frontend patterns | 150 | 900 |
| Team-specific rules | 200 | 1,200 |
| Historical notes (never cleaned up) | 300 | 1,800 |
| Miscellaneous accumulation | 500 | 3,000 |
| **Total** | **~2,000** | **~12,000** |

Real-world observation: a 5,000-line CLAUDE.md clocks in at ~13,000 tokens.

## leanclaude Approach

| Component | Lines | ~Tokens | Loaded |
|---|---|---|---|
| `CLAUDE.md` (index) | 110 | 300 | Every session |
| `MEMORY.md` (index) | 50 | 150 | Every session |
| Active memory files (avg) | 100 | 300 | Every session |
| All rule files combined | 1,700 | 4,200 | As needed |
| **Session minimum** | **260** | **750** | — |
| **Session maximum** | **1,960** | **4,950** | — |

Even at maximum (all rules loaded), the cost is **~5,000 tokens** — less than half of the monolithic approach.

## Per-Session Savings

| Scenario | Monolithic | leanclaude | Saved |
|---|---|---|---|
| Simple question | 13,000 | 750 | **12,250** |
| Complex coding task (all rules loaded) | 13,000 | 4,950 | **8,050** |
| Average session | 13,000 | 2,500 | **10,500** |

## Projected Savings by Usage Volume

| Sessions/day | Tokens saved/day | Tokens saved/month |
|---|---|---|
| 5 | ~52,500 | ~1,575,000 |
| 20 | ~210,000 | ~6,300,000 |
| 50 | ~525,000 | ~15,750,000 |
| 100 | ~1,050,000 | ~31,500,000 |

## The Quality Effect

Token savings are only part of the story. Context window efficiency also improves response quality:

- **Less noise** — Claude is not processing irrelevant rules about database migrations when you're asking about a UI component
- **Less confusion** — contradictory rules accumulated over time in a monolithic file dilute Claude's behavior
- **Better focus** — when a rule file is loaded for a specific task, it gets full attention rather than being buried in 13,000 tokens

## How to Measure Your Own Savings

1. Count lines in your current `CLAUDE.md`: `wc -l CLAUDE.md`
2. Estimate tokens: `lines × 6` (conservative estimate for mixed prose/code)
3. Compare to leanclaude total: `~4,500 tokens` average
4. Calculate: `((your_tokens - 4500) / your_tokens) × 100` = your % reduction
