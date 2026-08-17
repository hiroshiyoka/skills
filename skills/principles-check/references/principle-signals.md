# Principle Signals — What to Actually Look For

Each principle below has concrete, checkable signals. Look for these specifically rather than relying on a general "this feels messy" impression.

## Clean Code

- Functions longer than ~40 lines without a clear structural reason (e.g. a single large switch that genuinely needs to be in one place).
- Names that describe *how* something works instead of *what* it's for (`data2`, `handleClick2`, `tempFlag`) — rename or explain why it's necessary.
- Deep nesting (4+ levels of if/for) — usually fixable with early returns or extraction.
- Comments explaining *what* code does instead of *why* — a sign the code itself isn't clear enough to read on its own.
- Inconsistent naming conventions within the same file/module (mixing camelCase and snake_case, `get`/`fetch`/`load` used interchangeably for the same kind of operation).

## SOLID

Check each letter separately — violations of one don't imply violations of the others.

- **S (Single Responsibility):** a function or module that does two unrelated things (e.g. a component that both fetches data and formats it for three different display contexts).
- **O (Open/Closed):** adding a new case requires editing an existing function's internals with a new `if`/`switch` branch, rather than being pluggable (a new handler, a new strategy object, a new registered type).
- **L (Liskov Substitution):** a subtype/implementation that can't actually be swapped in for its parent/interface without breaking callers (rare in practice for smaller projects — only flag if it's a real, demonstrable issue).
- **I (Interface Segregation):** a type/interface that forces implementers to define methods they don't need — usually shows up as empty or `throw new Error("not implemented")` method bodies.
- **D (Dependency Inversion):** a module directly importing and instantiating a concrete implementation (e.g. a specific database client) deep inside business logic, instead of receiving it as a parameter/injected dependency — makes testing and swapping implementations hard.

## DRY

- The same logic (not just similar-looking code, actual same logic) appearing in 3 or more places — the threshold that justifies extraction.
- Copy-pasted validation, formatting, or calculation logic that has already drifted slightly between copies (a strong signal it should have been extracted before now, since the copies are already out of sync).
- Don't flag 2 occurrences as a DRY violation — see YAGNI below; premature extraction after only one duplicate is its own problem.

## YAGNI

- Configuration options, feature flags, or extensibility hooks for features that aren't in the current PRD/spec scope.
- Generic/abstracted code written for "future flexibility" with only one current caller.
- Unused exports, dead code paths, or commented-out blocks left "just in case."

## KISS

- A custom-built solution for a problem a well-known, maintained library already solves simply (check `pick-stack` skill's trusted list before flagging this — if the project already deviates from the trusted default for a stated reason, don't re-flag it here).
- Cleverness that requires a comment to explain what a simpler, more obvious approach wouldn't have needed.
- Over-abstracted code where a straightforward, slightly more repetitive approach would actually be easier to read and modify.
