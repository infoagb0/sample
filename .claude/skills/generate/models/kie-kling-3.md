# Kling 3.0 on Kie AI

Video via Kie. Use when I ask for Kie or fal fails. Default video route is fal.

| Field | Value |
|---|---|
| Model ID | kling-3.0/video |
| Provider | Kie AI |
| Method | Async (submit, then poll) |
| Type | Video |
| API key | .env -> KIE_API_KEY |
| Docs | https://docs.kie.ai/market/kling/kling-3.0 |
| Cost | UNKNOWN. Check kie.ai pricing before quoting |

PAID VIDEO: quote and get explicit approval before every run (see SKILL.md).
Model id confirmed by search results; request fields are from Kie's docs
example and have not been run.

## Endpoint
POST https://api.kie.ai/api/v1/jobs/createTask
Headers: `Authorization: Bearer $KIE_API_KEY`, `Content-Type: application/json`

## Request format
```json
{
  "model": "kling-3.0/video",
  "input": {
    "prompt": "PROMPT",
    "image_urls": ["https://public-url-of-start-frame.png"],
    "duration": "5",
    "aspect_ratio": "16:9",
    "mode": "std",
    "sound": false
  }
}
```
- mode: `std` (720p) or `pro` (1080p), from the guide. Not confirmed on the docs page.
- duration 3 to 15 seconds. Type (string or number) unconfirmed.
- Send `sound` explicitly. Leave out multi_shots, multi_prompt, kling_elements unless asked.
- image_urls need public URLs; omit for text-to-video.

## Response handling
Same flow as kie-nano-banana-2.md: taskId -> poll
`/api/v1/jobs/recordInfo?taskId=` every 10-15 s -> parse `resultJson` -> download
`resultUrls[0]` at once. The polling format was confirmed on an image run (see kie-nano-banana-2.md); result files live on `tempfile.aiquickdraw.com`, which the session must allow. Video fields are still UNTESTED. Give up after ~10 min and
report the taskId.
