---
type: Onboarding Guide
title: Component & State Architecture
description: How design tokens/components map to code; Context+Reducer store boundaries in practice.
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

Each of the three stores ([Architecture & Stack](/engineering/architecture-and-stack.md): movie cache, UI, user library) follows the same shape:

- **Two Contexts per store**, not one — a state Context and a dispatch Context. This is what makes "memoised store selectors" (per Architecture & Stack) achievable with plain Context+Reducer: a component that only dispatches never re-renders when state changes, and vice versa.
- **Two hooks per store**, the only sanctioned way to touch it — e.g. `useMovieCacheState()` and `useMovieCacheDispatch()`. Never call `useContext` directly in a component; always go through the store's own hook (see [Codebase Structure & Conventions](/engineering/onboarding/codebase-structure-and-conventions.md)).
- Each hook throws if called outside its Provider — fail loudly, not silently with `undefined`.
- Components don't reach into another store's reducer. Cross-store effects (e.g. adding a search result to the library) go through the consuming component/hook calling each store's own dispatch — never one reducer calling another.
- These store-access rules aren't enforced by tooling yet — relying on manual discipline for now (solo project); a lint rule to catch violations automatically is deferred post-MVP, see [Engineering — Future Considerations](/engineering/future-considerations.md).
- **Adding a search result to the library only writes to the user-library store.** The movie cache store ([persisted data shape](/engineering/architecture-and-stack.md#persisted-data-shape)) is the single source of movie data and is already populated by the time a result is on screen — the library store just saves a status/dates reference by OMDb ID, it doesn't duplicate the movie data. There's no two-store write to keep in sync for this action, and no rollback logic needed: the cache's own lookup-with-fallback behaviour makes any cache miss self-healing on the next read.

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
- Derive expensive computed values (filtered/sorted lists) with `useMemo`, keyed on the actual store state slice that changed. This is also why the state/dispatch context split above matters: without it, every store update would re-render every consumer regardless of what it actually reads.
