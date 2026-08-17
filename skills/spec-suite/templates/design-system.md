# Design System Template — `<project-name>`

## 1. Visual direction (one paragraph)
Describe the feeling this project should have in a sentence or two (e.g. "minimal, dark-first, developer-focused" or "warm, soft, emotionally gentle"). This should follow directly from the PRD's target user and the "signature moment," if one was named.

## 2. Signature element
If the PRD named a signature moment, design it concretely here: what it looks like, how it animates or behaves, when it appears. Give it a name (like "Fade Bar") so it's referenceable elsewhere in the docs and in the codebase.

## 3. Typography
- Primary font family, and fallback
- Scale (headings, body, small text) — a short list, not an exhaustive type ramp unless the project needs one

## 4. Color
- Base palette (background, text, accent) — list actual values, not just names
- Dark/light mode: pick one as default; state whether the other is supported

## 5. Spacing & layout
- Base spacing unit (e.g. 4px/8px grid)
- Any layout constraints (max content width, sidebar behavior, mobile breakpoints)

## 6. Components needing custom treatment
List only components that deviate from a standard UI library default (e.g. Tailwind/shadcn defaults) — don't redescribe standard buttons/inputs unless something about them is unusual for this project.

## 7. Motion (if relevant)
Keep this short and specific: what animates, how fast, what easing. Avoid vague language like "smooth transitions" — name the actual duration/easing intent.
