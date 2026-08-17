# Engineering Principles as Build-Prompt Instructions

Don't just tell a build agent "follow Clean Code and SOLID" — that's too vague to act on. Translate each principle into a concrete, checkable instruction inside the tech spec or agent prompt.

| Principle | Vague version (avoid) | Concrete version (use) |
|---|---|---|
| Clean Code | "Write clean code" | "Functions do one thing; name things by intent, not implementation; no function over ~40 lines without a clear reason." |
| SOLID | "Follow SOLID" | "New features extend via new modules/handlers, not by adding conditional branches to existing ones. Dependencies are injected/passed in, not hardcoded imports of concrete implementations." |
| DRY | "Don't repeat yourself" | "If the same logic appears in 3+ places, extract it — but not before the third occurrence (see YAGNI)." |
| YAGNI | "Don't over-engineer" | "Don't add config options, abstraction layers, or extensibility hooks for features that aren't in this spec's scope." |
| KISS | "Keep it simple" | "Prefer the boring, well-understood solution (e.g. Dexie.js over a custom IndexedDB wrapper) unless the spec explicitly requires otherwise." |

When writing the agent build prompt, include a short section literally titled "Engineering constraints" with 3-5 bullets pulled from this table, tailored to the specific project — not the whole table verbatim.
