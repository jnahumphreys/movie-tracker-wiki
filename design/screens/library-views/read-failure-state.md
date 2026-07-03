---
type: Design Screen
title: Library Views — Read Failure State
description: Design for the local-storage read-failure state shown in the to-watch and watched views.
tags: [design, screen, library]
timestamp: 2026-07-03T00:00:00Z
status: confirmed
---

⬅ [Library Views](/design/screens/library-views/index.md)

# Read Failure State

Behaviour spec: [Data Persistence Errors — Read failure](/product/user-journeys/data-persistence-errors.md#read-failure-loading-the-library). Shown in place of the movie list, per view ([To Watch View](/design/screens/library-views/to-watch-view.md) / [Watched View](/design/screens/library-views/watched-view.md)), whenever loading the library from local storage fails — **never** the same as the "no movies yet" empty state, since that could read as data loss.

## Visual treatment
Same shadcn **Empty** component pattern used across [No Results & Errors](/design/screens/omdb-search/no-results-and-errors.md#visual-treatment) and [Pre-Search State](/design/screens/omdb-search/pre-search-state.md#visual-treatment): icon → title → subtitle → action button, centered in place of the list at [360px](/design/foundations/viewports-and-breakpoints.md).

**Icon:** `circle-alert` (Lucide) — reused from [No Results & Errors' "Other API error" row](/design/screens/omdb-search/no-results-and-errors.md#icon-per-state), grouping this with the app's other generic "something went wrong" alert states rather than introducing a new icon. Same muted/secondary-token background badge treatment as the other Empty states.

## Copy
- **Title:** "Reel trouble."
- **Subtitle:** "We couldn't load your library. Your movies are probably still there — give it another take."

## Retry action
- **Button:** "Try Again" — re-attempts the local storage read. Unlike the message-only OMDb error states (no results, offline, generic API error), a retry action makes sense here because a local read failure is plausibly a transient glitch rather than something the user needs to fix first (like an invalid key).
- On tap: re-attempts the read. Success replaces this state with the normal populated/empty list for that view; failure keeps this same state on screen, with the button available for further attempts.

## Scope
- Applies independently per view — if the user is on `to watch` when the read fails, only that view shows this state; `watched` shows it separately if/when the user navigates there and its own read also fails. See [Data Persistence Errors](/product/user-journeys/data-persistence-errors.md#read-failure-loading-the-library).
- Does not affect [OMDb Search](/design/screens/omdb-search/index.md) — that view functions independently of library load state.

## Spacing at 360px
Matches [No Results & Errors](/design/screens/omdb-search/no-results-and-errors.md#spacing-at-360px): 48×48px icon badge, 16px icon-to-title gap, 8px title-to-subtitle gap, 16px subtitle-to-action-button gap, 16px page horizontal margin (content width 328px).
