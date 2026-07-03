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

Design reference: [Read Failure State](/design/screens/library-views/read-failure-state.md).

## Write failure (saving a change)

Applies to every action that writes to local storage: adding a movie, reassigning status, deleting/removing a movie (see [Managing the Local Library](/product/user-journeys/managing-local-library.md) and [Searching the OMDb — Post-Search Actions](/product/user-journeys/searching-omdb.md#post-search-actions)), and saving an API key (see [Add API Key Dialog](/design/components/add-api-key-dialog.md)).

**MVP model: blocking, not optimistic.** The triggering dialog (Movie Actions Dialog or Add API Key Dialog) shows a loading state while the write is in progress and does not close until it resolves — matching the Add API Key Dialog's existing "while checking" pattern. Nothing is shown to the user as changed until the write is actually confirmed, so there's no visible state to revert.

1. **On success:** the dialog closes and the list/card reflects the new state — this is the only point at which anything visibly changes.
2. **On failure:** the dialog stays open and shows an inline failure message explaining the save didn't stick, with a way to retry the same action (mirrors the dialog's own failure-state pattern, e.g. the Add API Key Dialog's invalid-key/network-issue states). The list/card the user came from is untouched throughout, since nothing was applied until success.
3. This is distinct from an OMDb API error (network/auth/other) — a write failure means the *local* save failed, not the remote OMDb request. The two can be told apart in the message UI's copy, defined separately in design.
4. **Post-MVP:** move to an optimistic pattern instead — dialog closes immediately on tap, change applied instantly, with a toast/snackbar notification only in the rare failure case. Deferred because it needs supporting infrastructure (a toast system, revert logic) beyond MVP scope; a brief blocking wait now was judged better than a silent revert with no user feedback. See [Future Considerations](/product/future-considerations.md).

Design reference: [Movie Actions Dialog — While saving / Failure](/design/components/movie-actions-dialog.md#while-saving) and [Add API Key Dialog — Failure, couldn't save key](/design/components/add-api-key-dialog.md#failure--couldnt-save-key).
