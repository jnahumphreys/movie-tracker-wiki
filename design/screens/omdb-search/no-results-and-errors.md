---
type: Design Screen
title: OMDb Search — No Results & Errors
description: No-results and error state design for OMDb search.
tags: [design, screen, omdb-search]
timestamp: 2026-07-02T00:00:00Z
status: in-progress
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

## Not yet covered
- Visual treatment (uses the shadcn **Empty** component) — icon choice and spacing per state
