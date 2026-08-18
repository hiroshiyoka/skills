---
name: name-it
description: Generate a shortlist of single-word, brand-friendly project name candidates from a short description of what the project does. Use this whenever the user needs a name for a new app, side project, or tool — including when they describe an idea and haven't picked a name yet, ask "what should I call this," or when the spec-suite skill's naming stage is reached. Also use it if the user wants to rename an existing project, or is unhappy with a current placeholder/working title. This is a small, standalone skill — it doesn't require running the full spec-suite workflow to use.
---

# Name It

A small skill for generating single-word, brand-friendly project names from a short description — the same naming step used at the start of `spec-suite`, but usable on its own whenever a name is needed without running the full spec workflow.

## How to use this

1. Get a short description of the project: what it does, who it's for, and — if there is one — the core feeling or metaphor behind it (e.g. "weaves two people's schedules together" for an LDR app).
2. Generate 5-8 candidates following the criteria in `references/naming-criteria.md`. Don't generate fewer than 5 — a shortlist of 2-3 doesn't give enough real choice.
3. Present the shortlist as a table: name, and a one-line reason it fits (see format below). Don't just dump a word list with no reasoning — the reasoning is what lets the user actually choose between them, not just react to the words.
4. Ask the user to pick one, or narrow to their top 2-3 favorites. Don't proceed to build any other documents (PRD, tech spec, etc.) under a placeholder or working title — get the name settled first.

## Shortlist format

| Name | Why it fits |
|---|---|
| Loom | Weaves two people's timelines together — fits an LDR app |
| Ember | Something small kept alive over distance |
| ... | ... |

## What makes a good candidate

See `references/naming-criteria.md` for the full checklist, but at minimum every candidate must be:
- A single word (no compound names, no "-App"/"-Hub"/"-ly" suffixes tacked on for availability's sake).
- Easy to say and spell out loud — if it needs to be spelled letter-by-letter in conversation, it's not a good candidate.
- Evocative of the project's function or feeling, not generic (avoid names that could apply to literally any app in the category).

## When NOT to force a metaphor

Not every project has (or needs) a poetic angle. For utility-first tools (trackers, dashboards, internal tools), a name that's simply clear and pleasant to say is enough — don't strain for a clever metaphor where a plain, solid word works better. Forcing cleverness onto a utility tool usually makes the name harder to remember, not easier.

## Quick availability sanity check

Before finalizing, do a quick check (web search) that the chosen name isn't already a heavily-used product in an adjacent space — this doesn't need to be a formal trademark search, just enough to avoid obvious collisions (e.g. naming a new app "Notion" or "Slack").
