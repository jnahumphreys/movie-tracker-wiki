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

1. **Search input** — same treatment (shadcn Input, leading `search` icon, no button, scrolls with the list, disabled when empty). Placeholder: "Search your watched list...". No dedicated clear button at MVP — see [To Watch View](/design/screens/library-views/to-watch-view.md#populated-layout) / [Future Considerations](/product/future-considerations.md).
2. **Movie Card list** — same bordered-card treatment with balanced gaps.

**Sort order:** most recently status-changed first (i.e. the most recently marked "watched" appears at the top).

## Empty state
Zero movies in `watched` (regardless of whether `to watch` has movies):
- **Title:** "That's a wrap!"
- **Subtitle:** "Nothing here yet — mark a movie as watched to start your reel."

## "No matches" state (active search, no results)
Copy shared with [OMDb Search — No Results & Errors](/design/screens/omdb-search/no-results-and-errors.md): "Plot Twist... no Results found!" / "We couldn't find a movie with that title."

## Spacing at 360px
Same values as [To Watch View](/design/screens/library-views/to-watch-view.md#spacing-at-360px) — 16px page margins, 16px gap below the search input, 12px gap between cards, card-internal spacing per [Movie Card](/design/components/movie-card.md#spacing-at-360px).
