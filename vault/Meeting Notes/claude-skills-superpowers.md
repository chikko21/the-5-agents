# Claude Skills — Superpowers

## Overview
14 skills from the [obra/superpowers](https://github.com/obra/superpowers) plugin, installed manually into `.claude/skills/`. Each skill is a folder with a `SKILL.md` defining when and how Claude should invoke it. They cover the full development lifecycle: planning → implementation → testing → review → shipping.

## Open Questions
- none

## Session Log

### 2026-05-06 — Installed Superpowers plugin (14 skills) [shipped]
- **What was done:** Cloned `obra/superpowers`, copied all 14 skill folders into `.claude/skills/`. No existing files were overwritten.
- **Decisions:** Manual install (no `/plugin` CLI available). Used `cp -rn` to avoid clobbering.
- **Notes / Caveats:** Skills are read-only reference material — do not edit SKILL.md files directly without using [[writing-skills-skill]].
- **Related:** [[project-structure]], [[claude-skills-obsidian]]

---

## Skills Reference

| Skill folder | When to use |
|---|---|
| `brainstorming` | Before any creative work — features, components, new behavior |
| `dispatching-parallel-agents` | When 2+ independent tasks can run in parallel |
| `executing-plans` | When executing a written plan in a separate session |
| `finishing-a-development-branch` | After implementation complete, before merge/PR |
| `receiving-code-review` | When receiving review feedback, before implementing |
| `requesting-code-review` | After completing features, before merging |
| `subagent-driven-development` | When executing plans with independent tasks in current session |
| `systematic-debugging` | Before proposing fixes for any bug/test failure |
| `test-driven-development` | Before writing implementation code |
| `using-git-worktrees` | When feature work needs isolation from main workspace |
| `using-superpowers` | At session start — establishes how to discover and invoke skills |
| `verification-before-completion` | Before claiming work is done / committing / PRs |
| `writing-plans` | When given a spec, before touching code |
| `writing-skills` | When creating or editing skill files |
