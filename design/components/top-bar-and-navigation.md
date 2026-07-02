---
type: Design Component
title: Top Bar & Navigation
description: Persistent chrome and view navigation.
resource: https://ui.shadcn.com/docs/components/radix/button-group#size
tags: [design, component]
timestamp: 2026-07-02T00:00:00Z
status: confirmed
---

# Top Bar & Navigation

Two-row layout at the [360px working viewport](/design/foundations/viewports-and-breakpoints.md): a persistent top bar, and a nav row beneath it.

## Top bar (persistent chrome, row 1 — sticky)
- Contains: [Lucide `clapperboard` icon](/design/foundations/iconography.md) + "Movie Tracker" as plain text in the title font. Always shown in full at 360px — never truncated or dropped to make room.
- Count badge: single badge, far right of the top bar.
  - Shows the count for whichever library view is currently active — `to watch` count while on To Watch, `watched` count while on Watched.
  - Hidden entirely on the `OMDb` view.
  - **Not shown when the library is empty** (confirmed for the first-launch state).
  - Updates instantly when the count changes — no animation/transition.
  - Capped display at `99+` for counts over 99.

## Navigation (row 2 — scrolls with content)
- Segmented button group: `to watch` | `watched` | `OMDb`.
- The active view's button switches variant from `default` to `secondary`.
- Not sticky — this row scrolls out of view with the list content, unlike the top bar above it.
