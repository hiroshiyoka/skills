# Local-First vs. Backend-Required — Decision Checklist

Answer these in order. The first "yes" that forces a backend wins — don't keep looking for reasons to justify a backend once one of these is true, and don't ignore one that's true just because local-first feels simpler.

## 1. Does more than one person need to see or change this data?
- **Yes** → backend-required (or on-chain, if the project's domain is Web3 and shared state fits a smart contract better than a server).
- **No** → continue.

## 2. Does the user need the same data across multiple devices, without manually exporting/importing?
- **Yes** → backend-required (need a sync layer of some kind).
- **No, occasional export/import is fine** → continue, lean local-first.

## 3. Does any feature depend on a secret that can't be shipped to the client (API key, payment credential)?
- **Yes** → backend-required, at minimum a thin proxy/Worker to hold the secret and forward requests.
- **No** → continue.

## 4. Does the feature need real server-side logic — scheduled jobs, webhooks, background processing?
- **Yes** → backend-required.
- **No** → local-first is very likely the right call.

## 5. Is privacy/data ownership a stated priority in the PRD?
- If yes, and none of the above forced a backend, this is a strong additional reason to commit to local-first — the data literally never leaving the device is a feature, not just an implementation detail.

---

## If local-first: what that implies

- Storage: Dexie.js over raw IndexedDB (see `pick-stack` skill for the standard choice).
- No auth layer needed unless there's a reason to gate the app itself.
- Export/import should be considered as a lightweight substitute for sync — even a simple JSON export covers most "I want to move to a new device" needs without building real sync infrastructure.
- Testing/dev: no backend deployment target to maintain, which simplifies the tech spec considerably — say so explicitly rather than leaving an empty "backend" section.

## If backend-required: what that implies

- Default to Cloudflare Workers + KV unless a relational query pattern specifically requires D1 (see `pick-stack` skill).
- State in the tech spec which specific checklist item (above) forced this decision — this prevents a future reviewer (or a future agent) from wrongly "simplifying" the architecture back to local-first.
- Consider whether auth is now required as a consequence (shared/multi-device data usually implies some form of user identity, even if lightweight).
