# Claude Skills — Obsidian

## Overview
3 Obsidian-specific skills installed in `.claude/skills/`. They handle vault read/write workflow, Obsidian Flavored Markdown authoring, and Obsidian Bases (database views). Together they make Claude a capable vault editor that follows this project's documentation protocol.

## Open Questions
- The skill folders have timestamp suffixes (e.g. `obsidian-vault-workflow-20260506T115537Z-3-001`) — decide whether to rename them to clean paths or leave as-is.

## Session Log

### 2026-05-06 — Obsidian skills discovered and vault initialized [shipped]
- **What was done:** Found 3 obsidian skills already present in `.claude/skills/`. Built the vault folder structure (`vault/Meeting Notes/`, `vault/Content Briefs/`, `vault/Publishing Log/`, `vault/Brand Guidelines/`). Created all topic files and `_index.md` files.
- **Decisions:** Vault root is the project root (`.obsidian/` sits at root). Sub-folder `vault/` used for notes as specified by `obsidian-vault-workflow` skill. Skill mandated on every session start via CLAUDE.md.
- **Notes / Caveats:** `vault 5 agent/` directory exists but is currently empty — may be legacy or user-intended scratch space.
- **Related:** [[project-structure]], [[vault-setup]], [[claude-skills-superpowers]]

---

## Skills Reference

| Skill folder | Purpose |
|---|---|
| `obsidian-vault-workflow-20260506T115537Z-3-001/obsidian-vault-workflow` | **Mandatory**: read vault before task, write session log after. Governs all note creation/update. |
| `obsidian-markdown-20260506T115539Z-3-001/obsidian-markdown` | Creating/editing Obsidian Flavored Markdown — wikilinks, callouts, embeds, properties |
| `obsidian-bases-20260506T115547Z-3-001/obsidian-bases` | Creating `.base` files for database-like views of notes |
