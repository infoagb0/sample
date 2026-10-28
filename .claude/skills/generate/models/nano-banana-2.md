# Nano Banana 2 (Gemini 3.1 Flash Image) on fal.ai

Everyday images: cheap, fast, strong with reference images. Default for drafts.

| Field | Value |
|---|---|
| Endpoint ID | UNVERIFIED. Look up on fal.ai/models ("Nano Banana 2") on first use and write it here |
| Provider | fal.ai |
| Method | Sync |
| Type | Image |
| API key | .env -> FAL_KEY |
| Cost | ~$0.03-0.04 per image (guide estimate; check fal's model page) |

## Endpoint
POST https://fal.run/{endpoint-id}
Headers: `Authorization: Key $FAL_KEY`, `Content-Type: application/json`
For reference images, fal usually has a separate `/edit` endpoint id. Check the model page.

## Request format (typical fal shape; confirm against the model page)
```json
{
  "prompt": "PROMPT",
  "aspect_ratio": "16:9",
  "num_images": 1,
  "image_urls": ["https://... (edit endpoint only)"]
}
```
- Local refs: upload them to fal storage first to get a URL (the fal client's
  `fal.storage.upload`), or pass a base64 data URI if the model page allows it.

## Response handling
Image URL at `images[0].url`. Download it into generations/ right away, then
write the sidecar log.

## Notes
- After the first successful call, replace every UNVERIFIED field in this file
  with the real values.
