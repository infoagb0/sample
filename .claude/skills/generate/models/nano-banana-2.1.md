# Nano Banana 2.1 (Google) on fal.ai

The quality image model. Only rerun a draft here once I've picked a favourite.
fal lists this as its default text-to-image pick. It replaces Nano Banana 2.

| Field | Value |
|---|---|
| Endpoint (text-to-image) | google/nano-banana-2.1 |
| Endpoint (with refs / edit) | google/nano-banana-2.1/edit |
| Provider | fal.ai |
| Method | Sync |
| Type | Image |
| API key | .env -> FAL_KEY |
| Cost | UNKNOWN. fal's pricing API shows only "1 unit". Check fal.ai/models/google/nano-banana-2.1 before quoting |

Endpoint ids and input fields verified against fal's model catalog on 2026-10-28.

## Endpoint
POST https://fal.run/{endpoint-id}
Headers: `Authorization: Key $FAL_KEY`, `Content-Type: application/json`

## Request format
```json
{
  "prompt": "PROMPT",
  "aspect_ratio": "16:9",
  "resolution": "2K",
  "num_images": 1,
  "output_format": "png"
}
```
- resolution: 1K | 2K | 4K (default 1K). Higher resolution likely costs more.
- aspect_ratio: default "auto"
- Edit endpoint only: `"image_urls": [...]` (up to 14 refs per fal's notes).
  It can also take `video_url` or `pdf_url` as context.

## Response handling
`images[0].url`. Download it into generations/ right away, then write the sidecar log.
