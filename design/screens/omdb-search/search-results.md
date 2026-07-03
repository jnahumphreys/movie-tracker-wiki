---
type: Design Screen
title: OMDb Search — Search Results
description: Search results list design.
tags: [design, screen, omdb-search]
timestamp: 2026-07-03T00:00:00Z
status: confirmed
---

# Search Results

Behaviour spec: [Return of Search Results](/product/user-journeys/searching-omdb.md#return-of-search-results). Up to 10 results, shown as [Movie Cards](/design/components/movie-card.md).

## Layout
Reuses the same list pattern already established for the library views (see [To Watch View](/design/screens/library-views/to-watch-view.md#populated-layout)): each [Movie Card](/design/components/movie-card.md) rendered as its own bordered container (shadcn Item with image), with a balanced gap between cards — not a plain divided list. The list scrolls below the [search input](/design/screens/omdb-search/search-input.md), same as the library views.

## Truncation hint
When a search returns the full 10 results (i.e. more may exist beyond the cap), show a hint below the list:
- **Copy:** "Showing our top 10 picks — try a more specific title if you don't see it."
- Not shown when the result count is under 10 — that means every match is already visible, so there's nothing to hint at.
- **Post-MVP:** the 10-result cap is planned to be replaced with lazy-loading (infinite scroll) to surface more results — see [Future Considerations](/product/future-considerations.md). This hint is an MVP-only stopgap.

### Visual treatment
- Text-only message block, **full width (328px, matching the movie cards above it)**, text centered within — reads as part of the list rather than a separate floating element. (Unlike the [No Results & Errors](/design/screens/omdb-search/no-results-and-errors.md) Empty component, which is a narrower, shrink-wrapped block — that pattern doesn't apply here.)
- **Background:** same muted/secondary-token background used for the [Movie Card poster fallback](/design/components/movie-card.md#poster-fallback) and the No Results & Errors icon badges, for token consistency.
- **Text color:** regular `foreground` (full-contrast body text), not `muted-foreground` — avoids the light-mode AA contrast gap flagged in [Accessibility](/design/foundations/accessibility.md#known-gaps-flagged-required-follow-up).
- **Block padding:** 12px vertical / 16px horizontal (matches [Movie Card](/design/components/movie-card.md#spacing-at-360px)'s internal padding for consistency).
- **Placement:** 16px gap below the last card in the list (matches the search-input-to-list gap below).

## Spacing at 360px
- Page horizontal margin: **16px** each side (content width 328px).
- Gap between the search input and the results list: **16px**.
- Vertical gap between cards: **12px** (`gap-3`).
- Card-internal spacing (poster size, padding, badge gaps): see [Movie Card — Spacing at 360px](/design/components/movie-card.md#spacing-at-360px).
