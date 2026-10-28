# Nano Banana 2 Lite (Google) on fal.ai

The cheap, fast draft model. Use it for every first pass.

| Field | Value |
|---|---|
| Endpoint (text-to-image) | google/nano-banana-2-lite |
| Endpoint (with refs / edit) | google/nano-banana-lite/edit  (note: no "2" in this id) |
| Provider | fal.ai |
| Method | Sync |
| Type | Image |
| API key | .env -> FAL_KEY |
| Cost | UNKNOWN. fal's pricing API shows only "1 unit" with no per-image $. Check fal.ai/models/google/nano-banana-2-lite before quoting |

Endpoint ids and input fields verified against fal's model catalog on 2026-10-28.

## Endpoint
POST https://fal.run/{endpoint-id}
Headers: `Authorization: Key $FAL_KEY`, `Content-Type: application/json`

## Request format
```json
{
  "prompt": "PROMPT",
  "aspect_ratio": "16:9",
  "num_images": 1,
  "output_format": "png"
}
```
- prompt: required
- aspect_ratio: default "auto". Common ratios plus extremes 4:1, 1:4, 8:1, 1:8
- output_format: jpeg | png | webp (default png)
- num_images: default 1
- Edit endpoint only: `"image_urls": ["https://..."]`, the reference images
- No resolution field on Lite. Use Nano Banana 2.1 when you need 2K or 4K.

## Response handling
`images[0].url`. Download it into generations/ right away, then write the
sidecar log. `description` may hold the model's text reply.

## Local reference images
image_urls needs URLs. Upload local files from generations/refs/ to fal storage
first, e.g. `fal_client.upload_file(path)` from the Python `fal-client` package.
(The exact upload method isn't verified here.)
