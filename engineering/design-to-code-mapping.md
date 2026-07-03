---
type: Engineering Architecture
title: Design-to-Code Mapping
description: How design docs and tokens become shadcn/Tailwind components in practice.
tags: [engineering, architecture, design-to-code]
timestamp: 2026-07-03T00:00:00Z
status: confirmed
---

⬅ [Engineering](/engineering/index.md)

# Design-to-Code Mapping

Design docs specify behaviour and shadcn component mappings; this doc covers how that becomes code.

- **shadcn primitives are the building blocks.** When a design doc names a shadcn component (e.g. [Movie Card](/design/components/movie-card.md) → Item + Badge), generate that primitive via the shadcn CLI into `components/ui/`, then compose it — don't hand-roll an equivalent.
- **Styling is Tailwind utility classes only**, using the token CSS variables from [Tokens & Theming](/design/foundations/tokens-and-theming.md) (`bg-background`, `text-muted-foreground`, etc.) — never hardcoded hex/oklch values in component code.
- **Spacing values in design docs are literal.** [Movie Card](/design/components/movie-card.md#spacing-at-360px)'s `p-3` / `gap-3` / `gap-1.5` etc. are the actual Tailwind classes to use, not just descriptive numbers.
- **Worked example — Movie Card:** shadcn `Item` (image variant) + two `Badge` + an outlined icon-only `Button` (Lucide `ellipsis-vertical`, `aria-label="More actions"`). One `MovieCard.tsx` in `components/`, taking a movie object and its context (library view vs. search result) as props — it doesn't reach into any store directly (see [State Management](/engineering/state-management.md#store-access-pattern)).

See also: [Codebase Conventions](/engineering/onboarding/codebase-conventions.md) for where new components physically live, and [State Management](/engineering/state-management.md) for how a component like this reads from the store.
