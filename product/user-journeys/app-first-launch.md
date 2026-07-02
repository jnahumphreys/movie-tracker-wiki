---
type: User Journey
title: App First Launch
description: Expected behaviour for a user's first launch of the app.
tags: [product, user-journey]
timestamp: 2026-07-02T00:00:00Z
status: confirmed
---

⬅ [User Journeys](/product/user-journeys/index.md)

# App First Launch

1. The user is presented with the library view (scoped to `to watch`). They see the application chrome, a search bar (for searching movies), a message stating they have no movies, and a button to add an OMDb API key.
2. The user can navigate the app freely, but the search view also displays the missing-API-key message, with the same button.
3. Clicking the "add API key" button presents UI for entering the API key. The application must always check the key is present on launch and when invoking a search. If the API key is removed after first launch, indicate the missing key on the OMDb search view only — the library views continue to display normally.
4. Once the user has added an OMDb key, they can invoke an OMDb search (to add movies). A user can navigate between the `to watch`, `watched`, and `OMDb search` views, but on first launch the search input for `watched` and `to watch` is disabled — only the `OMDb search` view's search input is functional. Each view shows a "no movies" message until movies are added to the library, or a search is invoked.

Design reference: [First Launch & Empty States](/design/screens/first-launch-and-empty-states.md)
