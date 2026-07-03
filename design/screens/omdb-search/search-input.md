---
type: Design Screen
title: OMDb Search — Search Input
description: Search input design for the OMDb search screen.
tags: [design, screen, omdb-search]
timestamp: 2026-07-03T00:00:00Z
status: confirmed
---

# Search Input

Behaviour spec: [Searching the OMDb to Add a Movie](/product/user-journeys/searching-omdb.md). Uses the shadcn **Button Group Input** component.

## Layout
Single-row button group input: text field with an adjoined search button, per the shadcn Button Group Input component defaults.

## Copy
- **Placeholder:** "What are we watching tonight?" — playful tone, consistent with the [No Results & Errors](/design/screens/omdb-search/no-results-and-errors.md) copy voice.

## Disabled state
The entire input (text field + search button) is disabled while no API key is stored — see [Pre-Search State — no key set](/design/screens/omdb-search/pre-search-state.md#variant-no-key-set). Once a key is set, the search button specifically is disabled when the input is empty, and again once a search is submitted (see [Search In Flight](/design/screens/omdb-search/search-in-flight.md)). Standard shadcn disabled treatment — reduced opacity and muted color, no icon change.

## Clearing a query
No dedicated clear button at MVP — resetting the input means backspacing manually or navigating away (which purges it, per [Searching the OMDb](/product/user-journeys/searching-omdb.md)). A clear (×) button is a considered, deferred addition — see [Future Considerations](/product/future-considerations.md).
