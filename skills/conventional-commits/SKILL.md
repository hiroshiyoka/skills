---
name: conventional-commits
description: Write commit messages that follow the Conventional Commits format (type(scope): description) consistently across every repo and skill catalog. Use this whenever the user asks to commit changes, write a commit message, or when an agent is about to run `git commit` as part of a larger task. Also use it to review/rewrite an existing commit message the user drafted, or to help decide which type (feat/fix/chore/docs/refactor/etc.) a change actually falls under when it's ambiguous.
---

# Conventional Commits

A skill for writing commit messages in a consistent, parseable format across every project — instead of an inconsistent mix of "update stuff", "wip", and one-off phrasing that makes history hard to scan later.

## Format

```
<type>(<optional scope>): <short description>

<optional body>

<optional footer>
```

- **type** — required, one of the types below.
- **scope** — optional, a short noun for the affected area (e.g. `spec-suite`, `readme`, `auth`). Omit if the change is repo-wide or scope would be redundant.
- **short description** — imperative mood, lowercase, no trailing period (e.g. "add principles-check skill", not "Added the principles-check skill.").

## Types

| Type | When to use it |
|---|---|
| `feat` | A new feature, skill, page, or capability — something a user/reader can now do that they couldn't before. |
| `fix` | A bug fix — correcting something that was actually broken or wrong. |
| `docs` | Documentation-only changes (README, SKILL.md content edits that don't change behavior, comments). |
| `chore` | Maintenance that doesn't affect behavior or docs — config files, `.gitignore`, `.gitattributes`, license, dependency bumps. |
| `refactor` | Restructuring existing code/content without changing what it does (e.g. reorganizing a SKILL.md's sections). |
| `style` | Formatting-only changes (whitespace, line breaks) with zero content/logic change. |
| `test` | Adding or fixing tests. |

## Deciding the type when it's ambiguous

- **New skill or new file that didn't exist before → `feat`**, even if the file is "just" a markdown document — for a skill catalog, a new skill is a new capability, not documentation.
- **Editing an existing SKILL.md's instructions/behavior → `feat` if it changes what the skill does, `docs` if it only clarifies wording without changing behavior.**
- **Adding `.gitattributes`, `.gitignore`, `LICENSE` → `chore`**, not `feat` — these don't add a capability, they configure the repo.
- If a commit touches multiple types of change at once, split it into separate commits rather than picking one type that only half-fits. A commit should represent one coherent change.

## Examples (good)

```
feat: add principles-check skill
feat(spec-suite): add naming stage to workflow
fix(pick-stack): correct outdated Groq API reference
docs: clarify local-first decision checklist
chore: add MIT license
chore: update .gitattributes for linguist documentation tag
refactor(readme): reorder skill reference table
```

## Examples (avoid)

```
update stuff              # no type, no useful description
Fixed bug                 # capitalized, past tense, no type, vague
feat: Added New Skill.    # wrong case, past tense, trailing period
wip                        # not a real description of the change
```

## Body and footer (optional)

Use a body when the "why" isn't obvious from the short description alone — for example, explaining why a default was changed in `pick-stack`. Keep it to a few lines, wrapped, in plain prose. Use a footer only for things like `BREAKING CHANGE:` notes or issue references — most commits in a personal/skill-catalog repo won't need one.

## When committing on behalf of the user

If asked to commit a change, propose the exact commit message using this format before running `git commit`, so the user can adjust the type/scope/wording if needed rather than discovering it in `git log` afterward.
