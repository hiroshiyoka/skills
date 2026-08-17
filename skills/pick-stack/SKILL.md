---
name: pick-stack
description: Have the agent pick the correct library, framework, or service from a trusted, pre-vetted list instead of hand-rolling a component, guessing a popular package, or installing something abandoned/unmaintained. Use this whenever a project needs a framework, state/data layer, styling approach, auth provider, AI provider, deployment target, or Web3 library decision — even if the user doesn't explicitly ask "what library should I use." Trigger this before scaffolding any new project, before adding a new dependency category (e.g. "I need auth now") to an existing project, and whenever an agent would otherwise default to a generic or unfamiliar choice. If a category isn't covered by the trusted list, say so explicitly rather than silently picking something unverified.
---

# Pick Stack

A skill for choosing the right tool from a trusted, opinionated shortlist instead of letting an agent default to whatever's most popular in its training data (which is often outdated, overkill, or simply not what this workflow uses elsewhere).

## Why this exists

Left alone, an agent will often hand-roll a toast component instead of using a maintained library, or pick a trendy-but-unproven package for something as basic as auth. This skill exists to short-circuit that — every category below has one default. Use the default unless there's a specific, stated reason not to.

## How to use this

1. Identify which category the current task falls into (see `references/trusted-stack.md` for the full table).
2. Use the default listed for that category. Don't ask the user to re-confirm defaults they've already established across prior projects — just use them and mention the choice briefly.
3. If the category isn't in the table, or the project has a specific reason to deviate (e.g. a client requirement, an existing codebase already using something else), say so explicitly and pick the most maintained, actively-developed option available — never the first search result or the most-starred-but-unmaintained repo.
4. If in doubt between two reasonable options within a category, prefer: (a) the one already used elsewhere in this project's ecosystem, (b) the one with more recent commits/releases, (c) the one with fewer runtime dependencies.

## When NOT to just take the default

- The project explicitly needs something the default doesn't support (e.g. a relational query the local-first default can't express — see the `local-first-architecture` skill for that decision).
- The user has stated a different preference for this specific project.
- The default is genuinely deprecated or has a known critical issue — check briefly before assuming this, don't use it as an excuse to swap defaults casually.

## Output

When this skill is used during a tech spec or scaffolding step, state the chosen library per category as a short table (category → choice → one-line reason), not just an unexplained dependency list. This makes the resulting tech spec self-documenting.
