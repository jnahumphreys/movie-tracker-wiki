---
type: Design Component
title: Add API Key Dialog
description: Dialog for entering the OMDb API key.
resource: https://ui.shadcn.com/docs/components/radix/dialog
tags: [design, component]
timestamp: 2026-07-02T00:00:00Z
status: confirmed
---

# Add API Key Dialog

Maps to the shadcn **Dialog** component. Triggered by the "add API key" CTA button described in [App First Launch](/product/user-journeys/app-first-launch.md) — shown wherever a key is currently missing (first launch, or the OMDb view if a previously-saved key is gone).

## Copy

- **Title:** "Lights, camera, API key!"
- **Subtitle:** "Pop in your free OMDb API key to start searching for movies."
- **Input label:** "OMDb API key"
- **Help link:** "Don't have one? Get a free key from OMDb" → [omdbapi.com/apikey.aspx](https://www.omdbapi.com/apikey.aspx)
- **Buttons:** "Cancel" / "Save key"

## Layout

- shadcn Dialog containing, top to bottom: title, subtitle, text input (with label), help link, then Cancel/Save key button pair.
- Input is plain text (not masked) — the key is a local-only value tied to a free API, not treated as a high-sensitivity secret.

## Behaviour

- **Save without validation:** clicking "Save key" saves whatever is typed and closes the dialog immediately — no live check against OMDb. If the key turns out to be invalid, that surfaces later as a search error on the OMDb view, not in this dialog. (Live validation-on-submit is a possible post-MVP enhancement — see [Future Considerations](/product/future-considerations.md).)
- **Empty input:** "Save key" is disabled until the input has text — no separate error message needed.
- **Dismissible anytime:** X button, backdrop click, and Escape all close the dialog without requiring a key — the user can reopen it later via the same CTA.

## Not yet covered
- None — ready for visual mockup.
