# Tech Spec Template — `<project-name>`

Translates the PRD into an actual architecture. Read `references/stack-defaults.md` before filling this in — use those defaults unless there's a specific reason not to, and if you deviate, say why here.

## 1. Architecture overview
One paragraph + a simple diagram-in-words: client, backend (if any), storage, third-party services. State explicitly whether this is local-first (no backend) or backend-required, and why (reference the PRD's features — does anything need sync, multi-user, or server logic?).

## 2. Stack
| Layer | Choice | Notes / deviation from defaults |
|---|---|---|
| Framework | | |
| Data/storage | | |
| Hosting | | |
| Styling | | |
| Auth (if any) | | |

## 3. Data model (high level)
List the core entities and their relationships — just enough to hand off to the data/API spec stage, not the full schema (that comes next).

## 4. Key technical risks
What's the one thing most likely to be hard or to break? (e.g. offline sync conflicts, IndexedDB storage limits, rate limits on a third-party API). Say how you plan to handle it, even briefly.

## 5. Non-functional requirements
Performance, offline support, browser targets, accessibility baseline — only include what's actually relevant to this project, don't pad with boilerplate.

## 6. Build phases (high level)
A rough order (e.g. scaffold → data layer → core flow → polish) — this gets fully detailed in the agent build prompt, but sketch it here first so the API spec and design system stages know what's coming first.
