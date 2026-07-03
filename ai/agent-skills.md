---
type: AI Tooling
title: AI Agent Skills — Product Owner
description: Pre-built Claude agent skills researched for a product-owner-focused assistant on this project; adoption not yet decided.
tags: [ai, agent-skills, product-owner]
timestamp: 2026-07-03T00:00:00Z
status: in progress
---

⬅ [AI](/ai/index.md)

# AI Agent Skills — Product Owner

Research into pre-built Claude agent skills that could act as a product-owner assistant for this wiki — writing specs, managing the roadmap, prioritizing scope, synthesizing research. See [MCP Tooling](/ai/mcp-tooling.md) for the development-side equivalent.

No skill has been installed yet — this is a candidate shortlist for a future decision.

## Candidates

### Anthropic Product Management plugin (recommended)

Official plugin, built primarily for Cowork but works in Claude Code too. `anthropics/knowledge-work-plugins/product-management`, installed via `claude plugins add knowledge-work-plugins/product-management`.

Eight skills: `write-spec` (PRDs), `roadmap-update`, `sprint-planning`, `stakeholder-update`, `synthesize-research`, `competitive-brief`, `metrics-review`, `product-brainstorming`. Supports connecting Linear/Asana/Notion/Slack/Figma/Amplitude/Intercom via MCP for richer context — none of which this project currently has connected.

### agile-product-owner (community, narrower)

`alirezarezvani/claude-skills`, `product-team/agile-product-owner`. A single skill focused specifically on Scrum backlog mechanics: INVEST-compliant user stories, Given-When-Then acceptance criteria, epic-splitting techniques, sprint capacity/velocity formulas. Good complement to the Anthropic plugin's `sprint-planning` skill if stricter INVEST/backlog rigor is wanted.

### pm-skills marketplace (community, broadest)

`phuryn/pm-skills` — 68 skills across 9 plugins spanning the full PM lifecycle (discovery, strategy, execution, GTM, analytics, growth). The `pm-execution` plugin is the most directly relevant (`user-stories`, `sprint-plan`, `create-prd`, `prioritization-frameworks`, `test-scenarios`). Explicitly documents copying `SKILL.md` folders into a project for tools without native plugin support.

## Open decision: where skills would live

Claude Code / Cowork auto-load project skills from `.claude/skills/<name>/SKILL.md` — a plain `/skills/` folder at the repo root would only be reference material Claude reads when asked, not something it auto-activates. Which of these (if either) to use, and which candidate(s) to adopt, is still open.
