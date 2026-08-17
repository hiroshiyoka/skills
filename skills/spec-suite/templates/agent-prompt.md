# AI Agent Build Prompt Template — `<project-name>`

This is the final artifact handed directly to Claude Code / Cursor. It should reference the other four documents rather than repeat their content in full.

---

## Project
`<project-name>` — one-sentence description (from the PRD's core value proposition).

## Reference documents
This build should follow, in full:
- `<project-name>-prd.md` — product scope and requirements
- `<project-name>-tech-spec.md` — architecture and stack
- `<project-name>-design-system.md` — visual language
- `<project-name>-api-spec.md` — data model and operations

Do not deviate from the stack or data model defined in those documents without flagging it first.

## Engineering constraints
(Pull 3-5 concrete bullets from `references/principles.md`, tailored to this project — not the whole table.)

- ...
- ...

## Build order
1. Scaffold the project (framework, folder structure, tooling) per the tech spec.
2. Implement the data layer exactly per the API spec (schema/Dexie tables or backend endpoints).
3. Build the core user flow end-to-end (the PRD's MVP features), functional before polished.
4. Apply the design system — typography, color, spacing, and the signature element.
5. Handle the error states listed in the API spec.

## Definition of done
Restate the PRD's success criteria here as a literal checklist the agent can verify against before calling the build complete.

## Explicitly out of scope
Copy the PRD's "out of scope" section here as a reminder — this is what stops an agent from over-building.
