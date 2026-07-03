---
type: Tracking
title: MVP Scope Gaps — Core Loop Focus
description: Reference doc for iterative Q&A sessions to rebalance product depth toward the core tracking loop.
tags: [product, tracking, mvp]
timestamp: 2026-07-03T00:00:00Z
status: living document
---

⬅ [Product](/product/index.md)

# MVP Scope Gaps — Core Loop Focus

## The finding

The API key flow has absorbed a disproportionate share of product/design depth relative to its role, while the app's actual core loop — browsing a library, marking movies watched/to-watch, removing them — is comparatively unexamined.

Evidence: [Managing the Local Library](/product/user-journeys/managing-local-library.md) — the journey covering the app's actual point — is 2 substantive lines. The API key flow, by contrast, spans roughly half of [App First Launch](/product/user-journeys/app-first-launch.md), a third of [Searching the OMDb](/product/user-journeys/searching-omdb.md)'s error handling, three dedicated bullets in [Future Considerations](/product/future-considerations.md), and motivated an entire new cross-cutting journey ([Data Persistence Errors](/product/user-journeys/data-persistence-errors.md)). In `/design`, the [Add API Key Dialog](/design/components/add-api-key-dialog.md) is now the single most heavily specified component in the wiki — more detailed than the Movie Card, the library views, or search results.

A user opens this app to browse their to-watch list and mark things watched — that's the overwhelming majority of real usage. Entering an API key is a thirty-second, once-ever setup step. It got enterprise-grade rigor; the daily-use loop didn't.

## Purpose of this doc

A working backlog for iterative Q&A sessions to give the core loop the same level of scrutiny the API key flow already received. Not urgent fixes, not contradictions — just areas that haven't been interviewed yet and should be, to flesh out MVP scope properly. Check items off (or expand them into their own journey updates) as they get worked through; add more as they surface.

## Candidate areas to interview on

### Managing the Local Library
- [x] **Visual feedback on reassignment.** Resolved: no transition — card is simply absent on next render once the dialog closes; the dialog closing is the confirmation. See [Managing the Local Library](/product/user-journeys/managing-local-library.md).
- [x] **Visual feedback on deletion.** Resolved: same as reassignment, no transition, consistent treatment. See [Managing the Local Library](/product/user-journeys/managing-local-library.md).
- [x] **Undo.** Resolved: no undo at MVP — deletion stays immediate and final, consistent with the existing no-confirmation stance. See [Managing the Local Library](/product/user-journeys/managing-local-library.md).
- [x] **Resulting empty state.** Resolved: standard empty-state message is sufficient, no special-cased transition for this path. See [Managing the Local Library](/product/user-journeys/managing-local-library.md).
- [x] **Library scale.** Resolved: no cap at MVP, plain full-height scroll, no pagination/virtualization — explicit stated assumption, not a gap. See [Managing the Local Library](/product/user-journeys/managing-local-library.md).

### Searching the User's Library
- [x] **Match semantics.** Resolved: case-insensitive substring match against the title. See [Searching the User's Library](/product/user-journeys/searching-library.md).
- [x] **Live-filter behavior.** Resolved: 250ms debounce after the user stops typing, rather than instant per-keystroke filtering — matches [Stack & Tooling](/engineering/stack-and-tooling.md)'s existing engineering-side commitment. See [Searching the User's Library](/product/user-journeys/searching-library.md).

### Cross-cutting
- [x] **Write interaction model (found during a `/product` readiness audit, not the original backlog).** Resolved: MVP uses a blocking model — the triggering dialog shows a loading state and waits for the write to succeed before closing, matching the Add API Key Dialog's precedent. Optimistic-with-toast is deferred post-MVP (needs a toast system + revert logic MVP doesn't have time for). See [Data Persistence Errors](/product/user-journeys/data-persistence-errors.md#write-failure-saving-a-change).

## How to use this

Pull one item at a time into a normal interview round (same pattern as the `/product` and `/design` "least confident" sessions) — resolve it, update the relevant journey doc, then check it off here. This doc stays open until the core loop has had a comparable amount of scrutiny to the API key flow, not until every box is checked — some of these may reasonably resolve as "not needed for MVP," which is a fine outcome as long as it's a deliberate answer rather than silence.

**Status:** the original seven candidate items are now all resolved (2026-07-03). This doc stays open as a living backlog — add new items here as they surface, rather than closing it out entirely, since the underlying goal (core loop gets comparable scrutiny to auth/error-handling going forward) is ongoing, not a one-time pass.
