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
Search button is disabled when the input is empty, and again once a search is submitted (see [Search In Flight](/design/screens/omdb-search/search-in-flight.md)). Standard shadcn disabled treatment — reduced opacity and muted color, no icon change.
