# Skills for Shipping With AI Agents

For developers who want their AI coding agents to build from a real spec instead of guessing.

Going from "I have an idea" to "an agent can build this correctly on the first try" is hard. Most of the gap isn't code — it's the planning an agent never gets: naming, scope boundaries, data shapes, and a consistent stack. Skip that step and every agent run invents its own answers, and none of them agree with each other.

These skills are a distillation of the same planning workflow used to ship every project in this catalog's origin — from local-first utilities to full-stack Web3 apps — encoded so an AI agent can follow it directly instead of being told "use your best judgment."

## Install

```bash
npx skills add hiroshiyoka/skills
```

Or install a single skill:

```bash
npx skills add hiroshiyoka/skills/spec-suite
```

## Why use it?

Agents default to guessing when a spec is incomplete

Hand an agent a one-line idea and it will pick a name, a stack, a data shape, and a visual style on its own — usually inconsistently across a single build, and almost never the way you'd have chosen. These skills front-load those decisions into documents the agent reads *before* writing code, so the guessing happens once, on paper, where it's cheap to fix — not five times, scattered across a codebase.

This is a shortcut to specs that are actually build-ready, not just a wall of prose an agent skims past.

## Reference

- **[spec-suite](./skills/spec-suite/SKILL.md)** — Turns a raw idea into a full build-ready spec: naming shortlist, PRD, tech spec, design system, data/API spec, and a self-contained AI agent build prompt, in that order.
- **[pick-stack](./skills/pick-stack/SKILL.md)** — Have your agent pick the right library, framework, or service from a trusted, pre-vetted list instead of hand-rolling a component or installing something abandoned.
- **[local-first-architecture](./skills/local-first-architecture/SKILL.md)** — Decide deliberately whether a project should be local-first (no backend) or backend-required, with a checklist instead of a default-by-habit.
- **[principles-check](./skills/principles-check/SKILL.md)** — Audit a codebase against Clean Code, SOLID, DRY, YAGNI, and KISS, and get a prioritized, actionable fix plan instead of vague "this could be cleaner" feedback.
- **[name-it](./skills/name-it/SKILL.md)** — Generate a shortlist of single-word, brand-friendly project names from a short description. Usable standalone, or as spec-suite's naming stage.
- **[conventional-commits](./skills/conventional-commits/SKILL.md)** — Write commit messages in a consistent, parseable `type(scope): description` format across every repo.
- **[smart-contract-patterns](./skills/smart-contract-patterns/SKILL.md)** — Apply proven Solidity patterns (multi-token whitelisting, isolated per-currency balances, soulbound tokens, escrow) instead of designing contract structure from scratch.

## Philosophy

- **Planning is cheap. Rework is not.** A wrong assumption caught at the PRD stage costs a sentence. The same assumption caught after the build costs a rewrite.
- **Specs should be precise enough to remove ambiguity, not just describe intent.** "Build a task tracker" is not a spec. A named data model, a stated out-of-scope list, and a defined stack are.
- **Defaults exist so they don't need to be re-decided every time.** These skills encode a consistent, opinionated stack (TypeScript-first, TanStack, Cloudflare Workers/Pages, local-first where it fits) so that decision doesn't get re-litigated on every new project — while still leaving room to deviate deliberately when a project calls for it.

## License

MIT