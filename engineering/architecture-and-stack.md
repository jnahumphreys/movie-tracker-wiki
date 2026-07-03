---
type: Engineering Architecture
title: Architecture & Stack
description: MVP technical stack and architectural decisions.
tags: [engineering, architecture]
timestamp: 2026-07-03T00:00:00Z
status: confirmed
---

⬅ [Engineering](/engineering/index.md)

# Architecture & Stack

- Web-based single page application, running entirely on the client.
- Fetches data from the OMDb API and caches movie data to the client session.
- User library data and preferences persist across sessions, using the browser's local storage API.
- **Stack:** npm, Vite, TypeScript, React, shadcn/Tailwind CSS, and the React Context + Reducer API with Suspense for client state management. Note: this is to reduce initial dependency overhead — future iterations will migrate to more suitable libraries as needed (see [Future Considerations](/engineering/future-considerations.md)).
- Maintain a clear separation between the movie cache store, UI store, and user library store.
- Improve performance by using the OMDb ID as a keyed lookup source of truth.
- Prevent unnecessary re-renders through memoisation of store selectors.
- **Browser support:** last 2 versions of evergreen browsers (Chrome, Firefox, Safari, Edge) — no legacy-browser workarounds needed. Post-MVP, formalise this with a [browserslist](https://github.com/browserslist/browserslist) config rather than an implicit convention.
- Track changes in git, using per-feature branching and [Conventional Commit](https://www.conventionalcommits.org/en/v1.0.0/) messages.
- Align to [Semantic Versioning (SemVer)](https://semver.org/).
- Local library search is live and debounced at **250ms**. OMDb search returns the top 10 hits, and only runs on hitting the search button. The 10-result cap is a deliberate MVP scope limit, not an oversight — see [Product — Future Considerations](/product/future-considerations.md) for the planned lazy-loading/infinite-scroll follow-up.

## Persisted data shape

Each localStorage-backed store (movie cache, user library) is saved as `{ schemaVersion: <number>, data: {...} }` — the version tag exists purely as insurance: most future field additions are safe without it (see per-store shapes below), but if a future change to a store's shape is ever non-additive, the app can detect the version mismatch on load and reset just that store, rather than guessing from missing/broken fields. Bump the relevant store's `schemaVersion` whenever its shape changes in a way older saved data won't satisfy.

- **User library store** — object keyed by OMDb ID, storing only a *reference*, not the movie data itself: `{ [omdbId]: { status: "to watch" | "watched", creationDate: <ISO 8601 string>, modifiedDate: <ISO 8601 string> } }`. Mirrors how this'll map to a database later (`omdbId` as a foreign key into a movies table). New optional fields can be added later without a version bump; renaming/restructuring an existing field needs one.
- **Movie cache store** — the single source of movie data, object keyed by OMDb ID: `{ [omdbId]: { ...raw OMDb response, see OMDb API Reference, lastAccessed: <ISO 8601 string> } }`. Every read of a movie's data — search results, library views alike — is an O(1) lookup into this store by OMDb ID.
- **Cache lookup with fallback:** to render a movie (in search results or in the library), look it up in the cache by OMDb ID first; on a miss, fetch it fresh from OMDb by ID (`?i=<imdbID>`) and re-populate the cache entry. This makes the cache self-healing — a failed or evicted cache write is never a permanent inconsistency, since the next read repairs it (network permitting).
- **Eviction never touches a library-referenced movie.** The cache is capped at 200 entries with LRU eviction (`lastAccessed` updated on every read) — but eviction only considers entries whose OMDb ID isn't currently a key in the user library store. This guarantees a saved library always has its movie data on hand without needing a network round-trip, satisfying "library persists across sessions" even offline. Only non-library search results are subject to the cap.
- **Dates are stored and typed as ISO 8601 strings, never `Date` objects.** `JSON.stringify` turns a `Date` into a string on the way into localStorage, but `JSON.parse` doesn't turn it back on the way out — treat `creationDate`/`modifiedDate`/`lastAccessed` as strings everywhere they're read.

See also: [Codebase Structure & Conventions](/engineering/onboarding/codebase-structure-and-conventions.md) for how this stack maps to actual folders and files, [Component & State Architecture](/engineering/onboarding/component-and-state-architecture.md) for how the Context+Reducer stores and Suspense are used in practice, and [Contribution & Testing Workflow](/engineering/onboarding/contribution-and-testing-workflow.md) for how the branching/commit conventions mentioned above play out day to day.
