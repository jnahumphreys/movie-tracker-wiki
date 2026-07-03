---
type: Design Screen
title: OMDb Search — Post-Search Actions
description: Design for the actions dialog opened from a search result.
tags: [design, screen, omdb-search]
timestamp: 2026-07-02T00:00:00Z
status: confirmed
---

# Post-Search Actions

Behaviour spec: [Post-Search Actions](/product/user-journeys/searching-omdb.md#post-search-actions). Opens the [Movie Actions Dialog](/design/components/movie-actions-dialog.md) — copy and variants (not in library / already in library) fully specified there.

## Card state after an action
Once an action completes (add / reassign / remove), the acted-upon [Movie Card](/design/components/movie-card.md) **updates in place within the existing results list** — it does not disappear and the user does not need to re-search:
- **Add:** card gains the "In your library" + status badge treatment (per [Movie Actions Dialog — Title](/design/components/movie-actions-dialog.md#title)), and its action button now opens the "already in library" dialog variant.
- **Reassign:** the status badge updates to the new status.
- **Remove:** the "In your library" + status badge treatment is dropped, and the action button reverts to the "not in library" dialog variant.

This keeps the results list stable so repeated actions across multiple cards don't cause confusion about what's already been done.
