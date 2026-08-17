# Trusted Stack

One default per category. Use it unless the project has a specific, stated reason to deviate — and if it does, note the deviation in the tech spec rather than picking silently.

## Language
| Category | Default | Notes |
|---|---|---|
| Language | TypeScript | No untyped JS in new projects. |

## Web app framework
| Category | Default | Notes |
|---|---|---|
| Framework | TanStack Start | Includes TanStack Router + Query. Default for most new web apps. |
| Framework (portfolio/content-heavy sites) | SvelteKit | Used when the project is primarily content/marketing-oriented rather than app-like. |

## Data & storage
| Category | Default | Notes |
|---|---|---|
| Local-only storage | Dexie.js (over IndexedDB) | Default for local-first projects — see `local-first-architecture` skill for when this applies. |
| Backend key-value | Cloudflare KV | Default when a full relational DB is overkill. |
| Backend relational | Cloudflare D1 | Use when relational queries are actually required — don't default to this if KV suffices. |
| ORM (when a relational DB is used) | Prisma | Used on prior projects (e.g. KKA university projects); reasonable default for Node/TS backends outside the Cloudflare edge runtime. |

## Styling
| Category | Default | Notes |
|---|---|---|
| CSS | Tailwind CSS | Default utility layer for all new frontend work. |
| Component primitives (when needed) | shadcn/ui | Only pull in specific components as needed, don't scaffold the whole library upfront. |

## Auth
| Category | Default | Notes |
|---|---|---|
| Web3 wallet auth (EVM) | Privy | Used with wagmi/viem on Base projects (Owe). |
| Traditional auth | Evaluate per project | No established default yet — pick the most maintained option (e.g. Lucia, Auth.js) and note the choice; flag for future addition to this table. |

## AI / LLM
| Category | Default | Notes |
|---|---|---|
| Fast inference AI features (chat, coach-style features) | Groq | Used for Farwell's "Wells" AI coach. Good default for latency-sensitive AI features. |
| General-purpose reasoning / document generation | Claude (Anthropic API) | Default for anything requiring stronger reasoning, longer context, or agentic tool use. |

## Web3
| Category | Default | Notes |
|---|---|---|
| EVM network | Base | Default chain for EVM projects. |
| EVM tooling | Solidity + wagmi + viem | Standard combination used across Owe and Proven. |
| Non-fungible / reputation tokens | ERC-5192 (soulbound) | Used for Proven's non-transferable reputation tokens. |
| Off-chain metadata | IPFS | Default for token/NFT metadata storage. |
| EVM indexing | Ponder | Used for Drip's on-chain event indexing. |
| Solana | Native Solana stack (no established default library yet) | Flag for future addition once a DLMM/monitoring library is settled on. |

## Deployment & infra
| Category | Default | Notes |
|---|---|---|
| Hosting | Cloudflare Pages / Workers | Default over Netlify/Vercel for new deployments. |
| Local containers | Podman | Not Docker. |
| Local DNS | `1.1.1.1` / `1.0.0.1` | Set at the OS level to avoid ISP resolver caching issues during local dev. |

## Presentation / document generation (tooling scripts, not app dependencies)
| Category | Default | Notes |
|---|---|---|
| Programmatic slide generation | pptxgenjs | Used for thesis defense deck generation. |
