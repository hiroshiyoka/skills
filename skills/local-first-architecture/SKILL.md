---
name: local-first-architecture
description: Decide whether a project should be built local-first (no backend, client-side storage only, e.g. Dexie.js/IndexedDB) or backend-required (Cloudflare Workers + KV/D1), and apply the architectural consequences of that choice consistently. Use this during tech spec creation for any new project or feature, whenever the user describes something that stores data (trackers, journals, dashboards, note-taking, personal tools), and especially before defaulting to "just add a backend" out of habit. Also use it to sanity-check an existing tech spec that added a backend without a clear justification, or that went local-only despite needing sync/multi-user features.
---

# Local-First Architecture

A skill for making the local-first vs. backend-required decision deliberately, early, and consistently — instead of defaulting to a backend out of habit, or going local-only for something that actually needs sync.

## Why this exists

A backend adds real cost: hosting, auth, data ownership questions, and an extra deployment target to maintain. For personal tools and single-user utilities, none of that cost buys anything the user needs. But going local-only for something that genuinely needs multi-device sync or shared data just pushes the problem to "rebuild it later." This skill exists so the decision gets made once, explicitly, with the tradeoffs stated — not defaulted either direction.

## Decision checklist

Walk through `references/decision-checklist.md` before writing the tech spec's data layer section. The short version:

**Lean local-first (Dexie.js/IndexedDB, no backend) when:**
- Data is single-user and doesn't need to be seen by anyone else.
- The device the user works on doesn't need to change mid-session (or occasional export/import is an acceptable substitute for sync).
- Privacy is a stated priority — local-first means the user's data never leaves their device.
- The project doesn't require server-side logic (scheduled jobs, webhooks, third-party API secrets that can't live in the client).

**Lean backend-required (Cloudflare Workers + KV/D1) when:**
- Multiple users need to see or edit shared data (e.g. Owe's shared expense ledgers).
- The user needs the same data across multiple devices without manual export/import.
- The feature depends on a secret that can't be exposed client-side (API keys for AI providers, payment processing).
- There's real-time or near-real-time behavior involving more than one client (notifications, live collaboration).

## How to apply this in a tech spec

State the decision explicitly in the tech spec's architecture section — not just the choice, but the specific criterion from the checklist that drove it (e.g. "local-first: single-user, no sync requirement, privacy-first per PRD"). This is what lets a build agent skip re-litigating the decision and go straight to implementation.

## Common mistake to avoid

Don't add a backend "just in case" a feature might need sync later. Per YAGNI, ship local-first if that's what the current PRD scope calls for, and revisit the architecture explicitly if a future feature genuinely requires it — don't pre-build the backend for a feature that isn't in scope yet.

Conversely, don't force something onto local-only storage if the PRD already describes multi-user or multi-device behavior as core scope (not a "nice to have") — that's a sign the project needs a backend from the start, and retrofitting sync onto a local-first app later is significantly harder than starting with one.

## Reference examples from prior projects

- **Trackr** (local-first): privacy-first job application tracker, single-user, no sync needed — Dexie.js, no backend.
- **Nomi** (local-first): personal productivity dashboard, single-user, localStorage/Dexie-based — no backend.
- **Farwell** (backend-required): needs an AI coach with a server-side API key (Groq) and Cloudflare KV for state — client-only wasn't an option once a third-party AI secret was involved.
- **Owe** (backend-required, on-chain): shared multi-currency ledgers between multiple users — inherently needs shared state, handled on-chain (Base) rather than a traditional backend, but the same "shared state" criterion applies.
