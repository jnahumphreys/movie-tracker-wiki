---
type: Design Component
title: Add API Key Dialog
description: Dialog for entering and validating the OMDb API key.
resource: https://ui.shadcn.com/docs/components/radix/dialog
tags: [design, component]
timestamp: 2026-07-03T00:00:00Z
status: confirmed
---

# Add API Key Dialog

Maps to the shadcn **Dialog** component. Lives entirely within the [OMDb Search](/design/screens/omdb-search/index.md) view — triggered by its own missing-key prompt when no key is stored, or by the "Add API key" action on the [missing/invalid-key error state](/design/screens/omdb-search/no-results-and-errors.md) when a search fails due to the key. Not shown from the library views — see [First Launch & Empty States](/design/screens/first-launch-and-empty-states.md) for how those views instead redirect to OMDb search.

## Entry state

### Copy
- **Title:** "Lights, camera, API key!"
- **Subtitle:** "Pop in your free OMDb API key to start searching for movies."
- **Input label:** "OMDb API key"
- **Help link:** "Don't have one? Get a free key from OMDb" → [omdbapi.com/apikey.aspx](https://www.omdbapi.com/apikey.aspx)
- **Buttons:** "Cancel" / "Save key"

### Layout
- shadcn Dialog containing, top to bottom: title, subtitle, text input (with label), help link, then the button pair.
- Buttons follow [Dialog Conventions](/design/components/dialog-conventions.md#button-layout): "Save key" — primary (this dialog's sole contextual action), divider, "Cancel" — secondary. No destructive action in this dialog.
- Input is plain text (not masked) — the key is a local-only value tied to a free API, not treated as a high-sensitivity secret.
- **Empty input:** "Save key" is disabled until the input has text — no separate error message needed.

## Submit behaviour — live validation
Clicking "Save key" runs a live check against OMDb before saving. The dialog stays open and swaps its content to reflect the result — this is not a silent background check.

### While checking
- "Save key" shows a loading state (matches the spinning `loader-circle` treatment established in [Search In Flight](/design/screens/omdb-search/search-in-flight.md)); input and buttons disabled for the duration.

### Success
- **Title:** "You're all set!"
- **Subtitle:** "Your OMDb API key is ready to roll."
- **Button:** single "Close" button (primary) replaces the Cancel/Save key pair — no divider needed with only one action.
- Closing saves the key and returns to the OMDb search view.

### Failure — invalid key
- **Title:** "Cut! That's a blooper."
- **Subtitle:** "That key doesn't look right — double check and try again."
- Input remains editable, pre-filled with what the user typed (not cleared). "Save key" is re-enabled and doubles as the retry — there's no separate "try again" action at MVP. Re-validating the same unchanged value on tap is a post-MVP enhancement — see [Future Considerations](/product/future-considerations.md).
- "Cancel" remains available to back out without saving.

### Failure — network/connection issue
- **Title:** "Who turned out the lights?"
- **Subtitle:** "Couldn't reach OMDb to check your key. Check your connection and try again."
- Same recovery pattern as the invalid-key case: input stays editable and pre-filled, "Save key" re-enabled as the retry, "Cancel" available.

## Dismiss behaviour
Follows [Dialog Conventions](/design/components/dialog-conventions.md#dismiss-behaviour) — Escape, backdrop click/touch, or "Cancel" all close without saving a key, from any state (entry, checking excluded — see below, success, or failure). The user can reopen it later via the same entry points.
- **While checking:** the dialog does not accept dismissal until the check resolves (success or failure) — prevents an ambiguous in-flight state on close.

## Relationship to search recovery
When this dialog is opened from a search error (as opposed to the initial missing-key prompt), the OMDb search view's retained query is **not** automatically re-run after a successful save at MVP — the user returns to the search view and taps search again. Auto-retrying the query on success is a post-MVP enhancement — see [Future Considerations](/product/future-considerations.md).
