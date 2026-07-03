---
type: Product Requirement
title: MVP Requirements
description: Functional scope for the Movie Tracker MVP.
tags: [product, mvp]
timestamp: 2026-07-03T00:00:00Z
status: confirmed
---

⬅ [Product](/product/index.md)

# MVP Requirements

- A user can add movies to their personal library by searching for movies by title using the OMDb API.
- A user can assign a status to a movie: either `watched` or `to watch`.
- A user can reassign a movie's status: to `to watch` if the current status is `watched`, and to `watched` if the current status is `to watch`.
- A user can delete a movie from their library.
- A user can search their library, scoped to a specific status (`watched` or `to watch`). Searching across the entire library at once (both statuses combined) is a post-MVP feature — see [Future Considerations](/product/future-considerations.md).
- A user can view metadata for a movie (both in their library and in OMDb search results), specifically:
  - Poster image (with a fallback when OMDb has none)
  - Movie title
  - Release year
  - Runtime (in minutes)
- A user can provide their OMDb API key via the application, entered from the OMDb search view. The key is validated live against OMDb before being saved.
- A user's library persists across sessions, using the browser's local storage API.
- A user can use the application across major common viewports: mobile, tablet, laptop, and desktop monitor.
- The UI will support light and dark mode, matching the user's system settings.
- The UI will prioritise mobile and touch-first interaction, as this is the core demographic.
- The UI must take into consideration accessibility requirements, as detailed in the [WCAG 2.1](https://www.w3.org/TR/WCAG21/) specification.
- The UI will indicate to the user when:
  - They have no movies in their library
  - Something has gone wrong (device offline, data persistence error, API error) — see [Data Persistence Errors](/product/user-journeys/data-persistence-errors.md) for the local-storage read/write cases specifically
  - A search returns no results
  - A search is in flight

See also: [Future Considerations](/product/future-considerations.md) for scope explicitly deferred beyond MVP.
