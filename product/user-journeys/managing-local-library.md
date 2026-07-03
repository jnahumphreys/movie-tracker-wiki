---
type: User Journey
title: Managing the Local Library
description: Expected behaviour for editing or removing movies already in the library.
tags: [product, user-journey]
timestamp: 2026-07-03T00:00:00Z
status: confirmed
---

⬅ [User Journeys](/product/user-journeys/index.md)

# Managing the Local Library

- From either the `to watch` or `watched` view, a user can click a movie's action button and be presented with the actions: **delete movie**, **mark as watched** (if the movie's current status is `to watch`), or **mark as to watch** (if the movie's current status is `watched`), or **cancel**.
- **The dialog blocks while saving:** picking an action shows a brief loading state in the dialog and waits for the write to actually succeed before closing — it does not close optimistically. See [Data Persistence Errors](/product/user-journeys/data-persistence-errors.md#write-failure-saving-a-change) for the full model, the failure case, and why (a deliberate MVP trade-off, revisited post-MVP).
- **No transition on removal:** once the write succeeds and the dialog closes, a movie that no longer matches the current view's status (reassigned away, or deleted) is simply absent from the list on next render — no animation. The dialog closing is the confirmation that the action happened.
- **No undo.** Deletion is immediate and final at MVP — no confirmation step (see [Future Considerations](/product/future-considerations.md) for the deferred two-step confirmation) and no "undo" affordance after the fact. A mis-tap is unrecoverable at MVP.
- **Emptying out a view.** If removing or reassigning the last movie in a view leaves it with zero movies, that view just shows its standard empty-state message — the same one shown any other time it has zero movies. No special-cased transition for this specific path into the empty state.
- **No library size limit at MVP.** Unlike OMDb search's 10-result cap, `to watch` and `watched` lists render every movie with that status, full-height scroll, no pagination or virtualization. This is an explicit MVP assumption, not an oversight — revisit if it becomes a real problem with large libraries.

Design reference: [Movie Actions Dialog](/design/components/movie-actions-dialog.md)
