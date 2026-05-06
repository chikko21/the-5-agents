---
name: gpt-image-gen
description: "Generate images from text prompts via the OpenAI Images API. Use when an agent needs to render an image, illustration, or visual asset. Reads OPENAI_API_KEY from environment, writes a PNG file at the requested output path. Curl-based primary path with a Python b64 fallback for environments without jq (Git Bash / Windows)."
---

# gpt-image-gen — OpenAI Images API Wrapper

## When to use

Any agent that needs to generate an image from a text prompt. Used by **yuval** (visual creative agent), and reusable by any future agent that needs imagery.

## Setup

Requires `OPENAI_API_KEY` set in the process environment. The project's `.env` (gitignored) holds the key; the calling shell must load it before invoking this skill.

```bash
# bash / Git Bash
set -a; . .env; set +a
```

```powershell
# PowerShell
Get-Content .env | Where-Object { $_ -match '^[^#].*=' } | ForEach-Object {
  $kv = $_ -split '=', 2
  [System.Environment]::SetEnvironmentVariable($kv[0].Trim(), $kv[1].Trim(), 'Process')
}
```

## Inputs

- `<the prompt>` — full text prompt for the image (single argument, may be long)
- `<output-path>.png` — absolute or relative path where the PNG is written

## Primary path (curl + jq)

```bash
curl -X POST "https://api.openai.com/v1/images/generations" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-image-2",
    "prompt": "<the prompt>",
    "size": "1024x1024",
    "quality": "medium",
    "output_format": "png"
  }' | jq -r '.data[0].b64_json' | base64 --decode > <output-path>.png
```

Substitute `<the prompt>` and `<output-path>.png` with concrete values. Escape any double quotes inside the prompt before injecting it into the JSON body.

## Python fallback (no jq required)

When `jq` is not on PATH (common on Git Bash / Windows), do the call in two steps — fetch JSON to a temp file, then decode in Python:

```bash
# Step 1: save raw API response
curl -s -X POST "https://api.openai.com/v1/images/generations" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-image-2",
    "prompt": "<the prompt>",
    "size": "1024x1024",
    "quality": "medium",
    "output_format": "png"
  }' -o /tmp/gpt_image_response.json

# Step 2: decode b64_json → PNG
python -c "import json,base64,sys; d=json.load(open(sys.argv[1])); open(sys.argv[2],'wb').write(base64.b64decode(d['data'][0]['b64_json']))" /tmp/gpt_image_response.json <output-path>.png
```

On Windows, replace `/tmp/gpt_image_response.json` with `$env:TEMP\gpt_image_response.json` (PowerShell) or `%TEMP%\gpt_image_response.json` (cmd).

## Error handling

Before claiming success, the caller MUST check:

1. **Missing key** — if `OPENAI_API_KEY` is empty/unset, abort with a clear error. Do not call the API.
2. **HTTP non-2xx** — use `curl -fS` (or inspect `%{http_code}` via `-w`) to surface failures. If the response body contains an `error` object instead of `data`, return the full error message verbatim — do not invent a fix.
3. **Empty `b64_json`** — defensive check after decode: the output file must be non-empty. If zero bytes, the API returned an empty payload; treat as failure.
4. **Invalid output path** — verify the parent directory exists before writing.

## Verification (caller responsibility)

After running:

```bash
# bash
[ -s "<output-path>.png" ] && echo "ok: $(stat -c%s <output-path>.png) bytes" || echo "FAILED"
```

```powershell
# PowerShell
if ((Test-Path '<output-path>.png') -and ((Get-Item '<output-path>.png').Length -gt 0)) {
  "ok: $((Get-Item '<output-path>.png').Length) bytes"
} else { "FAILED" }
```

## Notes

- The model name (`gpt-image-2`), `size`, `quality`, and `output_format` are fixed defaults for the common case. To change them for a specific call, edit the JSON body inline.
- This skill is stateless — it does not retain prompts, manage history, or version outputs. That responsibility belongs to the calling agent (e.g., yuval saves a sibling `.txt` with each PNG).
