---
type: User Journey
title: Data Persistence Errors
description: Expected behaviour when reading from or writing to local storage fails.
tags: [product, user-journey]
timestamp: 2026-07-03T00:00:00Z
status: confirmed
---

⬅ [User Journeys](/product/user-journeys/index.md)

# Data Persistence Errors

Covers the "data persistence error" case named in [MVP Requirements](/product/mvp-requirements.md) — failures reading from or writing to the browser's local storage. Cross-cutting: applies wherever the library is loaded or a change is saved, referenced from the relevant journeys rather than repeated in each.

## Read failure (loading the library)

1. On launch (and any point the app needs to (re)load the library from local storage), if that read fails, show a **distinct error state** — never the same as the normal "no movies yet" empty state. A failed read must not read to the user as "your library is empty," since that could be mistaken for data loss when it may just be a transient read error.
2. This applies per library view (`to watch` / `watched`) — whichever view(s) the user is looking at when the failure is encountered show the error state in place of their list.
3. The [OMDb search](/product/user-journeys/searching-omdb.md) view is unaffected by a read failure — search still functions independently of whether the existing library loaded successfully. (Adding a movie found via search is still subject to the write-failure behaviour below.)

## Write failure (saving a change)

Applies to every action that writes to local storage: adding a movie, reassigning status, deleting/removing a movie (see [Managing the Local Library](/product/user-journeys/managing-local-library.md) and [Searching the OMDb — Post-Search Actions](/product/user-journeys/searching-omdb.md#post-search-actions)), and saving an API key (see [Add API Key Dialog](/design/components/add-api-key-dialog.md)).

1. The UI does not keep an optimistic change standing if the underlying save fails — **the change reverts** (card/status snaps back to its prior state, a "deleted" movie reappears, an unsaved API key is not treated as saved) so the visible state never diverges from what's actually persisted.
2. An inline error message explains the save didn't stick, with a way to retry the same action.
3. This is distinct from an OMDb API error (network/auth/other) — a write failure means the *local* save failed, not the remote OMDb request. The two can be told apart in the message UI's copy, defined separately in design.

## Not yet covered
- Exact message copy and visual treatment for both states — a design task, not specified here.
