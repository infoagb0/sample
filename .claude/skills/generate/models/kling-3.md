# Kling 3.0 on fal.ai

General video, the sensible default. Standard tier for drafts, Pro for finals.

| Field | Value |
|---|---|
| Endpoint IDs | fal-ai/kling-video/v3/standard/image-to-video (default) |
|  | fal-ai/kling-video/v3/pro/image-to-video (quality) |
|  | fal-ai/kling-video/v3/pro/text-to-video (no start image) |
| Provider | fal.ai |
| Method | Async via queue |
| Type | Video |
| API key | .env -> FAL_KEY |
| Docs | https://fal.ai/models/fal-ai/kling-video/v3/standard/image-to-video |
| Cost | UNVERIFIED. A third-party listing said ~$0.112/s, +50% with audio. Check fal's page before quoting |

PAID VIDEO: quote and get explicit approval before every run (see SKILL.md).
These endpoint IDs come from search results. Not tested yet.

## Endpoint (queue)
1. Submit: POST https://queue.fal.run/{endpoint-id}
   Headers: `Authorization: Key $FAL_KEY`, `Content-Type: application/json`
   -> reply has `request_id`, `status_url`, `response_url`
2. Poll every 10-15 s: GET `status_url` -> `status` is IN_QUEUE / IN_PROGRESS / COMPLETED
3. When COMPLETED: GET `response_url` -> result JSON

## Request format (confirm fields on the docs page)
```json
{
  "prompt": "PROMPT",
  "start_image_url": "https://... (image-to-video only)",
  "duration": "5",
  "aspect_ratio": "16:9",
  "generate_audio": false
}
```
- start_image_url needs a public URL. Upload local files to fal storage first.

## Response handling
Video URL at `video.url`. Download immediately into generations/, then write
the sidecar log. Give up after ~10 min and report the request_id.
