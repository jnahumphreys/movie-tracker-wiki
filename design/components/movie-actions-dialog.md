---
type: Design Component
title: Movie Actions Dialog
description: Dialog for reassigning status, removing, or deleting a movie.
resource: https://ui.shadcn.com/docs/components/radix/dialog
tags: [design, component]
timestamp: 2026-07-02T00:00:00Z
status: confirmed
---

# Movie Actions Dialog

Maps to the shadcn **Dialog** component. Behaviour (which actions appear when) is fully specified in [Searching the OMDb](/product/user-journeys/searching-omdb.md#post-search-actions) and [Managing the Local Library](/product/user-journeys/managing-local-library.md). Opened via the [Movie Card](/design/components/movie-card.md)'s action button.

## Title
- Dialog title is always the movie's title.
- If opened from an OMDb search result for a movie **already in the library**: two [Badge](https://ui.shadcn.com/docs/components/radix/badge) components appear with the title — "In your library" and the movie's current status ("To Watch" / "Watched"). This variant then behaves identically to the library-view variant below (reassign/remove/cancel) — guards against creating duplicate library entries.

## Layout
Follows [Dialog Conventions](/design/components/dialog-conventions.md#button-layout): status action(s) primary, destructive action danger, divider, Cancel secondary.

## Copy by variant

### Library view (`to watch` / `watched`) — status reassignment
- "Mark as Watched" (if current status is `to watch`) or "Mark as To Watch" (if current status is `watched`) — primary.
- "Delete Movie" — danger.
- *(divider)*
- "Cancel" — secondary.

### OMDb search result — not in library
- "Add to To Watch" — primary.
- "Add to Watched" — primary.
- *(divider)*
- "Cancel" — secondary.

Both actions are intentionally equal-weight (both `primary` variant, no visual hierarchy between them) — neither status is a more "correct" default than the other, so this is a confirmed decision, not an oversight.

### OMDb search result — already in library
- Same as "Library view" variant above: "Mark as Watched" / "Mark as To Watch" (primary), "Remove from Library" (danger), divider, "Cancel" (secondary).

## Deletion behaviour
- "Delete Movie" / "Remove from Library" act **immediately** on tap — no second confirmation step for MVP. A two-step "are you sure?" confirmation is a possible post-MVP enhancement — see [Future Considerations](/product/future-considerations.md).

## Dismiss behaviour
Follows [Dialog Conventions](/design/components/dialog-conventions.md#dismiss-behaviour) — no dialog-specific exceptions.
