# Yuval — Creative Image Agent (יובל)

## Overview

Yuval (יובל) is the visual-creative sub-agent of the multi-agent system. Defined at `.claude/agents/yuval.md`, he uses `claude-opus-4-7` and the tools Read, Write, Bash, Glob. His primary responsibility: generate images that are visually consistent with the project's style by scanning `yuval/reference/` for style cues before every generation. He is Jacob's agent-1 in the Sub-Agent Registry.

**Skill dependency:** `gpt-image-gen` (`.claude/skills/gpt-image-gen/SKILL.md`) — wraps the OpenAI Images API with a curl primary path and Python b64 fallback. Requires `OPENAI_API_KEY` in `.env`.

**Workspace:** `yuval/` (project root) — `reference/` for style images, `outputs/` for generated PNG + sibling `.txt` prompt files.

## Open Questions

- What reference images will be added to `yuval/reference/`?
- Should yuval support multi-image batch requests natively (currently runs the workflow once per image)?
- Should Jacob be able to pass a reference image path directly, overriding the reference/ scan?

## Session Log

### 2026-05-06 — Yuval agent created + gpt-image-gen skill added [shipped]

- **What was done:**
  - Created `.claude/skills/gpt-image-gen/SKILL.md` — OpenAI Images API wrapper (model: gpt-image-2, 1024×1024, medium quality, PNG output). Primary path: curl + jq base64 decode. Fallback: Python one-liner for Git Bash / Windows where jq is absent. Explicit error handling for missing API key, HTTP failures, and empty b64_json.
  - Created `.claude/agents/yuval.md` — flat agent file with YAML frontmatter (name, description with trigger keywords, model=claude-opus-4-7, tools=[Read, Write, Bash, Glob]). Hebrew/English bilingual persona. 7-step workflow: scan reference/ → select cues → compose prompt → invoke gpt-image-gen → save outputs/<date>-<slug>.png + .txt → verify size > 0 → report.
  - Created `yuval/` workspace at project root: `reference/` (empty, for style images), `outputs/` (empty, for generated PNGs), `agent.md` (human pointer → .claude/agents/yuval.md), `skill.md` (human pointer → .claude/skills/gpt-image-gen/SKILL.md).
  - Added `OPENAI_API_KEY=your_openai_api_key_here` to `.env.example`. (`.env` already had the key set.)
  - Updated `jacob.md`: replaced agent-1 placeholder row in Sub-Agent Registry with yuval's real entry. Added new "Routing Triggers" section (Hebrew + English keywords) for Phase 0 routing.
- **Decisions:** Hybrid file layout: `.claude/agents/yuval.md` (Claude Code discovery) + `yuval/` root workspace (images). Python fallback in gpt-image-gen is intentional — jq is not typically present in Git Bash on Windows. Model name `gpt-image-2` kept verbatim per user spec. Trigger keywords section added to jacob.md as a new `## Routing Triggers` section rather than a table column — cleaner and extensible.
- **Notes / Caveats:** `yuval/reference/` is currently empty. Until reference images are added, yuval generates from the user's prompt alone (and notes this in the report). `OPENAI_API_KEY` was already present in `.env` — no key was added, only `.env.example` updated.
- **Related:** [[jacob-ceo-agent]], [[project-structure]]
