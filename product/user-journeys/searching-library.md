---
type: User Journey
title: Searching the User's Library
description: Expected behaviour for filtering the to-watch and watched library views.
tags: [product, user-journey]
timestamp: 2026-07-03T00:00:00Z
status: confirmed
---

⬅ [User Journeys](/product/user-journeys/index.md)

# Searching the User's Library

**MVP scope:** search is scoped to one status at a time (`to watch` or `watched`) — there is no combined, entire-library search at MVP. See [Future Considerations](/product/future-considerations.md) for the deferred "All Movies" combined view.

Default sort order (before any search is applied): most recently status-changed first — i.e. the most recently marked `to watch` or `watched` movie appears at the top of its respective view.

## Status "To Watch"

1. The user navigates to the `to watch` view.
2. The user is presented with all movies (as cards) with that status.
3. The user enters a search string (the movie title) into the search input, and the library is filtered live — without needing to click a search button. Filtering applies a short debounce (~150–300ms) after the user stops typing, rather than re-filtering on every keystroke — the list is local/in-memory so a delay isn't required for performance, but it avoids re-rendering on every character for fast typers.
4. **Match rule:** case-insensitive substring match against the movie title — the query can match anywhere in the title, not just the start (e.g. "trek" matches "Star Trek").
5. If the search has a match, display the matching movie card(s); otherwise, display a message indicating no movies match the query.
6. If the user navigates away from this view with an active search, reset the view (clear the query).

## Status "Watched"

As above, except the user navigates to the `watched` view, and both the displayed movies and the filtering are scoped to that status.

Design reference: [Library Views](/design/screens/library-views/index.md)
