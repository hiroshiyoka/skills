# Stack Defaults

Default choices to reach for unless the project has a specific reason to deviate. If deviating, say so explicitly in the tech spec rather than silently picking something else.

## Language & framework
- **TypeScript-first.** No untyped JS in new projects.
- **TanStack ecosystem** (TanStack Start, Router, Query) as the default web app framework, unless the project specifically calls for something else (e.g. SvelteKit for the portfolio site).

## Hosting & infra
- **Cloudflare Pages / Workers** over Netlify or Vercel for new deployments.
- **Cloudflare KV** for lightweight key-value persistence when a full database is overkill.
- Subdomains under the personal domain follow the `project.hiroshiyoka.xyz` pattern; consider a `hiroshiyoka.xyz/project` subpath + rewrite instead when the project should share domain SEO authority with the main site.

## Data layer — pick one deliberately
- **Local-first / no backend:** IndexedDB via **Dexie.js**, entirely client-side, when the project's data is personal/single-user and doesn't need sync or multi-device access (e.g. Trackr, Nomi). Default to this when in doubt — it's simpler to ship and inherently privacy-first.
- **Backend needed:** Cloudflare Workers + KV (or D1 if relational queries are required) when the project needs sync, multi-user data, or server-side logic (e.g. Farwell's AI coach, Owe's shared ledgers).

State the reasoning for the choice in the tech spec — don't just default silently.

## Styling
- **Tailwind CSS** as the default utility layer.

## Local development
- **Podman**, not Docker, for any local containers.
- Windows DNS pinned to `1.1.1.1` / `1.0.0.1` to avoid ISP resolver caching — mention in setup docs if the project involves DNS-sensitive local dev.

## Web3 (when applicable)
- **Base** network for EVM-based projects (Solidity + wagmi/viem + Privy for auth).
- **Solana** for DeFi-adjacent or DLMM-related tooling.
- Stablecoin support should consider both USDC and IDRX where relevant to the target users.

## Naming & documentation conventions
- Every project name is a single word, brand-friendly, selected from a generated shortlist (see `templates/naming.md`).
- AI agent build prompts live in `prompt-<projectname>.md` and are self-contained enough to hand directly to a coding agent.
