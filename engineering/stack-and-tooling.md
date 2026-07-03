---
type: Engineering Architecture
title: Stack & Tooling
description: MVP technical stack, browser support target, and version-control conventions.
tags: [engineering, architecture, stack]
timestamp: 2026-07-03T00:00:00Z
status: confirmed
---

⬅ [Engineering](/engineering/index.md)

# Stack & Tooling

- Web-based single page application, running entirely on the client.
- Fetches data from the OMDb API and caches movie data to the client session — see [Data Caching & Persistence](/engineering/data-caching-and-persistence.md) for how.
- User library data and preferences persist across sessions, using the browser's local storage API.
- **Stack:** npm, Vite, TypeScript, React, [shadcn](/design/foundations/design-system.md)/Tailwind CSS, [Zustand](https://zustand.docs.pmnd.rs/) (slices pattern) with Suspense for client state management. Confirmed 2026-07-03, replacing an earlier plain Context+Reducer plan — see [State Management](/engineering/state-management.md) for why slices specifically make this workable.
- **Browser support:** last 2 versions of evergreen browsers (Chrome, Firefox, Safari, Edge) — no legacy-browser workarounds needed. Post-MVP, formalise this with a [browserslist](https://github.com/browserslist/browserslist) config rather than an implicit convention.
- Track changes in git, using per-feature branching and [Conventional Commit](https://www.conventionalcommits.org/en/v1.0.0/) messages — see [Contribution & Testing Workflow](/engineering/onboarding/contribution-and-testing-workflow.md) for the practical branch-naming/PR process.
- Align to [Semantic Versioning (SemVer)](https://semver.org/).
- Local library search is live and debounced at **250ms**. OMDb search returns the top 10 hits, and only runs on hitting the search button. The 10-result cap is a deliberate MVP scope limit, not an oversight — see [Product — Future Considerations](/product/future-considerations.md) for the planned lazy-loading/infinite-scroll follow-up.
- **OMDb search costs up to 11 API calls, not 1.** OMDb's `s=` search endpoint doesn't return Runtime, so each of the up to 10 results gets a follow-up `t=`/`i=` detail lookup before rendering — see [OMDb API Reference](/engineering/omdb-api-reference.md#two-different-endpoints-two-different-response-shapes) for why, and the daily-quota implications.

See also: [State Management](/engineering/state-management.md) for the Zustand slices this stack uses, [Data Caching & Persistence](/engineering/data-caching-and-persistence.md) for the persisted data shapes, and [Codebase Conventions](/engineering/onboarding/codebase-conventions.md) for how this stack maps to actual folders and files.
