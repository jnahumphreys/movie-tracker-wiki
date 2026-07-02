---
type: Engineering Architecture
title: Architecture & Stack
description: MVP technical stack and architectural decisions.
tags: [engineering, architecture]
timestamp: 2026-07-02T00:00:00Z
status: confirmed
---

⬅ [Engineering](/engineering/index.md)

# Architecture & Stack

- Web-based single page application, running entirely on the client.
- Fetches data from the OMDb API and caches movie data to the client session.
- User library data and preferences persist across sessions, using the browser's local storage API.
- **Stack:** npm, Vite, TypeScript, React, shadcn/Tailwind CSS, and the React Context + Reducer API with Suspense for client state management. Note: this is to reduce initial dependency overhead — future iterations will migrate to more suitable libraries as needed (see [Future Considerations](/engineering/future-considerations.md)).
- Maintain a clear separation between the movie cache store, UI store, and user library store.
- Improve performance by using the OMDb ID as a keyed lookup source of truth.
- Prevent unnecessary re-renders through memoisation of store selectors.
- Track changes in git, using per-feature branching and [Conventional Commit](https://www.conventionalcommits.org/en/v1.0.0/) messages.
- Align to [Semantic Versioning (SemVer)](https://semver.org/).
- Local library search is live and debounced. OMDb search returns the top 10 hits, and only runs on hitting the search button.
