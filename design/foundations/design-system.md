---
type: Design Foundation
title: Design System
description: shadcn is this app's design system — the source-of-truth statement other docs point to instead of restating.
resource: https://ui.shadcn.com/
tags: [design, foundations, design-system, shadcn]
timestamp: 2026-07-03T00:00:00Z
status: confirmed
---

⬅ [Foundations](/design/foundations/index.md)

# Design System

**This app's design system is [shadcn](https://ui.shadcn.com/).** Every other doc that names a shadcn primitive, token, or convention is applying this decision, not making its own — treat this as the source of truth for "why does this look like shadcn," and point back here rather than re-justifying it per doc.

In practice, that means:

- **Primitives** (Button, Dialog, Item, Badge, Empty, etc.) are Radix-based components generated via the shadcn CLI into `components/ui/` — never hand-rolled. See [Design-to-Code Mapping](/engineering/design-to-code-mapping.md) for the generation workflow and [Codebase Conventions](/engineering/onboarding/codebase-conventions.md) for where they live.
- **Tokens** (colour, radius, typography) are the shadcn CSS variable set, themed for this app — see [Tokens & Theming](/design/foundations/tokens-and-theming.md) for the actual stylesheet.
- **Component-level decisions** (which primitive maps to which design doc, e.g. Movie Card → Item + Badge) live in each component's own doc under [Design — Components](/design/components/index.md), not here.
- Deviations from shadcn's defaults (e.g. no `X` close button on dialogs, see [Dialog Conventions](/design/components/dialog-conventions.md)) are called out explicitly in the doc that deviates — this doc only establishes the baseline they're deviating from.
