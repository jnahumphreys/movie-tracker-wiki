---
name: wiki-audit
description: Audit the Movie Tracker wiki for Open Knowledge Format (OKF) v0.1 conformance — dead/broken internal links, malformed or drifted frontmatter, type-taxonomy fragmentation, status/emoji mismatches, plus (manual only) source-of-truth duplication and cross-document contradictions. Auto-fixes unambiguous mechanical issues and escalates anything needing human judgment to /open-questions.md. Use when asked to audit, check, lint, clean up, check for duplication/contradictions, or "resolve open questions" on the wiki, or when running on a schedule.
---

# Wiki Audit

This skill is a thin pointer. The full process — what counts as mechanical vs. ambiguous, exactly what gets auto-fixed, and how escalations are written up — is defined in [`/ai/audit-process.md`](/ai/audit-process.md). Read that file in full before doing anything; it is the source of truth, kept as plain markdown (not Claude-specific) so it stays portable to other tools.

## Quick reference

There are three modes — don't conflate them:

1. **Audit mode** (default; safe to run unattended, including on a schedule) — scan the wiki, auto-fix only the deterministic, unambiguous issues described in `/ai/audit-process.md`, append everything else to `/open-questions.md`, and log every change to `/log.md`.
2. **Semantic mode** (manual only — invoke explicitly, e.g. "check for duplication," "semantic audit") — read and compare docs for source-of-truth duplication or cross-document contradictions. Always escalates to `/open-questions.md`, never auto-fixes; these are judgment calls. Never schedule this mode.
3. **Resolve mode** (manual only — invoke explicitly, e.g. "resolve open questions") — walk `/open-questions.md` interactively, one item at a time, per `AGENTS.md`'s existing one-question-at-a-time process. Never schedule this mode; it requires a human to answer.

## Hard rule

Never guess on an ambiguous finding. If `/ai/audit-process.md`'s auto-fix rules don't clearly cover a case, escalate it to `/open-questions.md` instead of editing the doc.
