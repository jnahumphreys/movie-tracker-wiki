---
type: Tracking
title: Open Questions
description: Wiki-wide rolling tracker of unresolved contradictions, ambiguities, and audit findings that need human judgment.
tags: [tracking, audit]
timestamp: 2026-07-03T00:00:00Z
status: living document
---

⬅ [Wiki Home](/index.md)

# Open Questions

Single wiki-wide tracker — replaces the former per-folder `product/open-questions.md` and `design/open-questions.md`. Populated by the [Wiki Audit Process](/ai/audit-process.md) (anything it can't auto-fix) or added manually when a contradiction/ambiguity is spotted. Remove an item once it's resolved in its relevant doc(s) — see `AGENTS.md` §5.

## 1. `muted-foreground` fails normal-text AA in light mode

4.26:1 / 3.86:1, needs 4.5:1 — resolve during Figma design phase once visual impact of a fix can be assessed. See [Accessibility](/design/foundations/accessibility.md#known-gaps-flagged-required-follow-up).

## 2. Light-mode focus ring under 3:1 AA minimum

2.32:1, weakened further by 50% opacity — resolve during Figma design phase. See [Accessibility](/design/foundations/accessibility.md#known-gaps-flagged-required-follow-up).

## 3. Subtitle text colour is unspecified everywhere except the one place it was explicitly worked around

[Search Results' truncation hint](/design/screens/omdb-search/search-results.md#truncation-hint) explicitly uses `foreground` instead of `muted-foreground` for its text, specifically to avoid the light-mode AA failure tracked in item 1 above. No other subtitle/body text — [Pre-Search State](/design/screens/omdb-search/pre-search-state.md), [No Results & Errors](/design/screens/omdb-search/no-results-and-errors.md), [Search In Flight](/design/screens/omdb-search/search-in-flight.md), [First Launch & Empty States](/design/screens/first-launch-and-empty-states.md), [Add API Key Dialog](/design/components/add-api-key-dialog.md) — specifies its text colour token. If any default to `muted-foreground` (a common shadcn Empty-component convention), they'd reintroduce the same violation the truncation hint was deliberately written to avoid. **Deliberately deferred, not resolved** — grouped with items 1 and 2 as a Figma-design-phase decision, once the actual visual weight of an all-`foreground` treatment vs. a fixed `muted-foreground` can be judged side by side, rather than locked in abstractly here.

## 4. `status` values that don't match `AGENTS.md` §2's three-value enum

`AGENTS.md` §2 defines `status` as `not started | in progress | confirmed` — nothing else. Nine docs currently deviate, in two distinct ways, and audit mode's auto-fix rules don't cover changing a status value itself (only the parent-index emoji), so this needs a human call:

- **Malformed/typo'd values** (clearly meant to be a canonical value): [Branding](/design/foundations/branding.md) has `confirmed (MVP scope)`; [First Launch & Empty States](/design/screens/first-launch-and-empty-states.md) has `in-progress` (hyphen, not the enum's `in progress`).
- **A consistent but undocumented extension** used across 7 reference/tracking-style docs, never added to the §2 enum: `reference only` on [DevOps — Future Considerations](/devops/future-considerations.md), [Engineering — Future Considerations](/engineering/future-considerations.md), [Product — Future Considerations](/product/future-considerations.md), and [Testing Strategy](/qa/testing-strategy.md); `living document` on [References](/references.md), [Open Questions](/open-questions.md) (this doc), and [MVP Scope Gaps](/product/mvp-scope-gaps.md).

Decision needed: normalize all nine to the three canonical values (parent `index.md` emoji would then need assigning too, e.g. for the `reference only`/`living document` docs which currently show ✅ or nothing), or formalize `reference only` and `living document` as additional enum values in `AGENTS.md` §2 with their own emoji convention. Left unfixed either way — not guessing.

## 5. `log.md` contains 11 broken links to files consolidated/relocated in later entries

Mechanical link-check found these targets don't exist, and exactly one same-named file exists elsewhere in each case — which would normally qualify for silent auto-fix per the audit process. Holding off because these are historical log entries describing the wiki's state *at the time they were written*, and rewriting them changes what actually happened rather than fixing a stale reference:

- 9× `/design/open-questions.md` (2026-07-02 and 2026-07-03 entries) — consolidated into `/open-questions.md` per the "Consolidated `product/open-questions.md` and `design/open-questions.md`..." entry.
- `/product/agent-skills.md` (2026-07-02 entry) — relocated to `/ai/agent-skills.md` per the "Established a new AI area..." entry.
- `/engineering/ai-and-mcp-tooling.md` (2026-07-02 entry) — consolidated into `/ai/mcp-tooling.md`, same entry.
- 15× `/engineering/architecture-and-stack.md`, 6× `/engineering/onboarding/component-and-state-architecture.md`, 4× `/engineering/onboarding/codebase-structure-and-conventions.md` (various 2026-07-03 entries) — per the "Split /engineering into concept docs" entry, these three files were retired and their content redistributed across `/engineering/stack-and-tooling.md`, `/engineering/state-management.md`, `/engineering/data-caching-and-persistence.md`, `/engineering/design-to-code-mapping.md`, and `/engineering/onboarding/codebase-conventions.md` — no single new file is a 1:1 replacement, so even a "rewrite to current path" resolution would need per-link judgment on which new doc a given historical mention now maps to.

Decision needed: leave `log.md`'s historical links as-written (accurate to their moment, but dead today), or rewrite them to current paths for reader convenience. Not treating this as a plain dead-link fix without a call on which `log.md`'s links are "for."

## 6. Nine `/references.md` entries have no hyperlink citation elsewhere in the wiki

New [ledger-sync check](/ai/audit-process.md#audit-mode-mechanical-cron-safe) added 2026-07-03: it added 3 missing entries automatically (safe — nothing lost), but flags removals for human review rather than auto-deleting, since this wiki often names a tool/library in prose without hyperlinking it every time. Checked each of the 9 against prose (not just links):

- **Genuinely no trace anywhere** — safe-looking candidate for removal: [shadcn preset](https://ui.shadcn.com/create?preset=b6sByO7BQ&pointer=true).
- **Named in prose, just not hyperlinked** — likely still relevant, missing an inline link rather than stale: [Button Group Input](https://ui.shadcn.com/docs/components/radix/button-group#input) (named in [Search Input](/design/screens/omdb-search/search-input.md)), [Input](https://ui.shadcn.com/docs/components/radix/input) (named in [Watched View](/design/screens/library-views/watched-view.md), [To Watch View](/design/screens/library-views/to-watch-view.md)), [Empty](https://ui.shadcn.com/docs/components/radix/empty) (named throughout `/design/screens`), [Fontsource — Noto Serif Variable](https://fontsource.org/fonts/noto-serif) (typeface named in [Tokens & Theming](/design/foundations/tokens-and-theming.md)), [Anthropic Product Management plugin](https://github.com/anthropics/knowledge-work-plugins/tree/main/product-management), [agile-product-owner skill](https://github.com/alirezarezvani/claude-skills/blob/main/product-team/agile-product-owner/SKILL.md), and [pm-skills marketplace](https://github.com/phuryn/pm-skills) (all three named by repo path in [AI Agent Skills — Product Owner](/ai/agent-skills.md)'s Candidates section).
- **Weak/generic match only** — mentions "Button" but generically, not clearly citing this specific component page: [Button](https://ui.shadcn.com/docs/components/radix/button).

Decision needed: for the "named but not hyperlinked" group, add the inline link where each is named (turning the citation real) or confirm it's fine for `/references.md` to hold links for prose-only mentions; for the preset and the generic-Button case, confirm they're actually stale before removing.
