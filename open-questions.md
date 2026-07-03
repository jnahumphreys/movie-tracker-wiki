---
type: Tracking
title: Open Questions
description: Wiki-wide rolling tracker of unresolved contradictions, ambiguities, and audit findings that need human judgment.
tags: [tracking, audit]
timestamp: 2026-07-03T00:00:00Z
status: living document
---

⬅ [Wiki Home](/index.md)

# Open Questions

Single wiki-wide tracker — replaces the former per-folder `product/open-questions.md` and `design/open-questions.md`. Populated by the [Wiki Audit Process](/ai/audit-process.md) (anything it can't auto-fix) or added manually when a contradiction/ambiguity is spotted. Remove an item once it's resolved in its relevant doc(s) — see `AGENTS.md` §5.

## 1. `muted-foreground` fails normal-text AA in light mode

4.26:1 / 3.86:1, needs 4.5:1 — resolve during Figma design phase once visual impact of a fix can be assessed. See [Accessibility](/design/foundations/accessibility.md#known-gaps-flagged-required-follow-up).

## 2. Light-mode focus ring under 3:1 AA minimum

2.32:1, weakened further by 50% opacity — resolve during Figma design phase. See [Accessibility](/design/foundations/accessibility.md#known-gaps-flagged-required-follow-up).

## 3. Subtitle text colour is unspecified everywhere except the one place it was explicitly worked around

[Search Results' truncation hint](/design/screens/omdb-search/search-results.md#truncation-hint) explicitly uses `foreground` instead of `muted-foreground` for its text, specifically to avoid the light-mode AA failure tracked in item 1 above. No other subtitle/body text — [Pre-Search State](/design/screens/omdb-search/pre-search-state.md), [No Results & Errors](/design/screens/omdb-search/no-results-and-errors.md), [Search In Flight](/design/screens/omdb-search/search-in-flight.md), [First Launch & Empty States](/design/screens/first-launch-and-empty-states.md), [Add API Key Dialog](/design/components/add-api-key-dialog.md) — specifies its text colour token. If any default to `muted-foreground` (a common shadcn Empty-component convention), they'd reintroduce the same violation the truncation hint was deliberately written to avoid. **Deliberately deferred, not resolved** — grouped with items 1 and 2 as a Figma-design-phase decision, once the actual visual weight of an all-`foreground` treatment vs. a fixed `muted-foreground` can be judged side by side, rather than locked in abstractly here.
