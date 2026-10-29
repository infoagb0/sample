# /generate skill (fal.ai)

A Claude Code skill that makes images and videos through fal.ai and saves every output flat into `generations/`.

## Setup
1. Clone this repo and open it in Claude Code.
2. `cp .env.example .env` and fill in `FAL_KEY=` (default provider). `KIE_API_KEY=` is optional (Kie AI).
3. Try it: `/generate a 1:1 test image of a red cube`

Endpoint IDs and inputs were verified against fal's catalog on 2026-10-28. Image prices are still unknown: check fal's model pages.
