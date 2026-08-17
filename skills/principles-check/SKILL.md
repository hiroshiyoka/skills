---
name: principles-check
description: Audit an existing codebase against Clean Code, SOLID, DRY, YAGNI, and KISS, and produce a prioritized, self-contained plan of what to fix and in what order. Use this whenever the user asks for a code review, wants to "clean up" or "refactor" a project, asks whether their code follows good practices, or before a project is handed off / open-sourced / submitted (e.g. thesis code, a portfolio project). Also use it proactively after a large feature build when the user asks "does this look okay" without specifying exactly what to check. Don't use this for a single small file or function — it's meant for a project-level or module-level pass, not a one-line lint check.
---

# Principles Check

A skill for auditing real code against five concrete principles — Clean Code, SOLID, DRY, YAGNI, KISS — and turning violations into a prioritized, actionable plan instead of a vague "this could be cleaner" comment.

## Why this exists

"Follow clean code" is not actionable feedback for an agent or for a human doing the fix. This skill exists to make the audit concrete: each principle below has specific, checkable signals to look for, and every finding in the output must point to a specific file/function and a specific fix — never a general impression.

## How to run the audit

1. **Scope it.** Confirm with the user (or infer from context) whether this is a full-project audit or a specific module/feature. Don't silently audit the entire codebase if the user asked about one feature.
2. **Walk the codebase against each principle** using the concrete signals in `references/principle-signals.md` — don't just skim for "code smells" in general; check each principle deliberately.
3. **Record findings** as you go: file/location, which principle it violates, why it matters (not just "this is bad"), and a concrete fix.
4. **Prioritize.** Not everything found needs fixing immediately. Rank findings by actual impact (see Prioritization below) rather than listing them in the order they were found.
5. **Output** using `templates/audit-report.md` — a report the user (or a build agent) can act on directly, ordered by priority, each item self-contained enough to execute without re-reading the whole codebase.

## Prioritization

Rank findings into three tiers, don't just list everything as equally urgent:

- **High** — actively causes bugs, makes the code hard to change safely, or blocks a stated goal (e.g. thesis submission needing consistent formatting, a codebase about to be handed to another engineer).
- **Medium** — real violation, no correctness risk, but adds friction to future changes (e.g. duplicated logic across 3+ files).
- **Low** — stylistic or minor; worth noting but not worth blocking anything on.

Don't inflate low-impact style nitpicks into "High" just to pad the report — a short, accurate report is more useful than an exhaustive one.

## What NOT to flag

- Don't flag YAGNI violations for abstractions that are already load-bearing (used in 3+ places) — that's the point where DRY should have kicked in, not over-engineering.
- Don't flag SOLID violations in code that's genuinely simple and unlikely to grow (a one-off script, a local-first app's small data layer) — SOLID matters most where the code will actually change and grow. Forcing SOLID onto small, stable code is itself a YAGNI violation.
- Don't flag naming or style issues that are just a different-but-consistent convention already used throughout the project — consistency matters more than matching an external style guide.

## Output

Always produce the report using `templates/audit-report.md`. If the user wants the findings turned into an actual fix (not just a report), hand the relevant High-priority items to a build agent as their own self-contained task — same principle as the `spec-suite` agent build prompt: specific enough that no further clarification is needed.
