# Principles Audit — `<project-name>`

Scope: `<full project | specific module/feature>`

## Summary

One short paragraph: overall state of the codebase against the five principles, and the single most important thing to fix first.

## High priority

For each finding:

### `<short title>`
- **Location:** `path/to/file.ts:line` (or function/component name)
- **Principle violated:** Clean Code / SOLID (specify letter) / DRY / YAGNI / KISS
- **Why it matters:** concrete consequence — what breaks, what's hard to change, what risk it creates
- **Fix:** specific, actionable — not "refactor this," but what the resulting code should look like

## Medium priority

(Same format as High.)

## Low priority

(Same format, can be more terse — one line each is fine if the fix is obvious.)

## Not flagged (explicitly out of scope for this audit)

Briefly note anything that looked like it might be a violation but was deliberately not flagged, and why (per the "What NOT to flag" section of the skill) — this prevents the same non-issue from being re-raised in a future audit.
