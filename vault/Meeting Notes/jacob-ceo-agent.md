# Jacob — CEO Router Agent

## Overview
Jacob (יעקב) is the CEO Router Agent and main entry point of the multi-agent content creation system. Defined at `.claude/agents/jacob.md`, he uses `claude-opus-4-7` and the Claude Code Task tool to orchestrate a sequential chain of up to 4 sub-agents. Key design: sequential-only execution, 3-retry error handling with structured abort, clean context extraction per step, and a 5-point QA checklist before any output is forwarded.

## Open Questions
- Which agents will fill registry slots 2–4 (names, roles, models)?
- Should Jacob support dynamic agent discovery (scan `.claude/agents/` at runtime)?
- Should retry count be configurable via the task prompt?

## Session Log

### 2026-05-06 — Jacob CEO agent created [shipped]
- **What was done:** Created `.claude/agents/jacob.md` with YAML frontmatter (name, description, model=claude-opus-4-7, tools=[Task, Read, Write, Bash]), Hebrew/English persona, sub-agent registry table (4 placeholders), full sequential execution protocol (Phases 0–2), retry logic (3 attempts → abort with Error Report), QA checklist, context handoff template, Final Report and Error Report output formats. Updated `CLAUDE.md` to designate Jacob as the single entry point for all production tasks.
- **Decisions:** `claude-opus-4-7` chosen for Opus-level reasoning on routing and QA evaluation. Sub-agents left as symbolic placeholders — registry table to be filled row-by-row as agents are built. YAML frontmatter added (the three existing evaluation agents predate this convention). Prose style mirrors existing agents: imperative H3 steps, explicit tables.
- **Notes / Caveats:** Sub-agent files (agent-1.md through agent-4.md) do not yet exist — Jacob will surface clear Error Reports when Task calls fail for missing agents. This is expected behavior.
- **Related:** [[project-structure]], [[vault-setup]], [[claude-skills-superpowers]]

### 2026-05-06 — Yuval added as agent-1; Routing Triggers section added [shipped]

- **What was done:** Replaced agent-1 placeholder row in Sub-Agent Registry with yuval's entry. Added `## Routing Triggers` section listing Hebrew + English keywords that map to yuval (תמונה של, ציור של, generate image, etc.). Jacob now has one real registered sub-agent.
- **Related:** [[yuval-agent]]
