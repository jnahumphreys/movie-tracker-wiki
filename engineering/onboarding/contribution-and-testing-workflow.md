---
type: Onboarding Guide
title: Contribution & Testing Workflow
description: Branching, commits, PR/review process, and FE-specific testing notes.
tags: [engineering, onboarding, workflow]
timestamp: 2026-07-03T00:00:00Z
status: confirmed
---

⬅ [Onboarding](/engineering/onboarding/index.md)

# Contribution & Testing Workflow

## Branching & commits

- Per-feature branching (see [Stack & Tooling](/engineering/stack-and-tooling.md)), named `<type>/<issue-number>-<kebab-description>` — the type prefix matches the Conventional Commit type used in that branch's commits. E.g. `feat/42-omdb-search`, `fix/57-library-empty-state`.
- Commit messages follow [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/).
- Releases align to [Semantic Versioning (SemVer)](https://semver.org/).

## PR review

- Currently a solo project — open a PR for the record and CI, but no blocking reviewer is required at this stage.
- As contributors join, the plan is to add a `CODEOWNERS` file and require peer review before merge — not yet in place, tracked in [DevOps — Future Considerations](/devops/future-considerations.md).

## Verifying your work before opening a PR

QA's [Testing Strategy](/qa/testing-strategy.md) — Vitest, Storybook, Playwright — is a planned direction, not implemented yet. Until it exists, this is the interim standard:

1. Find the relevant [Product](/product/index.md) user journey and [Design](/design/index.md) screen/component doc for what you built.
2. Manually walk through the behaviour described, confirming it matches — states, copy, and interactions as specified, not just "looks right".
3. Check both light and dark mode (see [Tokens & Theming](/design/foundations/tokens-and-theming.md)).
4. Check at least one mobile viewport — [MVP Requirements](/product/mvp-requirements.md) prioritises touch-first/mobile interaction.
5. Tab through what you built. Confirm focus lands somewhere sensible after a dialog closes or a Suspense boundary resolves/errors (see [State Management — Suspense boundaries](/engineering/state-management.md#suspense-boundaries)), and that composed shadcn primitives still behave as expected on keyboard (e.g. a dialog is dismissible and trapping focus, a disabled button isn't tab-reachable). This covers engineering-side accessibility (focus/keyboard); colour contrast and labelling are Design's [Accessibility](/design/foundations/accessibility.md) doc's concern, not re-checked here.
6. If your change resolves or touches an [Open Question](/open-questions.md), update or remove the relevant tracker item as part of the same PR.

This isn't a substitute for real automated tests — it's the floor until [Testing Strategy](/qa/testing-strategy.md) is implemented, at which point this section should be replaced with concrete commands (e.g. `npm test`).

## PR descriptions

Link back to the specific Product/Design doc(s) your change implements, so reviewers — present or future — can check behaviour against the same source of truth you used.
