---
type: Design Screen
title: OMDb Search — Pre-Search State
description: Empty-state design for the OMDb search results area before any search has been made.
tags: [design, screen, omdb-search]
timestamp: 2026-07-03T00:00:00Z
status: confirmed
---

# Pre-Search State

Behaviour spec: [Searching the OMDb to Add a Movie](/product/user-journeys/searching-omdb.md). Covers the results area before the user has made their first search — separate from the [Search Input](/design/screens/omdb-search/search-input.md)'s own placeholder copy, which is confirmed independently. Two variants, depending on whether an API key is stored — see [App First Launch](/product/user-journeys/app-first-launch.md).

## Variant: key set (normal pre-search)

### Copy
- **Title:** "Lights, camera... search!"
- **Subtitle:** "Find a movie to add to your library."

### Visual treatment
Same shadcn **Empty** component pattern as [No Results & Errors](/design/screens/omdb-search/no-results-and-errors.md#visual-treatment): icon → title → subtitle, centered in the results area at [360px](/design/foundations/viewports-and-breakpoints.md). No action button — the search input above is the only call to action.

**Icon:** `clapperboard` (Lucide), on the same muted/secondary-token background block used for the [Movie Card poster fallback](/design/components/movie-card.md#poster-fallback) and the other OMDb search empty states, for visual consistency.

## Variant: no key set

Shown **immediately on landing** when no key is stored — before any search is attempted, not just as a result of a failed search. Reuses the title, icon, and action from [No Results & Errors' "Invalid API key" row](/design/screens/omdb-search/no-results-and-errors.md#copy), but with its own subtitle — this is a genuinely *missing* key, not an invalid one, so "add" is the accurate framing here (see that doc's note on why the two no longer share subtitle copy):
- **Title:** "Cut! That's a blooper."
- **Subtitle:** "Add your OMDb API key to keep the cameras rolling."
- **Icon:** `key-round`, same muted/secondary-token background block treatment.
- **Action button:** "Add API key" → opens the [Add API Key Dialog](/design/components/add-api-key-dialog.md).
- The [search input](/design/screens/omdb-search/search-input.md) above is disabled in this variant — there's nothing to search yet.

## Spacing at 360px
Both variants match [No Results & Errors](/design/screens/omdb-search/no-results-and-errors.md#spacing-at-360px): 48×48px icon badge, 16px icon-to-title gap, 8px title-to-subtitle gap, 16px subtitle-to-action-button gap (no-key variant only), 16px page horizontal margin (content width 328px).
