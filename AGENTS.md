---
type: Agent Policy
title: AGENTS.md
description: Wiki-specific policy for any agent working in this bundle — structure, frontmatter, and working process. OKF-SKILL.md remains the source of truth for OKF mechanics and authoring judgment calls.
tags: [meta, agent-policy, governance]
timestamp: 2026-07-03T00:00:00Z
status: living document
---

⬅ [Wiki Home](/index.md)

# AGENTS.md

Policy for any agent (or human) working in this wiki. This file is a non-reserved OKF concept like any other — it carries its own frontmatter (`type: Agent Policy`, new to the taxonomy in §2) rather than being exempt from the rules it defines.

Read [OKF-SKILL.md](/ai/OKF-SKILL.md) first for OKF mechanics (validation, bundle creation, enrichment) — this file doesn't restate those. It covers what's specific to *this* bundle: how it's laid out, what its frontmatter actually looks like in practice, and how an agent should work with James inside it.

## 1. Structure

**Reserved files** — only `index.md` and `log.md` are exempt from frontmatter, per OKF-SKILL.md. Every other `.md` file, including this one, needs a `type`.

- `index.md` — one per directory with more than one document, for progressive disclosure. No frontmatter, except the bundle-root `index.md`, which may declare `okf_version: "0.1"`. Entries: `* <status-emoji> [Title](path) - description`, description pulled from the linked doc's frontmatter.
- `log.md` — single change history at the bundle root, newest-first, ISO 8601 date headings. Each entry leads with a bold verb (`**Create**`, `**Update**`, `**Fix**`, `**Conformance**`) — convention, not a fixed vocabulary.
- `open-questions.md` — also root-level and singular, not per-folder. An earlier per-folder version of the open-questions tracker (`product/open-questions.md`, `design/open-questions.md`) was consolidated into the single wiki-wide one; don't recreate folder-local copies. (A former `references.md` ledger — every external link, collated in one place — was dropped 2026-07-03 as too laborious to maintain; citations now live only inline or in a concept's `resource:` frontmatter, per doc.)

**Folder routing** — where new content belongs:

| Folder | Covers | Boundary |
| --- | --- | --- |
| `/product/` | What the app must do — scope, behaviour, user journeys | Not visual design (Design) or implementation (Engineering) |
| `/design/` | How it looks and behaves visually — tokens, components, screens | Behaviour lives in Product |
| `/engineering/` | How it's built — architecture, stack | Behaviour in Product, visual design in Design, AI/agentic tooling in AI |
| `/qa/` | How it's verified — testing strategy | |
| `/devops/` | How it's shipped and governed — deployment, CI/CD, tooling | |
| `/ai/` | How AI/agentic workflows are used — agent skills, MCP tooling | Behaviour lives in Product; implementation lives in Engineering |

Root-level files (`index.md`, `log.md`, `open-questions.md`, `AGENTS.md`) sit outside any area folder — they're wiki-wide, not domain-specific. `OKF-SKILL.md` lives at [/ai/OKF-SKILL.md](/ai/OKF-SKILL.md) — it's a Skill Reference like the rest of `/ai/`'s contents, not wiki-wide policy, even though `AGENTS.md` here leans on it for OKF mechanics.

**Cross-linking** — bundle-absolute links (`/folder/file.md`) are preferred over relative ones; they survive files moving. Every non-root doc opens with a single back-link line before its `# Title`: `⬅ [Parent](/path/to/parent/index.md)`.

**Code fences** — any multiline code-like content (shell commands, JSON/YAML/CSS/JS/TS, directory trees, config file contents, etc.) goes in a fenced block with a language annotation — never a bare ` ``` ` or left unfenced. Single-line inline code (`` `like this` ``) is exempt.

Where a new doc should live within a folder, and when a topic has grown broad enough to deserve its own sub-folder and index — that's an authoring judgment call, and it's OKF-SKILL.md's territory (its "organize into a directory tree that makes sense for the domain" guidance), not a rule this file prescribes. See §3.

## 2. Frontmatter

| Field | Status here | Notes |
| --- | --- | --- |
| `type` | Required | Free-form per OKF, but see the taxonomy below — reuse an existing value where one genuinely fits, don't fragment near-duplicates |
| `title` | Used on every content doc | Human-readable display name |
| `description` | Used on every content doc | One sentence; this is what index.md entries pull from |
| `tags` | Used on every content doc | YAML list, cross-cutting |
| `timestamp` | Used on every content doc | ISO 8601, date of last meaningful change |
| `resource` | Used where there's an underlying external asset | e.g. a shadcn component page, the OMDb API root — omit for abstract concepts |
| `status` | Used on every content doc | **Not part of the core OKF spec** — a producer-defined extension this wiki relies on everywhere. See below. |

**Type taxonomy in current use** (descriptive, not exhaustive — OKF never rejects an unexpected type; add a new one here when a doc introduces it, per the existing precedent of adding `Onboarding Guide` when the onboarding sub-wiki was written):

`Agent Policy` · `AI Tooling` · `API Reference` · `Deployment` · `Design Component` · `Design Foundation` · `Design Screen` · `Engineering Architecture` · `Future Consideration` · `Onboarding Guide` · `Product Requirement` · `Reference` · `Skill Reference` · `Testing Strategy` · `Tracking` · `User Journey`

**Status field** — used everywhere but not yet fully standardized. Three canonical values are in majority use, each with a matching `index.md` emoji:

- `not started` — ⬜
- `in progress` — 🚧
- `confirmed` — ✅

Two docs currently carry typo'd variants of these three, and seven consistently use `reference only` or `living document` instead (this file included — see the frontmatter above) — an established but never-formalized fourth/fifth value for docs that are intentionally ongoing rather than working toward "confirmed." Whether to fold those into the three-value enum or formalize them is still open — see [Open Questions #4](/open-questions.md#4-status-values-that-dont-match-agentsmd-2s-three-value-enum). Don't silently normalize in either direction; treat any status value you're unsure how to classify as this same open question, not a new one.

## 3. Scope & source of truth

This file is the policy layer: how agents should behave in this bundle, and what its local conventions actually are (folder routing, frontmatter reality, working process). It is not:

- **OKF mechanics.** [OKF-SKILL.md](/ai/OKF-SKILL.md) owns validation, bundle/concept creation, enrichment, and general authoring judgment — including where the line falls on splitting a broad topic into its own sub-folder. If a structural question isn't answered by §1 above, it's an OKF-SKILL.md question, not a gap in this file to patch with a new invented rule.
- **Audit procedure, tooling recommendations, or skill candidates.** [/ai/audit-process.md](/ai/audit-process.md), [/ai/mcp-tooling.md](/ai/mcp-tooling.md), and [/ai/agent-skills.md](/ai/agent-skills.md) own those respectively. This file points at them rather than restating their content — the wiki's own audit process treats a doc restating another doc's authoritative content as a duplication finding, and this file shouldn't commit the violation it expects other docs to avoid.

## 4. Editing autonomy

- **Mechanical edits** need no sign-off: frontmatter fixes, link fixes, code-fence/formatting fixes, `index.md`/`log.md` upkeep, and anything covered by [/ai/audit-process.md](/ai/audit-process.md)'s Audit mode auto-fix rules.
- **Anything that asserts a new fact, decision, or scope call** — a product behaviour, a design treatment, an architectural choice, moving a doc's status toward `confirmed` — always goes through the Working Process in §5. Don't draft it unprompted and present it as settled.
- **Ambiguities and contradictions get logged to [/open-questions.md](/open-questions.md)**, never guessed at or silently dropped — whether they surface during an explicit audit or incidentally while doing ordinary content work.
- **Agents don't commit or push.** Edit files and leave the working tree as-is; James reviews the diff and commits.

## 5. Working process

- **One question at a time.** When a decision needs a human call, ask a single, narrowly-scoped question with enough context to decide. Don't move to the next open item until the current one is actually resolved in its doc(s) — don't batch multiple decisions into one question.
- **Closing an item out**, once resolved: update the doc's own `status` frontmatter → update the parent `index.md`'s status emoji to match (the doc's frontmatter is always the source of truth; fix the emoji to match the doc, never the reverse) → remove the item from `/open-questions.md` if it was tracked there → record the change in `/log.md`.
- This is the same loop [/ai/audit-process.md](/ai/audit-process.md)'s Resolve mode formalizes for the audit tracker specifically — it's the general working process for any decision made in this wiki, not just audit findings.
