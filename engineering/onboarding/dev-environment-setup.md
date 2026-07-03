---
type: Onboarding Guide
title: Dev Environment Setup
description: Getting a local frontend dev environment running end to end.
tags: [engineering, onboarding, setup]
timestamp: 2026-07-03T00:00:00Z
status: confirmed
---

⬅ [Onboarding](/engineering/onboarding/index.md)

# Dev Environment Setup

## Prerequisites

- **Node.js 22 (LTS)**, managed via [nvm](https://github.com/nvm-sh/nvm). The repo commits an `.nvmrc` pinning the exact version — run `nvm use` rather than relying on whatever Node happens to be installed.
- **npm** (ships with Node — this project doesn't use yarn/pnpm).

## Getting started

1. Clone the repo.
2. `nvm use` — picks up `.nvmrc`, switches to Node 22.
3. `npm install`.
4. `npm run dev` — starts the Vite dev server.

## OMDb API key in development

There's no server-side or build-time API key baked into the app — see [Architecture & Stack](/engineering/architecture-and-stack.md). At runtime, the real flow is: a user enters their key through the app's own UI (OMDb search view), it's validated live against OMDb, then stored in local storage. See [MVP Requirements](/product/mvp-requirements.md) and [Add API Key Dialog](/design/components/add-api-key-dialog.md).

For local dev convenience only, an optional `.env.local` shortcut pre-seeds local storage with a key on boot, so you're not re-entering it after every storage reset:

```env
VITE_OMDB_API_KEY=your_key_here
```

- Get a free key: [OMDb API — Get a free API key](https://www.omdbapi.com/apikey.aspx).
- `.env.local` is gitignored — never commit a real key.
- This is a dev-only convenience. The UI entry/validation flow is the real, tested path and the only one that ships to users — don't build features that assume the env var exists.

## Available scripts

- `npm run dev` — local dev server with hot reload.
- `npm run build` — production build.
- `npm run preview` — serve the production build locally.
- `npm run lint` — run the linter.

No `test` script yet — see [Contribution & Testing Workflow](/engineering/onboarding/contribution-and-testing-workflow.md). QA's [Testing Strategy](/qa/testing-strategy.md) (Vitest, Storybook, Playwright) is a planned direction, not implemented.

## Deployment

There's no hosted environment at MVP — the app is run locally only. See [Deployment](/devops/deployment.md).
