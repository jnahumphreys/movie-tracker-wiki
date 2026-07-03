---
type: Testing Strategy
title: Testing Strategy
description: Planned testing approach across component, functional, and e2e layers.
tags: [qa, testing]
timestamp: 2026-07-02T00:00:00Z
status: reference only
---

⬅ [QA](/qa/index.md)

# Testing Strategy

> NOTE: these are not to be implemented now — reference only, to keep the MVP implementation testable and extensible.

- Component/integration testing, written for and run on the [Storybook](https://storybook.js.org/) test runner.
- Non-component (functional) testing using [Vitest](https://vitest.dev/).
- Mock the API (for testing and component-driven development) using [Mock Service Worker](https://mswjs.io/).
- End-to-end testing of core user flows.
- [Playwright MCP](https://playwright.dev/mcp) (official, Microsoft) — supports the planned end-to-end testing of core user flows.

See also: [Engineering — Future Considerations](/engineering/future-considerations.md) for the Component-Driven Development / Storybook adoption this depends on, and [Contribution & Testing Workflow](/engineering/onboarding/contribution-and-testing-workflow.md#verifying-your-work-before-opening-a-pr) for the interim manual verification checklist used until this strategy is implemented.
