# gpt-image-gen — Pointer

The skill yuval uses for image generation lives at `.claude/skills/gpt-image-gen/SKILL.md`.

## What it does

Wraps the OpenAI Images API:
- curl-based primary path (with `jq` for base64 decode)
- Python fallback for environments without `jq` (Git Bash / Windows)
- Requires `OPENAI_API_KEY` in `.env`

## Reusability

Any future agent in this project can reuse this skill — it's not yuval-specific. Yuval is just the first consumer. To add a second consumer, reference the skill at `.claude/skills/gpt-image-gen/SKILL.md` from the new agent's workflow.
