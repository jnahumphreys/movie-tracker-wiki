---
type: Design Screen
title: To Watch View
description: Design for the to-watch library view.
tags: [design, screen, library]
timestamp: 2026-07-02T00:00:00Z
status: confirmed
---

# To Watch View

## Populated layout

Below the [top bar and nav row](/design/components/top-bar-and-navigation.md), in scroll order:

1. **Search input** — scrolls away with the list (not sticky). shadcn Input with a leading Lucide `search` icon (decorative, `aria-hidden="true"`), no search button (filtering is live). Placeholder: "Search your to-watch list...". The input has its own visually-hidden `<label>` ("Search your to-watch list") — the placeholder alone isn't a reliable accessible name, see [Accessibility](/design/foundations/accessibility.md#accessible-names-for-icon-only-controls). Disabled while the view has zero movies, enabled once at least one movie is present. No dedicated clear button at MVP — backspace manually or navigate away to reset; a clear (×) button is a considered, deferred addition, see [Future Considerations](/product/future-considerations.md).
2. **Movie Card list** — each [Movie Card](/design/components/movie-card.md) rendered as its own bordered container (shadcn Item with image), with a balanced gap between cards — not a plain divided list.

**Sort order:** most recently status-changed first (i.e. the most recently marked "to watch" appears at the top).

## Empty state
Zero movies in `to watch` — reuses the copy and `clapperboard` icon treatment from [First Launch & Empty States](/design/screens/first-launch-and-empty-states.md#visual-treatment) ("Cue the lights... and action!"), whether this is a true first launch or the list has just been emptied out later (e.g. everything moved to watched).

## "No matches" state (active search, no results)
Copy shared with [OMDb Search — No Results & Errors](/design/screens/omdb-search/no-results-and-errors.md): "Plot Twist... no Results found!" / "We couldn't find a movie with that title."

## Read failure state
Distinct from the empty state above — see [Read Failure State](/design/screens/library-views/read-failure-state.md) for the full design, shared with [Watched View](/design/screens/library-views/watched-view.md).

## Spacing at 360px
- Page horizontal margin: **16px** each side (content width 328px).
- Gap between the search input and the card list: **16px**.
- Vertical gap between cards: **12px** (`gap-3`).
- Card-internal spacing (poster size, padding, badge gaps): see [Movie Card — Spacing at 360px](/design/components/movie-card.md#spacing-at-360px). Same values reused by [OMDb Search — Search Results](/design/screens/omdb-search/search-results.md) and [Watched View](/design/screens/library-views/watched-view.md).
