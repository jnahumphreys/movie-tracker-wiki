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
- **Stack:** npm, Vite, TypeScript, React, shadcn/Tailwind CSS, [Zustand](https://zustand.docs.pmnd.rs/) (slices pattern) with Suspense for client state management. Confirmed 2026-07-03, replacing an earlier plain Context+Reducer plan — Zustand's slices pattern is what makes the cache-eviction-needs-library-visibility case ([below](#persisted-data-shape)) workable without either scattering cross-store logic across components or reaching for a heavier library: slices share one store and a slice's own action can read another slice's state via `get()`, still small (~1KB) and dependency-light.
- Maintain a clear separation between the movie cache slice, UI slice, and user library slice — each exposes its own actions; other slices call those actions (or read state via `get()`), never reassign another slice's state directly.
- Improve performance by using the OMDb ID as a keyed lookup source of truth.
- Prevent unnecessary re-renders via Zustand's selector-based subscriptions — a component using `useStore(selector)` only re-renders when the selected slice of state actually changes, not on every store update.
- **Browser support:** last 2 versions of evergreen browsers (Chrome, Firefox, Safari, Edge) — no legacy-browser workarounds needed. Post-MVP, formalise this with a [browserslist](https://github.com/browserslist/browserslist) config rather than an implicit convention.
- Track changes in git, using per-feature branching and [Conventional Commit](https://www.conventionalcommits.org/en/v1.0.0/) messages.
- Align to [Semantic Versioning (SemVer)](https://semver.org/).
- Local library search is live and debounced at **250ms**. OMDb search returns the top 10 hits, and only runs on hitting the search button. The 10-result cap is a deliberate MVP scope limit, not an oversight — see [Product — Future Considerations](/product/future-considerations.md) for the planned lazy-loading/infinite-scroll follow-up.
- **OMDb search costs up to 11 API calls, not 1.** OMDb's `s=` search endpoint doesn't return Runtime, so each of the up to 10 results gets a follow-up `t=`/`i=` detail lookup before rendering — see [OMDb API Reference](/engineering/omdb-api-reference.md#two-different-endpoints-two-different-response-shapes) for why, and the daily-quota implications.

## Persisted data shape

Each localStorage-backed store (movie cache, user library) is saved as `{ schemaVersion: <number>, data: {...} }` — the version tag exists purely as insurance: most future field additions are safe without it (see per-store shapes below), but if a future change to a store's shape is ever non-additive, the app can detect the version mismatch on load and reset just that store, rather than guessing from missing/broken fields. Bump the relevant store's `schemaVersion` whenever its shape changes in a way older saved data won't satisfy.

- **User library store** — object keyed by OMDb ID, storing only a *reference*, not the movie data itself: `{ [omdbId]: { status: "to watch" | "watched", creationDate: <ISO 8601 string>, modifiedDate: <ISO 8601 string> } }`. Mirrors how this'll map to a database later (`omdbId` as a foreign key into a movies table). New optional fields can be added later without a version bump; renaming/restructuring an existing field needs one.
- **Movie cache store** — the single source of movie data, object keyed by OMDb ID: `{ [omdbId]: { ...raw OMDb response, see OMDb API Reference, cachedAt: <ISO 8601 string> } }`. `cachedAt` is set once, on first insert, and **never modified again** — reading a cache entry is a pure read with no write, by design (see below).
- **Cache lookup, read-only:** to render a movie (in search results or in the library), look it up in the cache by OMDb ID first, as a plain read. On a miss, fetch it fresh from OMDb by ID (`?i=<imdbID>`), stamp it with `cachedAt`, and insert it — this is the only time an entry is written. This makes the cache self-healing (a missing/evicted entry just gets re-fetched and re-inserted on next read) without ever needing to touch an existing entry, which keeps reads free of side effects — no re-render risk from merely displaying a cached movie.
- **Eviction is FIFO by `cachedAt`, checked only at insert time.** The cache is capped at 200 non-library entries. Whenever a new entry needs to be added and the cache is already at the cap, drop the single oldest non-library entry (by `cachedAt`) first, then insert the new one — one rule, no separate startup sweep or background job, and **never** a bulk purge of every non-library entry at once. Note this is oldest-*added*, not least-recently-*used* — a frequently re-searched-but-old entry can still get evicted, since re-finding it in cache is a read, not a write; an accepted simplicity tradeoff given a cache miss just triggers a self-healing re-fetch anyway.
- **Eviction lives entirely inside the movie-cache slice's own insert action.** It needs to skip any OMDb ID currently in the user library, which is why this store needs Zustand's slices pattern rather than plain isolated Context+Reducer stores: the insert action reads the library slice's state via `get()` (a same-store, cross-slice read that Zustand's pattern is explicitly designed for — not a violation of separation, since it's still only ever *reading* another slice's state, never reassigning it) to compute the eviction candidate set, then drops the oldest of those. No UI component needs to know eviction happens at all.
- **Dates are stored and typed as ISO 8601 strings, never `Date` objects.** `JSON.stringify` turns a `Date` into a string on the way into localStorage, but `JSON.parse` doesn't turn it back on the way out — treat `creationDate`/`modifiedDate`/`cachedAt` as strings everywhere they're read.

See also: [Codebase Structure & Conventions](/engineering/onboarding/codebase-structure-and-conventions.md) for how this stack maps to actual folders and files, [Component & State Architecture](/engineering/onboarding/component-and-state-architecture.md) for how the Zustand slices and Suspense are used in practice, and [Contribution & Testing Workflow](/engineering/onboarding/contribution-and-testing-workflow.md) for how the branching/commit conventions mentioned above play out day to day.
