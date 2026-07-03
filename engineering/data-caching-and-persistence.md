---
type: Engineering Architecture
title: Data Caching & Persistence
description: Persisted localStorage shape, movie-cache lookup, and FIFO eviction.
tags: [engineering, architecture, caching, persistence]
timestamp: 2026-07-03T00:00:00Z
status: confirmed
---

⬅ [Engineering](/engineering/index.md)

# Data Caching & Persistence

- Fetches data from the OMDb API and caches movie data to the client session. User library data and preferences persist across sessions, using the browser's local storage API.
- Improve performance by using the OMDb ID as a keyed lookup source of truth.

## Persisted data shape

Each localStorage-backed store (movie cache, user library) is saved as `{ schemaVersion: <number>, data: {...} }` — the version tag exists purely as insurance: most future field additions are safe without it (see per-store shapes below), but if a future change to a store's shape is ever non-additive, the app can detect the version mismatch on load and reset just that store, rather than guessing from missing/broken fields. Bump the relevant store's `schemaVersion` whenever its shape changes in a way older saved data won't satisfy.

- **User library store** — object keyed by OMDb ID, storing only a *reference*, not the movie data itself: `{ [omdbId]: { status: "to watch" | "watched", creationDate: <ISO 8601 string>, modifiedDate: <ISO 8601 string> } }`. Mirrors how this'll map to a database later (`omdbId` as a foreign key into a movies table). New optional fields can be added later without a version bump; renaming/restructuring an existing field needs one.
- **Movie cache store** — the single source of movie data, object keyed by OMDb ID: `{ [omdbId]: { ...raw OMDb response, see OMDb API Reference, cachedAt: <ISO 8601 string> } }`. `cachedAt` is set once, on first insert, and **never modified again** — reading a cache entry is a pure read with no write, by design (see below).
- **Cache lookup, read-only:** to render a movie (in search results or in the library), look it up in the cache by OMDb ID first, as a plain read. On a miss, fetch it fresh from OMDb by ID (`?i=<imdbID>`), stamp it with `cachedAt`, and insert it — this is the only time an entry is written. This makes the cache self-healing (a missing/evicted entry just gets re-fetched and re-inserted on next read) without ever needing to touch an existing entry, which keeps reads free of side effects — no re-render risk from merely displaying a cached movie.
- **Eviction is FIFO by `cachedAt`, checked only at insert time.** The cache is capped at 200 non-library entries. Whenever a new entry needs to be added and the cache is already at the cap, drop the single oldest non-library entry (by `cachedAt`) first, then insert the new one — one rule, no separate startup sweep or background job, and **never** a bulk purge of every non-library entry at once. Note this is oldest-*added*, not least-recently-*used* — a frequently re-searched-but-old entry can still get evicted, since re-finding it in cache is a read, not a write; an accepted simplicity tradeoff given a cache miss just triggers a self-healing re-fetch anyway.
- **Eviction lives entirely inside the movie-cache slice's own insert action.** It needs to skip any OMDb ID currently in the user library, which is why this store needs Zustand's slices pattern rather than plain isolated Context+Reducer stores — see [State Management](/engineering/state-management.md#store-access-pattern) for the cross-slice `get()` mechanics that make this possible.
- **Dates are stored and typed as ISO 8601 strings, never `Date` objects.** `JSON.stringify` turns a `Date` into a string on the way into localStorage, but `JSON.parse` doesn't turn it back on the way out — treat `creationDate`/`modifiedDate`/`cachedAt` as strings everywhere they're read.

See also: [Stack & Tooling](/engineering/stack-and-tooling.md) for the overall stack decision, [State Management](/engineering/state-management.md) for the Zustand slices this data lives in, and [OMDb API Reference](/engineering/omdb-api-reference.md) for the raw response shape being cached.
