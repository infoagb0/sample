---
name: generate
description: Generate images and videos via fal.ai model APIs. Use on /generate, "generate image", "generate video", "create image", "make a thumbnail", "animate this", or any request to produce an image or video file.
---

# /generate

## Models

| Task | Default model | Recipe |
|---|---|---|
| Image (default, drafts) | Nano Banana 2 Lite | models/nano-banana-2-lite.md |
| Image (quality, finals) | Nano Banana 2.1 | models/nano-banana-2.1.md |
| Video (default) | Kling 3.0 Standard | models/kling-3.md |

Read the recipe file before every generation. If a task needs a model with no
recipe (e.g. readable text in images -> GPT Image 2), say so and offer to write
the recipe first; do not improvise an endpoint.

## Provider
- fal.ai only. Auth header: `Authorization: Key $FAL_KEY`
- Sync models: POST https://fal.run/{endpoint-id}
- Async (video): use the queue API, see models/kling-3.md
- If fal errors or lacks a model, stop and tell me. Do not switch
  providers without my say-so.

## Keys
- Read FAL_KEY from `.env` in the workspace root. Load it into the shell environment
  (e.g. `set -a; . ./.env; set +a`) and reference it as a variable.
- Never print, echo, log, or paste a key into a file, script, or reply.
- If FAL_KEY is missing, stop and tell me.

## Output
- Save every file FLAT into: `generations/` (workspace root)
- No subfolders. Reference images live in `generations/refs/`
- Naming: `{project}_{description}_{unixtimestamp}.{ext}`, lowercase,
  underscores, no spaces. Use `misc` if no project is given.

## Rules
- Quote the cost (model, duration, resolution, expected dollars) and wait
  for my explicit go before ANY paid video run. Quoting is not approval.
  One approval = one run.
- Draft on the cheap image model first. Only rerun on a quality model
  when I pick a favourite.
- Never describe a logo or face in text. Pass the real image file from
  `generations/refs/` as a reference. If it's missing, stop and ask for it.
- Run multiple generations one at a time to avoid rate limits.
- Download async results immediately; result URLs expire.
- After every save, write the sidecar log (see Logging).
- Report back: file path, model, provider, and actual or estimated cost.

## Logging
After every saved file, write a JSON file with the same basename and a
`.json` extension next to it:

```json
{
  "model": "model-id",
  "provider": "fal",
  "prompt": "the full text prompt sent to the API",
  "refs": ["refs/logo.png"],
  "params": { "aspect": "16:9", "size": "2K" },
  "cost_estimate_usd": 0.03,
  "created": "ISO-8601 UTC timestamp"
}
```
