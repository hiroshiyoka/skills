# Data / API Spec Template — `<project-name>`

Must be precise enough that a build agent never has to invent a field name, type, or endpoint shape on its own.

## 1. Data model
For each entity from the tech spec, give the full shape:

```ts
interface Task {
  id: string;          // uuid
  title: string;
  completed: boolean;
  createdAt: number;    // unix ms
}
```

If local-first (Dexie.js/IndexedDB): include the Dexie schema/version definition. If backend-required: include the database schema (tables/columns or KV key patterns).

## 2. Endpoints / operations
If backend-required, list each endpoint:

| Method | Path | Request body | Response | Notes |
|---|---|---|---|---|
| POST | /api/tasks | `{ title: string }` | `Task` | |

If local-first, list the equivalent local operations (Dexie queries) instead of HTTP endpoints — same level of precision.

## 3. Error states
For each operation, what can go wrong and what should happen (e.g. validation failure, storage quota exceeded, network failure for any synced parts). Don't leave this to be improvised at build time.

## 4. Third-party integrations (if any)
Any external API, its auth method, and the specific endpoints/fields used — not just "integrates with X."
