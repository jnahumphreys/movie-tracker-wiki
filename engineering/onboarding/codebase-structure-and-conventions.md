---
type: Onboarding Guide
title: Codebase Structure & Conventions
description: Folder/file layout, naming conventions, and where new code lives.
tags: [engineering, onboarding, conventions]
timestamp: 2026-07-03T00:00:00Z
status: confirmed
---

⬅ [Onboarding](/engineering/onboarding/index.md)

# Codebase Structure & Conventions

> The codebase hasn't been scaffolded yet — this doc sets the convention new code should follow from the first commit, per [Architecture & Stack](/engineering/architecture-and-stack.md).

## Top-level structure

Organized **by type**, not by feature — the app's current scope (search + two library statuses) doesn't yet justify feature folders.

```
src/
  components/
    ui/          # shadcn-generated primitives (button.tsx, dialog.tsx, ...) — regenerate via shadcn CLI, never hand-edit
    MovieCard.tsx
    AddApiKeyDialog.tsx
    ...          # custom, composed app components
  stores/
    movie-cache/
      context.ts
      reducer.ts
      types.ts
    ui/
      context.ts
      reducer.ts
      types.ts
    user-library/
      context.ts
      reducer.ts
      types.ts
  hooks/
    useMovieCache.ts
    ...
  lib/
    omdb.ts        # OMDb API client
    ...
  types/
    movie.ts        # shared domain types (e.g. the OMDb response shape)
```

## Naming conventions

| What | Convention | Example |
|---|---|---|
| Components | PascalCase.tsx, matches the exported name 1:1 | `MovieCard.tsx` |
| shadcn primitives | kebab-case.tsx (shadcn CLI's own default — don't rename) | `button.tsx` |
| Hooks | camelCase.ts, `use` prefix | `useMovieCache.ts` |
| Stores | one sub-folder per store under `stores/` | `stores/movie-cache/` |
| Everything else (lib, types, config) | kebab-case.ts | `omdb.ts` |

## Where new code goes

- **New shadcn primitive** → generate via the shadcn CLI into `components/ui/` — never hand-write one.
- **New custom component** → `components/`, flat — no per-component sub-folders at this scope.
- **New store** → a new sub-folder under `stores/`, following the movie-cache / UI / user-library split from [Architecture & Stack](/engineering/architecture-and-stack.md). Don't add a fourth store without cause, and don't reach into another store's reducer directly — go through its own actions.
- **New hook** → `hooks/`, unless it's only ever used by one store's own internals, in which case it lives alongside that store instead.
- **Shared domain types** (e.g. the OMDb response shape — see [OMDb API Reference](/engineering/omdb-api-reference.md); the persisted store shapes — see [Architecture & Stack](/engineering/architecture-and-stack.md#persisted-data-shape)) → `types/`.

## Import paths

Uses the `@/` path alias (shadcn/Vite default) for everything under `src/` — `@/components/MovieCard`, `@/stores/movie-cache/context` — never deep relative `../../../` imports.
