---
name: generate
description: Generate images and videos via fal.ai and Kie AI model APIs. Use on /generate, "generate image", "generate video", "create image", "make a thumbnail", "animate this", or any request to produce an image or video file.
---

# /generate

## Models

| Task | Default model | Recipe |
|---|---|---|
| Image (default, drafts) | Nano Banana 2 Lite | models/nano-banana-2-lite.md |
| Image (quality, finals) | Nano Banana 2.1 | models/nano-banana-2.1.md |
| Video (default) | Kling 3.0 Standard | models/kling-3.md |
| Image via Kie | Nano Banana 2 | models/kie-nano-banana-2.md |
| Video via Kie | Kling 3.0 | models/kie-kling-3.md |

Read the recipe file before every generation. If a task needs a model with no
recipe (e.g. readable text in images -> GPT Image 2), say so and offer to write
the recipe first; do not improvise an endpoint.

## Provider
- fal.ai is the default (recipes verified). Kie AI is the second provider.
  Auth: fal `Authorization: Key $FAL_KEY`, Kie `Authorization: Bearer <key>` (added by proxy if a network secret exists)
- Use Kie when I ask for it, or when fal errors or lacks the model. Kie
  recipes are `models/kie-*.md`.
- Compare cost only from real quotes (fal pricing page, kie.ai pricing page).
  Do not assume either provider is cheaper.
- Never switch providers silently. Say which route ran and why.

## Keys
- Kie AI: the environment may add the key for me through a network secret
  (a proxy signs requests to api.kie.ai). If so, send the request with NO
  Authorization header and no `.env`. Only if the call returns 401, look for
  KIE_API_KEY in `.env` or the environment.
- fal.ai and anything else: read the key from `.env` in the workspace root
  (e.g. `set -a; . ./.env; set +a`) and reference it as a variable.
- Never print, echo, log, or paste a key into a file, script, or reply.
- A missing `.env` is not a blocker by itself. Stop only if a call fails
  with 401 and no key can be found. Then tell me.

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
  "provider": "fal | kie",
  "prompt": "the full text prompt sent to the API",
  "refs": ["refs/logo.png"],
  "params": { "aspect": "16:9", "size": "2K" },
  "cost_estimate_usd": 0.03,
  "created": "ISO-8601 UTC timestamp"
}
```
