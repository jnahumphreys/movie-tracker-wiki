---
type: API Reference
title: OMDb API Reference
description: Sample OMDb API response payload.
resource: https://www.omdbapi.com/
tags: [engineering, api, omdb]
timestamp: 2026-07-02T00:00:00Z
status: confirmed
---

⬅ [Engineering](/engineering/index.md)

# OMDb API Reference

## Two different endpoints, two different response shapes

OMDb has separate endpoints for a title/ID lookup vs. a search:

- **`t=<title>` or `i=<imdbID>`** — single best-match lookup. Returns the full object below (Runtime, Genre, Plot, Ratings, etc.).
- **`s=<title>`** — search, used for [Searching the OMDb](/product/user-journeys/searching-omdb.md)'s up-to-10-results list. Returns a much thinner `Search: [...]` array — each entry is only `Title`, `Year`, `imdbID`, `Type`, `Poster`, no Runtime or anything else.

Since [MVP Requirements](/product/mvp-requirements.md) needs Runtime shown on search-result cards too (not just library cards), **the app follows `s=` with a `t=`/`i=` lookup for each of the up to 10 results returned**, so every card has full data before rendering. This means one search costs up to 11 API calls (1 search + up to 10 detail lookups), not 1 — a deliberate accuracy-over-quota tradeoff, confirmed 2026-07-03. This compounds the daily-quota risk covered in [Searching the OMDb — Error](/product/user-journeys/searching-omdb.md#error) (the "daily request limit reached" state is reached roughly 10x faster than a naive 1-call-per-search estimate would suggest).

Sample response from the OMDb API (`t=`/`i=`, or a single enriched search result) when looking up "Back to the Future":

```json
{
  "Title": "Back to the Future",
  "Year": "1985",
  "Rated": "PG",
  "Released": "03 Jul 1985",
  "Runtime": "116 min",
  "Genre": "Adventure, Comedy, Sci-Fi",
  "Director": "Robert Zemeckis",
  "Writer": "Robert Zemeckis, Bob Gale",
  "Actors": "Michael J. Fox, Christopher Lloyd, Lea Thompson",
  "Plot": "Marty McFly, a 17-year-old high school student, is accidentally sent 30 years into the past in a time-traveling DeLorean invented by his close friend, the maverick scientist Doc Brown.",
  "Language": "English",
  "Country": "United States",
  "Awards": "Won 1 Oscar. 27 wins & 25 nominations total",
  "Poster": "https://m.media-amazon.com/images/M/MV5BZmM3ZjE0NzctNjBiOC00MDZmLTgzMTUtNGVlOWFlOTNiZDJiXkEyXkFqcGc@._V1_QL75_UX380_CR0,14,380,562_.jpg",
  "Ratings": [
    { "Source": "Internet Movie Database", "Value": "8.5/10" },
    { "Source": "Rotten Tomatoes", "Value": "93%" },
    { "Source": "Metacritic", "Value": "88/100" }
  ],
  "Metascore": "88",
  "imdbRating": "8.5",
  "imdbVotes": "1,450,794",
  "imdbID": "tt0088763",
  "Type": "movie",
  "DVD": "N/A",
  "BoxOffice": "$222,476,852",
  "Production": "N/A",
  "Website": "N/A",
  "Response": "True"
}
```
