---
type: Design Screen
title: OMDb Search — Search Results
description: Search results list design.
tags: [design, screen, omdb-search]
timestamp: 2026-07-03T00:00:00Z
status: confirmed
---

# Search Results

Behaviour spec: [Return of Search Results](/product/user-journeys/searching-omdb.md#return-of-search-results). Up to 10 results, shown as [Movie Cards](/design/components/movie-card.md).

## Layout
Reuses the same list pattern already established for the library views (see [To Watch View](/design/screens/library-views/to-watch-view.md#populated-layout)): each [Movie Card](/design/components/movie-card.md) rendered as its own bordered container (shadcn Item with image), with a balanced gap between cards — not a plain divided list. The list scrolls below the [search input](/design/screens/omdb-search/search-input.md), same as the library views.

## Spacing at 360px
- Page horizontal margin: **16px** each side (content width 328px).
- Gap between the search input and the results list: **16px**.
- Vertical gap between cards: **12px** (`gap-3`).
- Card-internal spacing (poster size, padding, badge gaps): see [Movie Card — Spacing at 360px](/design/components/movie-card.md#spacing-at-360px).
