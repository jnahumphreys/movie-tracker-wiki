---
type: User Journey
title: Searching the OMDb to Add a Movie
description: Expected behaviour for searching and adding movies via the OMDb API.
tags: [product, user-journey]
timestamp: 2026-07-02T00:00:00Z
status: confirmed
---

⬅ [User Journeys](/product/user-journeys/index.md)

# Searching the OMDb to Add a Movie

1. The user navigates to the OMDb view and sees the search input (prompting for a movie title) and an initial message inviting them to search for a movie. The user types a movie title into the search bar (for example, "Back to the Future") and hits search. The search button is disabled until there is valid input in the search field.

## Search in Flight

1. The search UI disables and indicates a search is in flight (reflected in both the input state and the message). If the user navigates away from this view, the search is cancelled and the search input value is reset to null.

## Return of Search Results

1. No more than 10 results are returned, displayed as a list of movie cards showing: the movie poster image (or a fallback placeholder), the movie title, the release year, and the runtime. Each card contains a button to invoke an action (see "Post-Search Actions" below). Once results return, the search button is re-enabled and the search input retains the original query.

## Post-Search Actions

### Add a Movie

1. The user clicks the action button on a movie card, which opens a UI of available actions:
   - If the movie is already in the user's library: indicate this, and present the actions **reassign status** (to the opposite of the current status), **remove from library**, or **cancel**.
   - If the movie is not in the user's library: present the actions **set as "to watch"**, **set as "watched"**, or **cancel**.
2. The user can continue actioning search results until they are done.

### User Invokes a Second Search with Previous Results Present

1. If the user runs a new search while results are already displayed, purge the existing results and re-run the search as per "Search in Flight."

### User Navigates Away from the Search View Without Performing an Action

1. If the user navigates away from the search view without performing an action, purge the search input and returned results. This also applies when closing the session/browser window.

## No Results Found

1. If no results are found, retain the search query and display this within the message UI.

## Error

1. If an error occurs, indicate this via the message UI. This could be a network error, an API error, or an authentication/API key error.

Design reference: [OMDb Search](/design/screens/omdb-search/index.md)
