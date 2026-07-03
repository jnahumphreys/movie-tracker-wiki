# Working with the Movie Tracker Wiki

This project maintains a wiki, conformant to the **Open Knowledge Format (OKF) v0.1**. Follow these rules whenever creating, editing, or discussing wiki content.

## 1. Structure rules (non-negotiable)

- **Every folder's hub file is `index.md`** — never `README.md`. `index.md` and `log.md` are reserved; never use them for concept content.
- **Only the root `index.md` has frontmatter** (`okf_version: "0.1"`). All other `index.md` files have none — just a short intro + bullet listings (`* [Title](/path.md) - description`).
- **Every other `.md` file (a "concept") MUST have YAML frontmatter with a `type` field.** This is the one hard OKF requirement — never skip it.
- **All internal links are bundle-absolute**, starting with `/` (e.g. `/product/mvp-requirements.md`), not relative (`../foo.md`). This survives files being moved.
- **Log significant changes in the root `log.md`** — dated entries, bold action verb (`**Create**`, `**Update**`), one line each.

## 2. Concept frontmatter template

```yaml
---
type: <Type — reuse one below, or propose a new one>
title: <Human title>
description: <One sentence>
resource: <optional — canonical external URL this concept documents>
tags: [lowercase, kebab-case]
timestamp: <ISO 8601 — date of last significant edit>
status: not started | in progress | confirmed
---
```

**Existing type taxonomy — reuse these, don't fragment it:**
`Product Requirement` · `Future Consideration` · `User Journey` · `Design Foundation` · `Design Component` · `Design Screen` · `Tracking` · `Engineering Architecture` · `API Reference` · `AI Tooling` · `Testing Strategy` · `Deployment` · `Reference`

## 3. Where new content goes

| Content is about...                                  | Folder         |
| ---------------------------------------------------- | -------------- |
| What the app must do, scope, user behaviour          | `product/`     |
| How it looks — tokens, components, screens           | `design/`      |
| How it's built — stack, architecture, dev tooling    | `engineering/` |
| How it's verified — testing                          | `qa/`          |
| How it's shipped/governed — deploy, CI, repo tooling | `devops/`      |
| How we use AI/agentic workflows, agent skills, MCP tooling | `ai/`   |

If unsure which folder, ask — don't guess and split it wrong later.

## 4. When a topic is too broad for one doc

Give it its own sub-folder with an `index.md` (see `design/screens/omdb-search/` as the pattern). Don't let a single file try to cover multiple distinct states/screens.

## 5. Working process

- Build the wiki **incrementally, one document at a time**, via interview — one question per message, narrowing to a recommendation rather than open-ended choices.
- Update a doc's `status` field as it moves from ⬜ not started → 🚧 in progress → ✅ confirmed. Reflect the same emoji in the parent `index.md` listing.
- When a decision resolves an item in `/open-questions.md` (the wiki-wide tracker), move it to the relevant doc and remove it from the tracker — don't let resolved items linger.
- Every new or edited concept doc: add/update its frontmatter, add it to its parent `index.md` if new, and cross-link it from related docs in other folders (e.g. a new Design screen should link back to its Product user journey), finally update `log.md` with changes.
- When updating the log, prepend the entry to the top of the log, so the log reads in chonological order. For log entries created on the same date, also prepend the log entry to the top of the entries for that given date.
- A conformance audit against this spec (mechanical issues auto-fixed, ambiguous findings escalated to `/open-questions.md`) is defined in `/ai/audit-process.md`, wrapped by the `wiki-audit` skill. Only its audit mode is safe to schedule unattended; resolving flagged items is always a manual, interactive session.

## 6. Don't

- Don't reintroduce a flat `Requirements.md` or `Design-Brief.md` — Product is the single source of behavioural truth now.
- Don't add relative links (`../`) — always bundle-absolute (`/...`).
- Don't skip `type` frontmatter, even for "quick" stub docs.
- Don't dump multiple new documents on the user at once without checking in — confirm scope for one document before moving to the next.
