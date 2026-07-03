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
| Invalid API key | "Cut! That's a blooper." | "That key doesn't look right — update it to keep the cameras rolling." |
| Daily request limit reached | "That's a wrap for today!" | "You've hit today's search limit for this key. Try a different key, or wait for tomorrow's reset." |
| No connection | "Who turned out the lights?" | "Looks like you're offline. Check your connection and try again." |
| Other API error | "Cut! That's a blooper." | "Something went wrong on our end. Give it another take." |

This row's title, icon, and action button are also reused as [Pre-Search State's "no key set" variant](/design/screens/omdb-search/pre-search-state.md#variant-no-key-set) — that variant is the only place a genuinely *missing* key surfaces (this table row only fires for an *invalid* one, since the search input is disabled entirely while no key is stored, see [App First Launch](/product/user-journeys/app-first-launch.md)). **The subtitle differs between the two** — "missing" and "invalid" aren't the same problem, so they no longer share subtitle copy; see that doc for its own wording.

## Visual treatment
All four states use the shadcn **Empty** component, centered in the results area at [360px](/design/foundations/viewports-and-breakpoints.md): icon → title → subtitle, with an optional action button.

### Icon per state
Icon sits on the same muted/secondary-token background block used for the [Movie Card poster fallback](/design/components/movie-card.md#poster-fallback), for visual consistency.

| State | Lucide icon |
| --- | --- |
| No results found | `search-x` |
| Invalid API key | `key-round` |
| Daily request limit reached | `hourglass` |
| No connection | `wifi-off` |
| Other API error | `circle-alert` |

### Action button
The invalid API key and daily-limit states each get an action: an **"Add API key"** button in the Empty component's action slot, opening the [Add API Key Dialog](/design/components/add-api-key-dialog.md) directly from the error — for the limit case, this is the same-session workaround (switch keys) rather than a fix to the existing key. See the dialog's own doc for its live-validation behaviour and what happens to this search on a successful fix. The other three states are message-only (no button).

### Spacing at 360px
- Icon badge: 48×48px, centered.
- Icon-to-title gap: 16px.
- Title-to-subtitle gap: 8px.
- Subtitle-to-action-button gap (invalid-key and daily-limit states only): 16px.
- Page horizontal margin: 16px each side (content width 328px), matching [Search Results](/design/screens/omdb-search/search-results.md#spacing-at-360px).
