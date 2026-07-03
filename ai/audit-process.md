---
type: AI Tooling
title: Wiki Audit Process
description: Process for auditing the wiki against the OKF v0.1 spec — mechanical issues are auto-fixed, ambiguous findings are tracked for human Q&A.
tags: [ai, audit, okf, tracking]
timestamp: 2026-07-03T00:00:00Z
status: confirmed
---

⬅ [AI](/ai/index.md)

# Wiki Audit Process

Defines how an agent audits this wiki for conformance to the Open Knowledge Format (OKF) v0.1, as specified in `AGENTS.md`. This is the source of truth for the `wiki-audit` skill (see [Agent Skills](/ai/agent-skills.md) for where skills live) — deliberately kept as plain markdown, not a Claude-specific format, so it can be handed to any agent or tool without rewriting.

There are three modes. They are not interchangeable — mode determines what's safe to run unattended.

## Audit mode (mechanical, cron-safe)

Fully deterministic checks only. No judgment calls. Safe to run unattended (manually invoked or on a schedule) because every action taken is either a confirmed fix or a logged escalation — nothing is guessed.

### Checks

`AGENTS.md` is the single source of truth for what "conformant" means — its §1 (structure rules) and §2 (frontmatter template, including the type taxonomy and the status field's current values) define every rule being checked here. This doc doesn't restate those rules; it only says how they get checked, fixed, or escalated. Re-read `AGENTS.md` §1–§2 directly for the actual rule text before auditing — don't rely on a paraphrase of it living in two places.

On top of enforcing `AGENTS.md` §1, §2, and the status-emoji parity rule in §5, audit mode also checks **link integrity**, which isn't defined anywhere else:
- Every bundle-absolute internal link resolves to a file that actually exists at that path.
- Every external link (in `/references.md` or inline) is reachable — best-effort; skip silently if the environment running the audit has no network access, don't escalate a network failure as a dead link.
- **`/references.md` ledger sync**: every outbound (`http(s)://`) link cited anywhere in the wiki — inline markdown links or a concept's `resource:` frontmatter field — has a matching entry in `/references.md`. This direction (missing entry) is mechanical. The reverse — a `/references.md` entry with no hyperlink citation elsewhere — is *not* automatically "stale": this wiki's convention is to often name a tool/component/library in prose (e.g. "shadcn Empty component", "Noto Serif Variable") without inlining its URL at every mention, relying on `/references.md` as the one place the link lives. Confirming a link is genuinely orphaned (vs. just prose-cited without a hyperlink) requires reading the surrounding text, which is a judgment call, not a mechanical match — see auto-fix/escalation split below.
- **Code-fence formatting** (`AGENTS.md` §1): any multiline code-like content (shell commands, JSON/YAML/CSS/JS/TS, directory trees, config file contents, etc.) is inside a ` ``` ` fence with a language annotation — never a bare ` ``` ` and never left unfenced. Single-line inline code (`` `like this` ``) is out of scope; it was never required to fence.

### Auto-fix rules

Only fix when the fix is unambiguous. Specifically:

- **Broken internal link, target found elsewhere**: if exactly one file in the wiki matches the broken link's filename or old path, rewrite the link to the correct bundle-absolute path. If more than one file could plausibly match, or none do, don't guess — escalate instead.
- **`type` value is a near-miss typo** of a taxonomy entry (checked against `AGENTS.md` §2 directly, e.g. `Api Reference` vs `API Reference`) — correct to the canonical value.
- **Status-emoji mismatch** — fix per the parity rule in `AGENTS.md` §5.
- **Missing `timestamp`** where it can be derived from the file's last commit date — fill it in.
- **`/references.md` missing entry**: a cited outbound link (inline or `resource:`) not yet in `/references.md` — append it as a new numbered entry, using the link text from where it's cited (first occurrence in file-path alphabetical order, if cited with different text in more than one place) as the reference title. Renumber the list after any addition so numbering stays sequential. Adding is low-risk (nothing is lost), so this direction is a straight auto-fix.
- **Bare code fence, language unambiguous from content**: a ` ``` ` with no language tag wrapping content whose language is obvious (shell/env output, JSON, YAML, CSS, a directory tree, etc.) — add the tag (`bash`, `json`, `yaml`, `css`, `text` for trees/plain output, etc.). If the content's language genuinely isn't clear, don't guess — escalate instead.

Every auto-fix gets logged to `/log.md` (dated entry, `**Fix**` verb) so it's visible, even though no one approved it in the moment.

### Escalation rules

Anything not covered by an auto-fix rule above gets appended to `/open-questions.md` instead of edited — never guessed at. This includes:

- A broken link with zero or multiple plausible targets (genuinely dead, or ambiguous).
- A `type` value that isn't a near-miss of the `AGENTS.md` §2 taxonomy — looks like a genuinely new type, which needs a human decision on whether to add it or reuse an existing one.
- A structural violation that implies a rename or restructure (e.g. a `README.md` that should become `index.md`, or a doc that's grown too broad and should become its own sub-folder) — these can break other files' links if acted on automatically, so they're always escalated.
- Anything that looks like a cross-document contradiction or duplication noticed in passing. Audit mode does **not** go looking for these on its own — see Semantic mode below — but if one is obvious while doing the mechanical sweep, note it rather than discard it.
- A `/references.md` entry with no matching hyperlink citation elsewhere in the wiki. Check first whether its subject is still named in prose without a link (common in this wiki — see the ledger-sync check above); if so, escalate as "possibly still relevant, just not hyperlinked" rather than removing it. Never delete a `/references.md` entry as an auto-fix — the risk of dropping a genuinely-still-used resource is higher than the cost of one extra open question.
- A bare code fence whose language can't be confidently inferred from its content (e.g. ambiguous pseudo-code, or content that's a mix of more than one language/format).

Each escalation entry in `/open-questions.md` should include: which doc(s) are involved, what was found, and why it couldn't be auto-resolved.

## Semantic mode (manual only, judgment-based)

Never runs on a schedule — every finding here is a judgment call, and always escalates, never auto-fixes. Triggered explicitly (e.g. "check the wiki for duplication," "semantic audit," "any contradictions in `/design`?").

Looks for two things a mechanical sweep can't catch:

- **Source-of-truth duplication** — a doc restates a rule, fact, or definition that's already authoritatively defined elsewhere (e.g. a process doc re-listing `AGENTS.md`'s frontmatter template instead of pointing to it). The tell: if the "source" doc changed, would the restating doc silently go stale? If yes, it's a duplication, not independent content.
- **Cross-document contradiction** — two docs describe the same behaviour, fact, or decision differently (e.g. `/product/open-questions.md`'s original Watched-View example: product confirms a capability design's copy doesn't reflect).

Both get escalated to `/open-questions.md`, never fixed directly — resolving a duplication means deciding which doc is authoritative and how to rephrase the other as a reference, and resolving a contradiction means deciding which version is correct. Both are calls for a human, not the audit. An escalation entry here should name every doc involved, quote or summarize the overlapping/conflicting content, and say which one looks authoritative if that's obvious (without deciding it unilaterally).

This mode has no fixed checklist — it requires actually reading and comparing docs, not pattern-matching. Scope a session to a folder or topic rather than attempting the whole wiki at once.

## Resolve mode (manual only, interactive)

Never runs on a schedule — it requires a human present to make judgment calls. Triggered explicitly (e.g. "resolve open questions," "let's go through the audit tracker").

Applies `AGENTS.md` §5's working process to the tracker specifically: walk `/open-questions.md` top to bottom, one item at a time, following §5's ask → resolve → close-out loop for each. See `AGENTS.md` §5 for the actual steps — nothing here overrides or adds to them; the only thing specific to this mode is the top-to-bottom walk order.

## Triggering

- **Manual**: any mode can be invoked directly in a session ("audit the wiki," "check for duplication," "resolve open questions").
- **Scheduled**: only Audit mode should ever be attached to a recurring/cron schedule. Semantic and Resolve modes are excluded from scheduling by design — both require a human present, either to make a judgment call or to answer one.
