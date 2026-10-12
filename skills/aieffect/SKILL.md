---
name: aieffect
description: Generate AI images and AI videos with the AI Effect tools. Use when the user asks to create, generate, draw, render, edit or animate an image, a photo, a thumbnail, a poster, a product shot, a video, a clip, an ad, an animation or a first/last-frame video, or to turn an image into a video.
---

# Making images and videos with AI Effect

The `aieffect` MCP server generates images and videos with many models (Kling, Veo, Seedance, Hailuo, PixVerse, Grok Imagine, GPT Image, Nano Banana, Midjourney, Seedream, FLUX and more). Every generation spends the user's AI Effect credits, so keep the user in control of spending.

## Workflow

1. **Account**: call `check_account` once per session to see the credit balance and this connection's daily cap. If the server asks for sign-in, tell the user to authenticate the `aieffect` server (they sign in with their AI Effect account in the browser).
2. **Pick a model**: call `list_models` with `type: "image"` or `type: "video"`. Only use model ids, resolutions, aspect ratios, durations and reference inputs that the list shows for that model.
   - Image to video: use a model with `inputs.firstFrame` and pass `firstFrameUrl`.
   - Start and end frames: also needs `inputs.lastFrame`.
   - Reference images / videos / audio: respect the `inputs.reference*` limits.
3. **Reference files**: public https URLs work directly. For a local file, call `create_upload_url` with its MIME type and exact size in bytes, PUT the bytes to `uploadUrl` with exactly the returned headers (for example `curl -X PUT -H "Content-Type: image/png" -H "Content-Length: 12345" --data-binary @file.png "<uploadUrl>"`), then use the returned `url`.
4. **Quote the price**: call `estimate_credits` (free) with the same arguments you plan to generate with, and tell the user the exact credit cost. Wait for a yes before generating, unless the user already approved this specific generation. To compare options, estimate a few and show the prices side by side.
5. **Generate**: call `generate_image` or `generate_video` with a short, stable `idempotencyKey` describing the request (for example `scarf-ad-v1`). It returns right away with a `taskId`.
6. **Wait**: call `get_task` until `status` is `completed` or `failed`. Poll images about every 10 seconds (usually under a minute) and videos about every 30 seconds (usually a few minutes).
7. **Deliver**: give the user every URL in `urls`. If `urlsArePermanent` is false, the file is still being copied to the AI Effect CDN; call `get_task` again a few seconds later for the permanent link. Mention `expiresAt` so they download anything they want to keep.

## Retries and failures

- If a `generate_image` / `generate_video` call times out or its result is unknown, call it again with the **same** `idempotencyKey` and the **same** arguments. This returns the existing task and never charges twice.
- If you change anything, or retry after a failed task, use a **new** `idempotencyKey`.
- Failed generations are refunded automatically. Read the error, fix the prompt or parameters, and do not resend the same input.
- `INVALID_PARAMS` errors name the allowed values; adjust and estimate again.
- Never call the generate tools repeatedly in a loop without the user's go-ahead.
