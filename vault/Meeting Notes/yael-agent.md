# Yael — Content Writer Agent (יעל)

## Overview

Yael (יעל) is the content writer/rewriter sub-agent of the multi-agent system. Defined at `.claude/agents/yael.md`, she uses `claude-opus-4-7` and is intentionally **LLM-only**: tools are `Read`, `Write`, `Edit`, `Glob`, `Grep` — no `Bash`, no `Task`, no web access, no API. Her job: take raw articles from `Content/`, rewrite them in the project's voice (defined in `yael/style-guide.md` + examples in `yael/reference/`), and save the rewrite to `Output/<name>.md`. She is Jacob's agent-2 in the Sub-Agent Registry.

**Architectural constraint addressed:** Claude Code sub-agents cannot invoke other sub-agents — only Jacob (the CEO, who has `Task`) can. Therefore, when Yael identifies a need for an image, she does **not** call Yuval. Instead, she inserts `{{IMAGE_NEEDED: "<detailed description>"}}` placeholders in the article. Jacob resolves these in a new **Phase 1.5** of his protocol: he iterates through each placeholder, invokes Yuval per placeholder, and uses `Edit` with exact-string replacement to swap the placeholder for a Markdown image reference pointing at Yuval's output.

**Workspace:** `yael/` (project root) — `style-guide.md` (canonical voice/tone/structure rules), `reference/` (example texts, currently empty), `agent.md` (human pointer → `.claude/agents/yael.md`).

**Article flow:**

```
Content/<name>.md  →  yael (rewrite)  →  Output/<name>.md  +  Content/Ready/<name>.md (archive copy)
                                                ↓
                                      {{IMAGE_NEEDED}} placeholders
                                                ↓
                                  Jacob → Yuval → Edit-stitch  →  final Output/<name>.md
```

The original article in `Content/<name>.md` **stays in place** after Yael processes it — she has no `Bash` to delete it. The `Ready/` copy is the audit trail.

## Open Questions

- What kind of "already processed" detection should the next flow run use? Currently Glob will re-pick the original from `Content/` on the next invocation.
- Should the style guide be versioned? If voice changes, do we want old rewrites to remain consistent with the old guide, or get retroactively updated?
- Should Yael support multi-article batch requests, or strictly one article per invocation?
- What is the right density heuristic for image placeholders? Style guide says ~1 per 300–500 words — needs validation against real articles.

## Session Log

### 2026-05-06 — Yael agent created + Jacob updated with Phase 1.5 IMAGE_NEEDED protocol [shipped]

- **What was done:**
  - Created `.claude/agents/yael.md` — flat agent file with YAML frontmatter. `tools: [Read, Write, Edit, Glob, Grep]` — explicitly LLM-only (no Bash, no Task, no Web*). Hebrew/English bilingual persona. 8-step workflow: read style baseline (style-guide.md + reference/) → locate raw article in `Content/` → analyze → rewrite preserving meaning + voice → insert `{{IMAGE_NEEDED}}` placeholders where visuals genuinely help (~1 per 300–500 words) → save to `Output/<name>.md` → archive copy to `Content/Ready/<name>.md` (no `mv`/`rm` available) → report back to Jacob with rewrite summary + placeholder list.
  - Created `yael/style-guide.md` — full Hebrew style guide covering: tone (friendly-professional, second-person), sentence rhythm (mix short/medium/long, ~15-word average), 5-part article structure (title ≤8 words → opener → 3–5 body blocks → summary → optional CTA), word choice preferences and avoid-list, English-in-Hebrew rules, source-citation discipline (`[צריך מקור]` for unsupported claims), Markdown conventions, and 3 before/after examples.
  - Created `yael/agent.md` — human pointer doc explaining the workspace layout and architecture diagram.
  - Created `yael/reference/.gitkeep`, `Content/.gitkeep`, `Content/Ready/.gitkeep`, `Output/.gitkeep` — empty placeholders so git tracks the directory structure.
  - Updated `.claude/agents/jacob.md` — three surgical edits:
    1. **Sub-Agent Registry**: replaced agent-2 placeholder row with yael's real entry.
    2. **Routing Triggers**: added a yael block with Hebrew (שכתב, ערוך, נסח מחדש, תרגם, סכם, מאמר, תוכן, פוסט) and English (rewrite, edit, rephrase, translate, summarize, article, content, post) keyword lists.
    3. **Phase 1.5 — Image Placeholder Resolution (Yael → Yuval bridge)**: a new ~50-line section inserted between Phase 1 (Sequential Agent Execution) and the Retry Protocol. Defines the 6-step bridge: parse placeholders → for each, invoke Yuval sequentially → stitch images via `Edit` exact-replacement → verify zero `{{IMAGE_NEEDED:` remain → log to vault/Meeting Notes/ → proceed to Phase 2. Includes an explicit graceful-degradation rule: if Yuval fails on a single placeholder after retry, leave it in place under "Pending images" and continue with the rest — do **not** abort the whole chain.

- **Decisions:**
  - **LLM-only tool whitelist for Yael** — explicit and intentional. Trading some convenience (cannot move files, cannot self-orchestrate Yuval) for clean separation of concerns: writers write, orchestrators orchestrate.
  - **Move semantics for original article**: Yael writes a *copy* to `Content/Ready/`, leaves the original in `Content/`. User-confirmed decision over option B (add Bash) or option C (Jacob does the move). The implication — `Glob Content/*.md` on the next run will still see the article — is acknowledged and deferred (out of scope for this task).
  - **Phase 1.5 placement** — bridge logic lives in Jacob, not Yael, because invoking sub-agents requires `Task` which Yael does not have. Phase 1.5 sits between Phase 1 (single agent execution) and Phase 2 (Final Output Assembly) because it triggers another agent (Yuval) and modifies the prior agent's output before assembly.
  - **Single-placeholder failure tolerance** — graceful degradation, not chain abort. Rationale: a missing image should not destroy a successfully rewritten article. The Final Report surfaces "Pending images" so the human can re-run that piece.
  - **Style guide content** — Hebrew-first, business/web-content focused, with concrete before/after examples. Built generic enough to apply to most article types; specific enough to actually constrain voice. Will evolve as `yael/reference/` is populated with real examples.

- **Notes / Caveats:**
  - `yael/reference/` is currently empty. Until examples are added, Yael works only from `style-guide.md`.
  - No verification end-to-end was run (no test article was supplied with the request). Manual verification suggested in the plan: drop a test article into `Content/`, ask Jacob to "שכתב את המאמר", confirm Yael produces `Output/<name>.md` with placeholders, and Jacob successfully bridges to Yuval.
  - Filename collision in `Output/` — if the same article is rewritten twice, the second run overwrites the first. Acknowledged, deferred.

- **Related:** [[jacob-ceo-agent]], [[yuval-agent]], [[project-structure]]
