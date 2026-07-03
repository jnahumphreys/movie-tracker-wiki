---
type: User Journey
title: App First Launch
description: Expected behaviour for a user's first launch of the app.
tags: [product, user-journey]
timestamp: 2026-07-03T00:00:00Z
status: confirmed
---

⬅ [User Journeys](/product/user-journeys/index.md)

# App First Launch

1. The user is presented with the library view (scoped to `to watch`). They see the application chrome, a search bar (for searching movies), and a message stating they have no movies, with a button that takes them to the `OMDb search` view. This is the same button and destination whether or not an API key is already set — the library views never mention the API key themselves. This "no movies" message assumes the library genuinely loaded and is empty — see [Data Persistence Errors](/product/user-journeys/data-persistence-errors.md#read-failure-loading-the-library) for the distinct, separate behaviour when the load itself fails.
2. The user can navigate the app freely. Each view shows its own "no movies" message until movies are added to the library.
3. On the `OMDb search` view, if no key is stored, it shows a message prompting for one, with a button that opens the [Add API Key Dialog](/design/components/add-api-key-dialog.md) — the dialog does not open automatically. A search cannot be attempted until a key is set. Saving a key requires it to pass a live check against OMDb — see the dialog's own behaviour spec for the success/failure states.
4. **Key presence is only checked at the point of use** (navigating to or acting on the `OMDb search` view) — the app does not proactively re-validate a stored key on every launch or navigation. A key that's present but has since gone invalid is only discovered when a real search against it fails; that failure's error state offers the same Add API Key Dialog as a fix, see [Searching the OMDb](/product/user-journeys/searching-omdb.md#error).
5. Once the user has a working OMDb key, they can invoke an OMDb search (to add movies). A user can navigate between the `to watch`, `watched`, and `OMDb search` views. The search input for `watched` and `to watch` follows a general rule, not a first-launch-only one: **disabled whenever that view has zero movies, enabled once at least one is present** — first launch is just the first instance of this, not a special case. The `OMDb search` view's search input is functional as soon as a key is set, independent of library contents.
6. **`watched` empty state gets a conditional second entry point.** Like `to watch`, its empty state always offers a "search for a movie" CTA to `OMDb search`. If `to watch` already has movies, a second button is also shown that navigates to the `to watch` view — a plain view switch, not a new picker or dialog — so the user can reassign an existing to-watch movie to `watched` via the ordinary [Movie Actions Dialog](/design/components/movie-actions-dialog.md) flow. This exists because a movie can reach `watched` two ways (direct add via search, or reassignment from `to watch`), so its empty state offers a path into both rather than just the reassignment one.

Design reference: [First Launch & Empty States](/design/screens/first-launch-and-empty-states.md), [Watched View](/design/screens/library-views/watched-view.md#empty-state)
