---
type: Design Screen
title: Watched View
description: Design for the watched library view.
tags: [design, screen, library]
timestamp: 2026-07-02T00:00:00Z
status: confirmed
---

# Watched View

As [To Watch View](/design/screens/library-views/to-watch-view.md), scoped to `watched` status.

## Populated layout

Same structure as [To Watch View](/design/screens/library-views/to-watch-view.md#populated-layout), scoped to `watched`:

1. **Search input** — same treatment (shadcn Input, decorative `aria-hidden` leading `search` icon, no button, scrolls with the list, disabled when empty). Placeholder: "Search your watched list...". Visually-hidden `<label>`: "Search your watched list" — see [To Watch View](/design/screens/library-views/to-watch-view.md#populated-layout) / [Accessibility](/design/foundations/accessibility.md#accessible-names-for-icon-only-controls). No dedicated clear button at MVP — see [Future Considerations](/product/future-considerations.md).
2. **Movie Card list** — same bordered-card treatment with balanced gaps.

**Sort order:** most recently status-changed first (i.e. the most recently marked "watched" appears at the top).

## Empty state
Zero movies in `watched`. Same shadcn Empty component pattern as [First Launch & Empty States](/design/screens/first-launch-and-empty-states.md#visual-treatment) (icon → title → subtitle → action button(s)), but with its own icon and a CTA set that depends on whether `to watch` has movies — added to cover the product-confirmed direct-add-via-search path, which the previous copy (reassignment-only) didn't reflect.

### Copy
- **Icon:** `popcorn` (Lucide) — differentiates from To Watch/First Launch's `clapperboard`, same muted-token badge treatment.
- **Title:** "That's a wrap!"
- **Subtitle:** "Nothing here yet — search for a movie to add straight to your reel." (Same subtitle regardless of the `to watch` state below — the button set is what changes.)

### Action buttons
- **"Search for a Movie"** (primary) — always shown, identical destination and copy to [To Watch/First Launch](/design/screens/first-launch-and-empty-states.md)'s CTA: navigates to [OMDb Search](/design/screens/omdb-search/index.md).
- **"Browse Your To-Watch List"** (secondary) — shown only when `to watch` has at least one movie. Navigates to the [To Watch View](/design/screens/library-views/to-watch-view.md) (a plain view switch, same as tapping the nav bar's "to watch" button) — the user then reassigns a movie to `watched` via the existing [Movie Actions Dialog](/design/components/movie-actions-dialog.md) flow, same as any other reassignment. No new dialog or picker component — this is a shortcut into an existing flow, not a new one.
- Buttons stack full-width in that order (primary above secondary) when both are shown, matching the visual weighting convention from [Dialog Conventions](/design/components/dialog-conventions.md#button-layout) even though this isn't a dialog.

## "No matches" state (active search, no results)
Copy shared with [OMDb Search — No Results & Errors](/design/screens/omdb-search/no-results-and-errors.md): "Plot Twist... no Results found!" / "We couldn't find a movie with that title."

## Read failure state
Distinct from the empty state above — see [Read Failure State](/design/screens/library-views/read-failure-state.md) for the full design, shared with [To Watch View](/design/screens/library-views/to-watch-view.md).

## Spacing at 360px
Same values as [To Watch View](/design/screens/library-views/to-watch-view.md#spacing-at-360px) — 16px page margins, 16px gap below the search input, 12px gap between cards, card-internal spacing per [Movie Card](/design/components/movie-card.md#spacing-at-360px).
