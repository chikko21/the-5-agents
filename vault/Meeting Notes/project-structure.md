# Project Structure

## Overview
The project is a multi-agent content creation system ("the-5-agents") where a CEO agent orchestrates a team of specialized sub-agents. The repository root serves as the Obsidian vault. Core config files live at root level; all Claude agent/skill/command definitions live under `.claude/`.

## Open Questions
- Which sub-agents will be built first (writer, editor, publisher, etc.)?
- Will the stack be Python (anthropic SDK) or another language?
- Is `vault 5 agent/` a legacy folder or still in use?

## Session Log

### 2026-05-06 — Initial project setup [shipped]
- **What was done:** Created repo skeleton — `CLAUDE.md`, `.env`, `.env.example`, `.gitignore`, `.claude/agents/`, `.claude/skills/`, `.claude/commands/` directories. Pushed to GitHub at `chikko21/the-5-agents`.
- **Decisions:** Used `.gitignore` to exclude `.env`; `.env.example` shared as template. Git identity set to `barak21@gmail.com`.
- **Notes / Caveats:** `ANTHROPIC_API_KEY` in `.env` is still empty — must be filled before any agent code runs.
- **Related:** [[claude-skills-superpowers]], [[claude-skills-obsidian]], [[vault-setup]]
