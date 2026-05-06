# Yuval — Workspace

This directory is yuval's **workspace**, not his definition.

The canonical agent definition lives at `.claude/agents/yuval.md`. Claude Code only discovers agent files that sit flat under `.claude/agents/` — nested directories are ignored — so the actual agent logic must stay there.

## What lives here

- `reference/` — visual style references (PNG / JPG / WebP). Yuval reads every file here before each generation to extract palette, composition, and motifs.
- `outputs/` — generated images plus sibling `.txt` files containing the exact prompts used (for iteration).

## How to modify yuval

Edit `.claude/agents/yuval.md`. Don't put agent prompts or workflow logic in this folder — they won't be loaded by Claude Code.
