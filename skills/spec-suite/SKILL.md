---
name: spec-suite
description: Turn a raw project idea into a complete, build-ready specification suite (naming shortlist, PRD, tech spec, design system, data/API spec, and an AI agent build prompt) before any code is written. Use this whenever the user wants to start a new app, side project, or feature and hasn't yet produced planning documents — even if they only describe a vague idea, ask "help me plan this project," or say "I want to build X." Also use it to review or fill gaps in an existing partial spec, or to keep a project's documents consistent with the stack conventions below. Don't wait for the user to explicitly ask for "a PRD" — if they're describing a project they intend to build, this skill's workflow applies.
---

# Spec Suite

A skill for turning an idea into a full set of build-ready planning documents, in a fixed order, before any implementation starts. This is the difference between "let's just start coding and figure it out" and "the AI agent that eventually builds this has zero ambiguity to fill in on its own."

## Why this exists

Vague specs force a coding agent to invent decisions — naming, data shape, error states, visual language — and those invented decisions are usually inconsistent with each other and with the rest of the codebase. Producing the five documents below, in order, closes that gap before an agent ever touches a file.

## The workflow

Always follow this order. Each stage feeds the next — don't skip ahead or produce documents out of sequence, and don't silently merge stages together even if the user asks for "just the PRD."

1. **Brainstorm & scope** — Clarify what the project actually is in 2-3 sentences: the core problem, who it's for, and the one thing it must do well. If the user's description is vague, ask exactly one clarifying question (per the project's normal ambiguity-handling behavior) rather than guessing at scope.
2. **Naming** — Generate a shortlist of 5-8 single-word, brand-friendly name candidates (see `templates/naming.md` for the exact criteria). Get the user to pick one, or narrow it to their top 2-3 and ask which they want, before moving on. Don't generate documents under a placeholder name.
3. **PRD** (`templates/prd.md`) — What the product does and why, from a user/business perspective. No implementation detail here.
4. **Tech spec** (`templates/tech-spec.md`) — Translates the PRD into an actual architecture using the stack defaults in `references/stack-defaults.md`. This is where local-first vs. backend, and which Cloudflare primitives, get decided explicitly.
5. **Design system** (`templates/design-system.md`) — Visual language: type, color, spacing, and any signature visual element unique to this project (e.g. Farwell's "Fade Bar"). Every project should have at least one deliberate, named visual signature — not just "uses Tailwind defaults."
6. **Data/API spec** (`templates/api-spec.md`) — Concrete schemas, endpoints or IndexedDB tables, and request/response shapes. This must be precise enough that an agent doesn't need to invent field names.
7. **AI agent build prompt** (`templates/agent-prompt.md`) — The final, self-contained prompt that gets handed to Claude Code / Cursor to actually build the thing. It should reference the other four documents rather than repeat them, and state build order (scaffold → data layer → core flow → polish).

## Stack defaults

Before writing the tech spec, read `references/stack-defaults.md` — it encodes the default stack choices so the agent doesn't re-litigate them per project (TypeScript-first, TanStack ecosystem, Cloudflare Workers/Pages/KV, Tailwind, Podman for local containers, local-first with Dexie.js/IndexedDB when no backend is truly needed). Deviating from these defaults is fine when the project genuinely calls for it, but the tech spec should say so explicitly rather than silently picking something else.

## Engineering principles

All generated documents — especially the tech spec and agent prompt — should bake in Clean Code, SOLID, DRY, YAGNI, and KISS as explicit constraints for the build agent, not just as a vague aspiration. See `references/principles.md` for how to phrase these as concrete build-prompt instructions rather than platitudes.

## Output

Produce each of the five documents as separate files (not one giant doc), named after the project (e.g. `loom-prd.md`, `loom-tech-spec.md`). Confirm the name and one-line scope with the user before generating all five in full — a wrong assumption compounds across five documents, so it's worth a 10-second check first.

## When NOT to use the full suite

For a small, single-purpose script or a one-off fix, don't force all five documents — a short tech note is enough. This skill is for projects intended to be built by an AI coding agent from a cold start (new app, new major feature with its own data model), not for quick utilities.
