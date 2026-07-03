---
type: Future Consideration
title: Engineering — Future Considerations
description: Engineering scope explicitly deferred beyond MVP.
tags: [engineering, future]
timestamp: 2026-07-03T00:00:00Z
status: reference only
---

⬅ [Engineering](/engineering/index.md)

# Future Considerations (Engineering)

> NOTE: these are not to be implemented now.

- Agentic / AI wiki (as part of the repo), including agents, tooling, etc. — ✅ this wiki is an implementation of that.
- [Component-driven development](https://www.componentdriven.org/), using [Storybook](https://storybook.js.org/).
- Introduce a routing library.
- Introduce a server-side fetch/cache library (e.g. TanStack Query) to replace the current manual OMDb-fetch-and-cache logic — see [Data Caching & Persistence](/engineering/data-caching-and-persistence.md) for the MVP approach (Zustand slice + manual fetch-and-cache).
- Lazy loading of movie thumbnail images.
- Virtualisation of scrolling lists, to prevent performance bottlenecks.
- [Chrome DevTools MCP](https://developer.chrome.com/blog/chrome-devtools-mcp) (official, Google) — live performance tracing, console/network inspection, Core Web Vitals; most useful once there's a real UI to debug and tune.
- Multi-tab localStorage sync (e.g. a `storage` event listener to refresh in-memory state when another tab writes) — not built at MVP; low-impact for a single-user local app where multi-tab use is uncommon. Known limitation until then: two tabs open at once can silently overwrite each other's saved changes.
- Formalise browser support with a [browserslist](https://github.com/browserslist/browserslist) config, rather than the implicit "last 2 versions of evergreen browsers" convention stated in [Stack & Tooling](/engineering/stack-and-tooling.md).
- Lint rules enforcing this doc's architectural conventions (e.g. banning a slice from reassigning another slice's state directly, requiring cross-slice changes to go through the owning slice's own action) — Vite's default tooling already includes Prettier, basic linting, and TypeScript, and adding custom rules on top is straightforward; deferred until post-MVP since it's currently a solo project relying on manual discipline.

See also: [DevOps — Future Considerations](/devops/future-considerations.md) (tooling/CI) and [QA — Testing Strategy](/qa/testing-strategy.md) (testing).
