---
type: Design Screen
title: OMDb Search — Search In Flight
description: Loading-state design while an OMDb search is in progress.
tags: [design, screen, omdb-search]
timestamp: 2026-07-03T00:00:00Z
status: confirmed
---

# Search In Flight

Behaviour spec: [Search in Flight](/product/user-journeys/searching-omdb.md#search-in-flight). Search UI disables and indicates a search is in progress; cancelled on navigation away.

## Visual treatment
- The search button's icon swaps to a spinning Lucide **`loader-circle`** (see [Iconography](/design/foundations/iconography.md)) for the duration of the search.
- The input and button both take the same disabled treatment already established in [Search Input](/design/screens/omdb-search/search-input.md) — reduced opacity, muted color — and reject focus/typing while in flight, so a query can't be edited or resubmitted mid-search.
- The message area (initial prompt, per [Pre-Search State](/design/screens/omdb-search/pre-search-state.md)) swaps to the in-flight copy below for the duration of the search.

## Copy
- **Title:** "Rolling the film..."
- **Subtitle:** "Searching for [query]..."

## Accessibility
- The message area is an `aria-live="polite"` region, so screen readers announce the in-flight title/subtitle when the state changes.
- The disabled input/button carry `aria-busy="true"` while the search is in flight.
- The spinner icon is `aria-hidden` — the live-region text is what conveys the loading state to assistive tech, not the icon itself.
- Full WCAG conformance target still open — tracked in [Accessibility](/design/foundations/accessibility.md).
