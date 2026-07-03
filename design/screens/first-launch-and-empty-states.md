---
type: Design Screen
title: First Launch & Empty States
description: Empty-library and first-launch screen design.
tags: [design, screen]
timestamp: 2026-07-02T00:00:00Z
status: in-progress
---

# First Launch & Empty States

## Scope

Design at [360px](/design/foundations/viewports-and-breakpoints.md), in both light and dark mode:

1. Library view (`to watch`, scoped), empty state — copy below, with a CTA button that navigates to the [OMDb Search](/design/screens/omdb-search/index.md) view (not a direct API-key entry point — the OMDb view handles its own missing-key prompt if no key is set, see [Add API Key Dialog](/design/components/add-api-key-dialog.md)).
2. [Persistent top chrome](/design/components/top-bar-and-navigation.md) — icon + "Movie Tracker" text, no count badge (0 movies).
3. [Navigation button group](/design/components/top-bar-and-navigation.md) — `to watch` shown as active.
4. Light mode variant.
5. Dark mode variant.

## Copy

### First-launch empty state (`to watch`, 0 movies)
- **Title:** "Cue the lights... and action!"
- **Subtitle:** "Your cinematic journey starts here. What are you watching next?"

This is also the general "empty `to watch`" message — reused any time the `to watch` view has zero movies, not just on literal first launch (e.g. after moving every to-watch movie to watched). See [To Watch View](/design/screens/library-views/to-watch-view.md).

**CTA button copy:** "Search for a Movie" — navigates to the OMDb Search view/tab. Same button and destination regardless of whether an API key is already set; the OMDb view is what decides whether to prompt for one.

## Status
Ready to build in Figma — mockup not yet created.
