---
type: Design Component
title: Movie Card
description: Shared card used across library and search views.
resource: https://ui.shadcn.com/docs/components/radix/item#image
tags: [design, component]
timestamp: 2026-07-02T00:00:00Z
status: confirmed
---

# Movie Card

Used consistently across `to watch`, `watched`, and `OMDb search` views. Maps to the shadcn **Item** component (Image variant), laid out as a horizontal list item.

## Layout
- Poster thumbnail on the left.
- Text block to the right of the poster:
  - **Title** on its own line.
  - Below it, two [Badge](https://ui.shadcn.com/docs/components/radix/badge) components: one for release year, one for runtime (e.g. `1985` and `116 min` as separate badges).
- Action button far right: icon-only, **outlined** variant, Lucide `ellipsis-vertical` icon, `aria-label="More actions"` (see [Accessibility — Accessible names for icon-only controls](/design/foundations/accessibility.md#accessible-names-for-icon-only-controls)). Opens the [Movie Actions Dialog](/design/components/movie-actions-dialog.md) — the specific actions offered depend on context (library view vs. OMDb search result), per [Managing the Local Library](/product/user-journeys/managing-local-library.md) and [Searching the OMDb](/product/user-journeys/searching-omdb.md#post-search-actions).

## Poster fallback
- When OMDb returns no poster (`"N/A"`), show the [Lucide `clapperboard` icon](/design/foundations/iconography.md) centered on a muted/secondary-token background block, matching the real poster's dimensions.

## Accessibility
- Real poster image: `alt="[Movie Title] poster"` — concise, avoids redundant "movie poster for the movie X" phrasing.
- Poster fallback block: decorative — empty `alt=""` / `aria-hidden="true"`, not a description of a missing image. The adjacent title text already conveys the movie's identity, so nothing is lost.

## Spacing at 360px
- Poster thumbnail: fixed **56×84px** (2:3 ratio), regardless of container width.
- Card internal padding: **12px** (`p-3`) on all sides.
- Gap between poster and text block: **12px** (`gap-3`).
- Gap between title and the badge row: **4px** (`gap-1`).
- Gap between the two badges: **6px** (`gap-1.5`).
- Action button tap area: **44×44px minimum**, per [Accessibility — Touch target sizing](/design/foundations/accessibility.md#touch-target-sizing) — the icon itself renders smaller and is centred within this tap area.

## Not shown on the card
- Added/status-changed timestamps — deferred to [Future Considerations](/product/future-considerations.md); data is still tracked internally, just not surfaced in any UI yet.

See also: [Component & State Architecture](/engineering/onboarding/component-and-state-architecture.md#from-design-to-code) for how this maps to actual shadcn primitives and code structure.
