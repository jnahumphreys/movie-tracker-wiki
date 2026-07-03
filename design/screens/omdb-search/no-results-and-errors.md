---
type: Design Screen
title: OMDb Search — No Results & Errors
description: No-results and error state design for OMDb search.
tags: [design, screen, omdb-search]
timestamp: 2026-07-03T00:00:00Z
status: confirmed
---

# No Results & Errors

Behaviour spec: [No Results Found](/product/user-journeys/searching-omdb.md#no-results-found) and [Error](/product/user-journeys/searching-omdb.md#error). Retains the search query; message UI communicates no-results vs. network/API/auth errors.

## Copy

### No results found
Also shared with the `to watch` / `watched` "no matches" search state — see [To Watch View](/design/screens/library-views/to-watch-view.md) / [Watched View](/design/screens/library-views/watched-view.md).
- **Title:** "Plot Twist... no Results found!"
- **Subtitle:** "We couldn't find a movie with that title."

### Errors
OMDb-search-specific — covers network, API, and auth/key errors per the behaviour spec above.

| Cause | Title | Subtitle |
| --- | --- | --- |
| Missing/invalid API key | "Cut! That's a blooper." | "Add your OMDb API key to keep the cameras rolling." |
| No connection | "Who turned out the lights?" | "Looks like you're offline. Check your connection and try again." |
| Other API error | "Cut! That's a blooper." | "Something went wrong on our end. Give it another take." |

## Visual treatment
All four states use the shadcn **Empty** component, centered in the results area at [360px](/design/foundations/viewports-and-breakpoints.md): icon → title → subtitle, with an optional action button.

### Icon per state
Icon sits on the same muted/secondary-token background block used for the [Movie Card poster fallback](/design/components/movie-card.md#poster-fallback), for visual consistency.

| State | Lucide icon |
| --- | --- |
| No results found | `search-x` |
| Missing/invalid API key | `key-round` |
| No connection | `wifi-off` |
| Other API error | `circle-alert` |

### Action button
Only the missing/invalid API key state gets an action: an **"Add API key"** button in the Empty component's action slot, opening the [Add API Key Dialog](/design/components/add-api-key-dialog.md) directly from the error — matching that dialog's own note that it's shown "wherever a key is currently missing... or the OMDb view if a previously-saved key is gone." The other three states are message-only (no button).

### Spacing at 360px
- Icon badge: 48×48px, centered.
- Icon-to-title gap: 16px.
- Title-to-subtitle gap: 8px.
- Subtitle-to-action-button gap (missing-key state only): 16px.
- Page horizontal margin: 16px each side (content width 328px), matching [Search Results](/design/screens/omdb-search/search-results.md#spacing-at-360px).
