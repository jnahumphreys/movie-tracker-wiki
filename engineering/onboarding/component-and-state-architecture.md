---
type: Onboarding Guide
title: Component & State Architecture
description: How design tokens/components map to code; Zustand slice boundaries in practice.
tags: [engineering, onboarding, architecture]
timestamp: 2026-07-03T00:00:00Z
status: confirmed
---

⬅ [Onboarding](/engineering/onboarding/index.md)

# Component & State Architecture

## From design to code

Design docs specify behaviour and shadcn component mappings; this section covers how that becomes code.

- **shadcn primitives are the building blocks.** When a design doc names a shadcn component (e.g. [Movie Card](/design/components/movie-card.md) → Item + Badge), generate that primitive via the shadcn CLI into `components/ui/`, then compose it — don't hand-roll an equivalent.
- **Styling is Tailwind utility classes only**, using the token CSS variables from [Tokens & Theming](/design/foundations/tokens-and-theming.md) (`bg-background`, `text-muted-foreground`, etc.) — never hardcoded hex/oklch values in component code.
- **Spacing values in design docs are literal.** [Movie Card](/design/components/movie-card.md#spacing-at-360px)'s `p-3` / `gap-3` / `gap-1.5` etc. are the actual Tailwind classes to use, not just descriptive numbers.
- **Worked example — Movie Card:** shadcn `Item` (image variant) + two `Badge` + an outlined icon-only `Button` (Lucide `ellipsis-vertical`, `aria-label="More actions"`). One `MovieCard.tsx` in `components/`, taking a movie object and its context (library view vs. search result) as props — it doesn't reach into any store directly (see below).

## Store access pattern

State is a **single Zustand store composed of three slices** ([Architecture & Stack](/engineering/architecture-and-stack.md): movie cache, UI, user library) — one store, not three separate Providers:

- **One slice file per concern**, each exposing its own typed state and actions (e.g. the movie-cache slice exposes `cacheMovie()`, the user-library slice exposes `addToLibrary()`, `reassignStatus()`, `removeFromLibrary()`). Components read via a selector — `useStore(state => state.movieCache.entries)` — and only re-render when the selected slice of state actually changes; no Provider, no Context, no manual memoised-selector plumbing to write yourself.
- **A slice's actions are the only sanctioned way to change its own state.** Never mutate slice state directly from a component or from another slice — always go through the owning slice's exposed action, even when that call happens from inside another slice (see cross-slice case below).
- **Cross-slice reads are allowed via `get()`, cross-slice mutation is not.** A slice's own action may call `get()` to read another slice's state (e.g. the movie-cache slice's eviction check reads library state to know what's exempt, see [persisted data shape](/engineering/architecture-and-stack.md#persisted-data-shape)) — but it still only changes state by calling that other slice's own exposed action, never by reassigning its state directly. This is what makes the eviction logic possible without scattering it across UI components, while keeping each slice's own state changes centralised in one place.
- These store-access rules aren't enforced by tooling yet — relying on manual discipline for now (solo project); a lint rule to catch violations automatically is deferred post-MVP, see [Engineering — Future Considerations](/engineering/future-considerations.md).
- **Adding a search result to the library only touches the user-library slice's state.** The movie cache slice ([persisted data shape](/engineering/architecture-and-stack.md#persisted-data-shape)) is the single source of movie data and is already populated by the time a result is on screen — the library slice just saves a status/dates reference by OMDb ID, it doesn't duplicate the movie data. There's no second slice to keep in sync for this action, and no rollback logic needed: the cache's own lookup-with-fallback behaviour makes any cache miss self-healing on the next read.

## Suspense boundaries

Two places suspend, both matching designs that already exist:

- **OMDb search results** — the results area is wrapped in a Suspense boundary, with [Search In Flight](/design/screens/omdb-search/search-in-flight.md) as its fallback.
- **Initial local-library load** — wrapped in a Suspense boundary on app start, matching the library loading treatment before data is available (see [Library Views](/design/screens/library-views/index.md)).

Both resolve or throw (paired with an error boundary) rather than exposing manual `isLoading`/`error` booleans in component state — keep async data-fetching state out of components entirely.

Each boundary is scoped to its own section, not one app-wide catch-all — a failure in one never takes down the other, or any other part of the app:

- **OMDb search results boundary** wraps only the results area. Its error fallback renders [No Results & Errors](/design/screens/omdb-search/no-results-and-errors.md)' relevant state (network / other API / invalid key / rate-limit) in place of the results list — search input, nav, and library views are unaffected. Retry is just running a new search (per [Searching the OMDb](/product/user-journeys/searching-omdb.md#user-invokes-a-second-search-with-previous-results-present)), which re-triggers the boundary rather than needing a dedicated "reset" affordance.
- **Initial library load boundary** wraps only the list area of each library view. Its error fallback renders [Read Failure State](/design/screens/library-views/read-failure-state.md), whose "Try Again" button re-attempts the load and resets the boundary.

## Memoisation

- Wrap components rendered in lists (`MovieCard` inside library/search-results lists) in `React.memo` — props are stable per item.
- Derive expensive computed values (filtered/sorted lists) with `useMemo`, keyed on the actual store state slice that changed.
- **Selectors do the heavy lifting Zustand already gives you.** Always subscribe with a selector (`useStore(state => state.userLibrary.entries)`), never the whole store (`useStore()` with no selector) — the former only re-renders on a change to that specific piece of state, the latter re-renders on every store update regardless of what the component actually reads. A component that only calls actions (never reads state) doesn't need to subscribe to anything at all.
