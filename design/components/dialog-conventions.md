---
type: Design Component
title: Dialog Conventions
description: Shared layout and dismiss-behaviour pattern for all dialogs in the app.
resource: https://ui.shadcn.com/docs/components/radix/dialog
tags: [design, component, dialogs]
timestamp: 2026-07-03T00:00:00Z
status: confirmed
---

# Dialog Conventions

Source of truth for shared structure across every dialog in the app (currently: [Movie Actions Dialog](/design/components/movie-actions-dialog.md), [Add API Key Dialog](/design/components/add-api-key-dialog.md)). Dialog-specific copy, variant counts, and behaviour live in each dialog's own doc — this doc only covers what's shared.

## Button layout
- Buttons are **stacked, full-width**, in this order:
  1. Contextual action(s) — **primary (`default`) variant**. (Where a dialog has more than one contextual action at equal priority — e.g. Movie Actions Dialog's "not in library" variant — all of them are `primary`, stacked in this slot.)
  2. Destructive action, if the dialog has one — **danger (`destructive`) variant**.
  3. A horizontal [Separator](https://ui.shadcn.com/docs/components/radix/dialog), to visually separate contextual/destructive actions from Cancel.
  4. "Cancel" — **secondary variant**.

## Dismiss behaviour
Escape key, backdrop click/touch, or the footer "Cancel" button all close the dialog without performing an action. **No `X` close button** on any dialog — this is deliberately excluded app-wide, even though shadcn's Dialog includes one by default.
