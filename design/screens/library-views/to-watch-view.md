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

1. **Search input** — scrolls away with the list (not sticky). shadcn Input with a leading Lucide `search` icon, no search button (filtering is live). Placeholder: "Search your to-watch list...". Disabled while the view has zero movies, enabled once at least one movie is present.
2. **Movie Card list** — each [Movie Card](/design/components/movie-card.md) rendered as its own bordered container (shadcn Item with image), with a balanced gap between cards — not a plain divided list.

**Sort order:** most recently status-changed first (i.e. the most recently marked "to watch" appears at the top).

## Empty state
Zero movies in `to watch` — reuses the copy from [First Launch & Empty States](/design/screens/first-launch-and-empty-states.md) ("Cue the lights... and action!"), whether this is a true first launch or the list has just been emptied out later (e.g. everything moved to watched).

## "No matches" state (active search, no results)
Copy shared with [OMDb Search — No Results & Errors](/design/screens/omdb-search/no-results-and-errors.md): "Plot Twist... no Results found!" / "We couldn't find a movie with that title."

## Not yet covered
- Exact spacing/sizing at 360px
