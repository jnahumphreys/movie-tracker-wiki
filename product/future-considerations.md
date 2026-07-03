---
type: Future Consideration
title: Product — Future Considerations
description: Product scope explicitly deferred beyond MVP.
tags: [product, future]
timestamp: 2026-07-03T00:00:00Z
status: reference only
---

⬅ [Product](/product/index.md)

# Future Considerations (Product)

> NOTE: these are not to be worked on now. Included for reference, to ensure the MVP implementation stays extensible.

- Add an "All Movies" view combining `to watch` and `watched` into one list, with search across the entire library at once (both statuses combined) — MVP search is scoped to one status at a time. See [Searching the User's Library](/product/user-journeys/searching-library.md).
- Add a clear/reset (×) button to search inputs — library search ([To Watch View](/design/screens/library-views/to-watch-view.md), [Watched View](/design/screens/library-views/watched-view.md)) and [OMDb Search](/design/screens/omdb-search/search-input.md). MVP requires backspacing manually or navigating away to reset a query.
- Load more than 10 OMDb search results via lazy-loading (infinite scroll), replacing the MVP's fixed 10-result cap and its "top 10 picks" truncation hint — see [Search Results](/design/screens/omdb-search/search-results.md#truncation-hint).
- Add a manual light/dark theme toggle in the UI, overriding the system setting — MVP strictly follows the OS setting only, with no in-app control. See [Tokens & Theming](/design/foundations/tokens-and-theming.md).
- Migrate user data from local storage to a backend database
- Progressively enhance the UI to support viewports above mobile
- Allow users to create custom lists
- Introduce a dashboard detailing helpful metrics
- Internationalisation / translation support
- Integrate AI natural language queries against a user's library via a chat interface
- Support offline use, with re-sync when a network connection is available
- Allow a user to see how much storage space their library is using (via the local storage API)
- Mask the OMDb API key input (password-style, with a show/hide toggle) in the [Add API Key Dialog](/design/components/add-api-key-dialog.md)
- Auto-retry the retained search query after a successful key fix from a search error, instead of requiring a manual re-search — see [Add API Key Dialog](/design/components/add-api-key-dialog.md#relationship-to-search-recovery).
- Add a dedicated "try again" action in the Add API Key Dialog's failure state that re-validates the same unchanged key (useful for transient network failures) — MVP only lets the user edit-and-resubmit via "Save key". See [Add API Key Dialog](/design/components/add-api-key-dialog.md#failure--invalid-key).
- Surface the date/time a movie was added and its status last changed, somewhere in the UI (moved out of MVP — see [Movie Card](/design/components/movie-card.md)). The underlying data should still be tracked/stored at MVP so this is a UI-only addition later, not a data migration.
- Add a two-step "are you sure?" confirmation before deleting/removing a movie in the [Movie Actions Dialog](/design/components/movie-actions-dialog.md) — MVP performs deletion immediately on tap.
- Move local-storage writes (add/reassign/delete a movie, save an API key) from the MVP's blocking model (dialog waits for the write to succeed before closing) to an optimistic pattern — dialog closes immediately, change applied instantly, with a toast/snackbar notification only on the rare failure case. Needs a toast system and revert logic that MVP doesn't have time for. See [Data Persistence Errors](/product/user-journeys/data-persistence-errors.md#write-failure-saving-a-change).
