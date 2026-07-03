---
type: Engineering Architecture
title: State Management
description: Zustand slices — why, store access rules, Suspense boundaries, and memoisation.
tags: [engineering, architecture, state]
timestamp: 2026-07-03T00:00:00Z
status: confirmed
---

⬅ [Engineering](/engineering/index.md)

# State Management

## Why Zustand, why slices

- **Stack:** [Zustand](https://zustand.docs.pmnd.rs/) (slices pattern) with Suspense, confirmed 2026-07-03 — replacing an earlier plain Context+Reducer plan. Slices are what make the cache-eviction-needs-library-visibility case (see [Data Caching & Persistence](/engineering/data-caching-and-persistence.md)) workable without either scattering cross-store logic across components or reaching for a heavier library: slices share one store, and a slice's own action can read another slice's state via `get()`. Still small (~1KB) and dependency-light.
- Maintain a clear separation between the movie cache slice, UI slice, and user library slice — each exposes its own actions; other slices call those actions (or read state via `get()`), never reassign another slice's state directly.
- **UI slice content, confirmed 2026-07-03:** just `currentView: "to-watch" | "watched" | "omdb-search"`, plus the action that sets it. There's no routing library yet (see [Engineering — Future Considerations](/engineering/future-considerations.md)), so this is the single source of truth for which view is active — it drives both the [nav row's active-button highlight](/design/components/top-bar-and-navigation.md#navigation-row-2--scrolls-with-content) and the [top bar's count badge](/design/components/top-bar-and-navigation.md#top-bar-persistent-chrome-row-1--sticky) (which count to show, and whether it's hidden). Per-dialog save/error state is deliberately **not** here — see [Write state (dialogs)](#write-state-dialogs) below.

## Store access pattern

State is a **single Zustand store composed of three slices** (movie cache, UI, user library) — one store, not three separate Providers.

- **One slice file per concern**, each exposing its own typed state and actions (e.g. the movie-cache slice exposes `cacheMovie()`, the user-library slice exposes `addToLibrary()`, `reassignStatus()`, `removeFromLibrary()`). Components read via a selector — `useStore(state => state.movieCache.entries)` — and only re-render when the selected slice of state actually changes; no Provider, no Context, no manual memoised-selector plumbing to write yourself.
- **A slice's actions are the only sanctioned way to change its own state.** Never mutate slice state directly from a component or from another slice — always go through the owning slice's exposed action, even when that call happens from inside another slice (see cross-slice case below).
- **Cross-slice reads are allowed via `get()`, cross-slice mutation is not.** A slice's own action may call `get()` to read another slice's state (e.g. the movie-cache slice's eviction check reads library state to know what's exempt — see [Data Caching & Persistence](/engineering/data-caching-and-persistence.md)) — but it still only changes state by calling that other slice's own exposed action, never by reassigning its state directly. This is what makes the eviction logic possible without scattering it across UI components, while keeping each slice's own state changes centralised in one place.
- These store-access rules aren't enforced by tooling yet — relying on manual discipline for now (solo project); a lint rule to catch violations automatically is deferred post-MVP, see [Engineering — Future Considerations](/engineering/future-considerations.md).
- **Adding a search result to the library only touches the user-library slice's state.** The movie cache slice ([Data Caching & Persistence](/engineering/data-caching-and-persistence.md)) is the single source of movie data and is already populated by the time a result is on screen — the library slice just saves a status/dates reference by OMDb ID, it doesn't duplicate the movie data. There's no second slice to keep in sync for this action, and no rollback logic needed: the cache's own lookup-with-fallback behaviour makes any cache miss self-healing on the next read.

## Suspense boundaries

Two places suspend, both matching designs that already exist:

- **OMDb search results** — the results area is wrapped in a Suspense boundary, with [Search In Flight](/design/screens/omdb-search/search-in-flight.md) as its fallback.
- **Initial local-library load** — wrapped in a Suspense boundary on app start, matching the library loading treatment before data is available (see [Library Views](/design/screens/library-views/index.md)).

Both resolve or throw (paired with an error boundary) rather than exposing manual `isLoading`/`error` booleans in component state — keep async data-fetching state out of components entirely. **This rule is scoped to these two reads specifically** — it's not a blanket ban on local loading state elsewhere; see [Write state (dialogs)](#write-state-dialogs) below for the one other place async state exists in the app.

Each boundary is scoped to its own section, not one app-wide catch-all — a failure in one never takes down the other, or any other part of the app:

- **OMDb search results boundary** wraps only the results area. Its error fallback renders [No Results & Errors](/design/screens/omdb-search/no-results-and-errors.md)' relevant state (network / other API / invalid key / rate-limit) in place of the results list — search input, nav, and library views are unaffected. Retry is just running a new search (per [Searching the OMDb](/product/user-journeys/searching-omdb.md#user-invokes-a-second-search-with-previous-results-present)), which re-triggers the boundary rather than needing a dedicated "reset" affordance.
- **Initial library load boundary** wraps only the list area of each library view. Its error fallback renders [Read Failure State](/design/screens/library-views/read-failure-state.md), whose "Try Again" button re-attempts the load and resets the boundary.

## Write state (dialogs)

Every write ([Movie Actions Dialog](/design/components/movie-actions-dialog.md#while-saving), [Add API Key Dialog](/design/components/add-api-key-dialog.md#while-checking)) needs its own "saving" / "failed to save" state to drive the blocking spinner and inline failure message specified in [Data Persistence Errors](/product/user-journeys/data-persistence-errors.md#write-failure-saving-a-change). Confirmed 2026-07-03: this is **plain local component state** (`useState` for `isSaving` and an error message) inside the dialog itself — not the `ui-slice`, and not a Suspense boundary.

- **Not `ui-slice`:** this state is read by exactly one component (the dialog that owns it), never shared across the app, and doesn't need to survive the dialog unmounting. Putting it in the global store would be global state for a purely local concern, without the cross-slice `get()` need that justified Zustand for the movie-cache/library eviction case above.
- **Not Suspense:** the two Suspense boundaries above cover *reads* that gate what renders. A write's loading/error state doesn't gate rendering the same way — the dialog stays mounted and visible throughout (see each dialog's own "while saving" spec) — so there's nothing to suspend.
- On success, the dialog's local state is irrelevant again — it closes and unmounts. On failure, the same local state holds the inline message until the user retries or cancels.

## Memoisation

- Wrap components rendered in lists (`MovieCard` inside library/search-results lists) in `React.memo` — props are stable per item.
- Derive expensive computed values (filtered/sorted lists) with `useMemo`, keyed on the actual store state slice that changed.
- **Selectors do the heavy lifting Zustand already gives you.** Always subscribe with a selector (`useStore(state => state.userLibrary.entries)`), never the whole store (`useStore()` with no selector) — the former only re-renders on a change to that specific piece of state, the latter re-renders on every store update regardless of what the component actually reads. A component that only calls actions (never reads state) doesn't need to subscribe to anything at all.

See also: [Codebase Conventions](/engineering/onboarding/codebase-conventions.md) for how slices map to actual files, [Data Caching & Persistence](/engineering/data-caching-and-persistence.md) for the movie-cache and user-library slice shapes specifically, and [Design-to-Code Mapping](/engineering/design-to-code-mapping.md) for how design docs become components that read from this store.
