# Nano Banana 2 on Kie AI

Image via Kie. Use when I ask for Kie or fal fails. Default image route is fal.

| Field | Value |
|---|---|
| Model ID | nano-banana-2 |
| Provider | Kie AI |
| Method | Async (submit, then poll) |
| Type | Image |
| API key | .env -> KIE_API_KEY |
| Docs | https://docs.kie.ai/market/google/nano-banana-2 |
| Cost | 8 Kie credits for one 1K image (observed). Dollar value UNKNOWN. Check kie.ai pricing |

Tested 2026-10-29: model id `nano-banana-2` works with the fields below
(1:1, 1K, png), result saved and checked. If a call ever says "model not found",
open the docs page and copy the id fresh.

## Endpoint
POST https://api.kie.ai/api/v1/jobs/createTask
Headers: `Authorization: Bearer $KIE_API_KEY`, `Content-Type: application/json`

## Request format
```json
{
  "model": "nano-banana-2",
  "input": {
    "prompt": "PROMPT",
    "image_input": ["https://... optional reference URLs"],
    "aspect_ratio": "16:9",
    "resolution": "1K",
    "output_format": "png"
  }
}
```
- aspect_ratio: 1:1 1:4 1:8 2:3 3:2 3:4 4:1 4:3 4:5 5:4 8:1 9:16 16:9 21:9 auto
- resolution: 1K | 2K | 4K (default 1K)
- output_format: png | jpg (default unclear, send it explicitly)
- image_input limit is 10 or 14 depending on the source; stay at 10 or fewer.
- Refs must be public URLs (see Kie file upload in kling recipe notes).

## Response handling
1. createTask reply -> `data.taskId`
2. Poll every 3-5 s: GET https://api.kie.ai/api/v1/jobs/recordInfo?taskId={taskId}
3. `data.state` is success / fail; `data.resultJson` is a JSON string holding
   `resultUrls`. Confirmed on `nano-banana-2` (task finished in ~15 s).
4. Result files are hosted on `tempfile.aiquickdraw.com`. The session's network
   must allow that host, or the download is refused. If it is refused, stop and
   tell me; don't re-run the paid job.
5. Download immediately (Kie URLs expire), save flat in generations/, write the sidecar log.
- 401 = bad key, 402 = out of credits. Tell me, don't retry.
