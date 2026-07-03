---
type: Design Foundation
title: Accessibility
description: Accessibility conformance target and outstanding checks.
tags: [design, foundations, accessibility]
timestamp: 2026-07-03T00:00:00Z
status: confirmed
---

# Accessibility

Requirement from [MVP Requirements](/product/mvp-requirements.md): designs must take into account **WCAG 2.1** accessibility requirements.

## Conformance target
**WCAG 2.1 AA** — the standard baseline for most apps and common legal accessibility requirements, balancing real accessibility against achievable MVP scope (over A: too weak a bar; under AAA: often impractical to fully meet).

## Colour contrast audit
Checked every [token](/design/foundations/tokens-and-theming.md) text/background pairing actually used in the app (both modes) against AA's 4.5:1 (normal text) / 3:1 (large text ≥18px, and non-text UI components) thresholds.

### Passing
| Pairing | Light | Dark |
| --- | --- | --- |
| `foreground` on `background` (body text) | 19.59:1 | 18.93:1 |
| `primary-foreground` on `primary` (primary buttons) | 16.38:1 | 13.81:1 |
| `secondary-foreground` on `secondary` (secondary buttons, badges) | 15.37:1 | 13.79:1 |
| `destructive-foreground` on `destructive` (danger buttons) | 4.60:1 | 5.86:1 |
| `foreground` on `muted` ([OMDb Search truncation hint](/design/screens/omdb-search/search-results.md#visual-treatment)) | 17.76:1 | 13.79:1 |

The `destructive-foreground` token didn't exist before this audit — see [Tokens & Theming — Destructive button text](/design/foundations/tokens-and-theming.md#destructive-button-text) for the fix and why it reuses `primary-foreground`'s per-mode values rather than a flat colour.

### Known gaps (flagged, required follow-up)
Two token pairings fail AA. Rather than patching the token values blindly, both are tracked as **required to resolve during the Figma design phase**, once we can see the actual visual impact of a fix — see [Open Questions](/open-questions.md).

- **`muted-foreground` fails normal-text AA in light mode** — 4.26:1 on `background`, 3.86:1 on `muted` (both need 4.5:1). Both clear the 3:1 large-text/non-text threshold. **Constraint until resolved:** don't use `muted-foreground` for normal-size body/label text in light mode — only for large text (≥18px) or non-text UI (icons, borders). Any future doc proposing small `muted-foreground` text should flag it as a violation, not use it as-is. **Related open item:** most subtitle/body text across the Empty-component screens (Pre-Search State, No Results & Errors, Search In Flight, First Launch & Empty States, Add API Key Dialog) doesn't yet specify its colour token at all — only [Search Results' truncation hint](/design/screens/omdb-search/search-results.md#visual-treatment) explicitly commits to `foreground` to sidestep this gap. Whether the rest should follow suit, or use a different resolution once this token issue is fixed, is grouped with this item for the Figma phase — see [Open Questions](/open-questions.md).
- **Light-mode focus ring is under the 3:1 UI-component minimum** — `--ring` on `background` measures 2.32:1 at full opacity, and the base stylesheet applies it at 50% (`outline-ring/50`), which weakens it further. Dark mode is fine (4.60:1).

## Touch target sizing
**44×44px minimum** for all tappable elements (movie card action button, nav buttons, search button, dialog buttons) — the Apple HIG / WCAG 2.1 AAA size. WCAG 2.1 AA itself has no minimum target size criterion (that's AAA in 2.1, or 24×24px at AA in the newer WCAG 2.2), but the app is explicitly [mobile/touch-first](/design/foundations/viewports-and-breakpoints.md) per [MVP Requirements](/product/mvp-requirements.md), so 44×44px is adopted as a practical baseline beyond the formal minimum.

## Focus states
Interactive components (button group, dialogs, inputs) get their focus indicator from the base stylesheet's global `outline-ring/50` (see [Tokens & Theming](/design/foundations/tokens-and-theming.md#base-behaviour)) plus whatever `radix`/shadcn build in by default (focus trap in dialogs, roving tabindex in the nav button group, etc.) — no custom focus treatment beyond that. The light-mode contrast shortfall on this ring is tracked above as a known gap, not fixed here.

## Accessible names for icon-only controls
Source of truth for this pattern app-wide — component docs below only note their specific label text or icon, not restate the rule.

- **Icon-only interactive controls** (no visible text label) need an explicit accessible name via `aria-label`.
- **Icons paired with visible text** (a label, adjacent heading text, a `<label>`-bearing input) are decorative — `aria-hidden="true"`. The adjacent text is what conveys meaning to assistive tech, not the icon.
- An input's `placeholder` is **not** a substitute for an accessible name — it isn't reliably announced by all screen readers and disappears once the user types. Every input needs a real (possibly visually-hidden) `<label>`.

| Control | Treatment |
| --- | --- |
| [Movie Card](/design/components/movie-card.md) `ellipsis-vertical` action button | Icon-only — `aria-label="More actions"`. |
| [Top Bar](/design/components/top-bar-and-navigation.md) `clapperboard` mark | Decorative, `aria-hidden="true"` — adjacent "Movie Tracker" text carries the name. |
| Library search inputs' leading `search` icon ([To Watch View](/design/screens/library-views/to-watch-view.md), [Watched View](/design/screens/library-views/watched-view.md)) | Decorative, `aria-hidden="true"` — the input itself needs a visually-hidden `<label>` (e.g. "Search your to-watch list"), not just its placeholder. |
| [OMDb Search Input](/design/screens/omdb-search/search-input.md)'s search button icon | Icon-only — `aria-label="Search"` in its idle state. (Its in-flight spinner state is already specified as `aria-hidden` in [Search In Flight](/design/screens/omdb-search/search-in-flight.md#accessibility) — the `aria-live` region text covers that state instead.) |
