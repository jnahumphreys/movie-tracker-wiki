---
type: Design Foundation
title: Viewports & Breakpoints
description: Target viewport and breakpoint scope for MVP.
tags: [design, foundations, responsive]
timestamp: 2026-07-02T00:00:00Z
status: confirmed
---

# Viewports & Breakpoints

- Mobile/touch-first. Per [MVP Requirements](/product/mvp-requirements.md), the app must work across mobile, tablet, laptop, and desktop — but **no dedicated tablet/laptop/desktop mockups at MVP**. Larger viewports are satisfied by stretching the mobile layout, not by designing separate breakpoint layouts.
- **Container approach:** above mobile, content stretches into a `max-w-3xl` (Tailwind) container, horizontally centred in the viewport. No new layouts, components, or breakpoint-specific designs — just the existing 360px design reflowing into a wider, centred column.
- **Working design viewport: 360px** (common Android width) — the baseline for all mockups unless stated otherwise.
