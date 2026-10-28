# Kling 3.0 on fal.ai

General video. Standard tier by default, Pro only for finals.

| Field | Value |
|---|---|
| Image-to-video (default) | fal-ai/kling-video/v3/standard/image-to-video |
| Text-to-video | fal-ai/kling-video/v3/standard/text-to-video |
| Pro tier | swap `standard` for `pro` in either id |
| Provider | fal.ai |
| Method | Async via queue |
| Type | Video |
| API key | .env -> FAL_KEY |
| Cost | fal pricing API (2026-10-28): $0.14 per second, listed the same for standard AND pro. A 5 s clip is about $0.70 before any audio surcharge |

Endpoint ids and input fields verified against fal's model catalog on 2026-10-28.
Pricing caveat: the API showed the same rate for both tiers and doesn't say
whether audio changes it. Confirm on the model page before quoting a Pro or
audio-on run.

PAID VIDEO: quote and get explicit approval before every run (see SKILL.md).

## Endpoint (queue)
1. Submit: POST https://queue.fal.run/{endpoint-id}
   Headers: `Authorization: Key $FAL_KEY`, `Content-Type: application/json`
   -> reply has `request_id`, `status_url`, `response_url`
2. Poll every 10-15 s: GET `status_url` (same auth header) -> `status`:
   IN_QUEUE / IN_PROGRESS / COMPLETED
3. When COMPLETED: GET `response_url` -> result JSON
(Queue flow is fal's standard pattern. Not exercised against this model yet.)

## Request format: image-to-video
```json
{
  "prompt": "PROMPT",
  "start_image_url": "https://...",
  "duration": "5",
  "generate_audio": false
}
```
- start_image_url: REQUIRED. Use a public URL, or upload the local file to fal storage first
- end_image_url: optional last frame
- duration: STRING "3" to "15" (default "5")
- generate_audio: DEFAULTS TO TRUE. Always send it explicitly. Use false unless I ask for sound.
- No aspect_ratio field. Output follows the start image.
- Also available: shot_type (customize | intelligent), multi_prompt, elements.
  Leave them out unless asked.

## Request format: text-to-video
Same as above, minus start_image_url and end_image_url, plus
`"aspect_ratio": "16:9" | "9:16" | "1:1"` (default 16:9).

## Response handling
Video URL at `video.url`. Download it immediately into generations/, then write
the sidecar log. Give up after ~10 min and report the request_id.
