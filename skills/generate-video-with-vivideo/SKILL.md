---
name: generate-video-with-vivideo
description: Generate a real AI video through the Vivideo MCP server (@vivideo/mcp) — text-to-video, image-to-video with a chosen model, or a complete AI-avatar video from a prompt — then wait for it and return the video URL. Use when the user asks to create, render or generate a video with Vivideo, check a Vivideo render, list their Vivideo videos, or estimate the credit cost of a video.
---

# Generate a video with Vivideo

[Vivideo](https://vivideo.ai) exposes its video generation to agents through an [MCP server](https://vivideo.ai/features/mcp). The tools are the source of truth; this skill is about calling them in the right order, spending credits only when the user means to, and reporting results honestly.

## Setup (once)

1. The user creates an API key at https://app.vivideo.ai/account/api-keys. Keys start with `vv_live_`.
2. Add the server to the MCP client config. Claude Desktop / Claude Code:

   ```json
   {
     "mcpServers": {
       "vivideo": {
         "command": "npx",
         "args": ["-y", "@vivideo/mcp"],
         "env": { "VIVIDEO_API_KEY": "vv_live_..." }
       }
     }
   }
   ```

   Cursor uses the same shape in `~/.cursor/mcp.json`. Restart the client after editing.
3. Source and issue tracker: https://github.com/egocen-vivideo/vivideo-toolchain. API reference: https://developers.vivideo.ai.

**The API key is a secret.** Never print it, echo it, write it to logs, commit it, or paste it into a prompt, file or chat message. If the user pastes a key into the conversation, tell them to put it in the MCP config's `env` instead and suggest rotating it.

## Tools

| Tool | Purpose | Spends credits |
| --- | --- | --- |
| `check_configuration` | Is the server authenticated? | No |
| `check_account` | Plan, premium status, credit balance | No |
| `list_models` | Models with allowed durations, resolutions, aspect ratios, styles and rates; filter by `mode`, `duration_seconds`, `aspect_ratio`, `limit` | No |
| `list_avatars` | Presenters for auto mode (`avatar_id`) | No |
| `list_voices` | Voices for auto mode (`voice_id`) | No |
| `list_brand_kits` | Brand kits (`brand_kit_id`) | No |
| `estimate_credits` | Cost estimate before creating | No |
| `create_auto_video` | Auto mode: Vivideo writes the script and produces a complete AI-avatar video from a prompt | **Yes** |
| `create_manual_video` | Manual mode: send a prompt to a specific model; add `image_url` for image-to-video | **Yes** |
| `get_video` | Current status and result of one video | No |
| `wait_for_video` | Bounded wait (up to ~20 s per call) for a video to finish | No |
| `list_videos` | Recent videos, newest first | No |
| `render_preview` | Render a video held at status `preview` | **Yes** |

## Call order

1. **`check_configuration`** — if not authenticated, stop and give the setup steps above. Do not guess at fixes.
2. **`check_account`** — if `premium` is false or credits are too low for the request, stop and say so plainly. Plans are listed at [vivideo.ai/pricing](https://vivideo.ai/pricing).
3. **Choose the mode.**
   - **Auto** (`create_auto_video`) — the user wants a finished presenter-style video from a description ("a 60-second explainer about our app"). Optional: `avatar_id` from `list_avatars`, `voice_id` from `list_voices`, `brand_kit_id` from `list_brand_kits`, `orientation` (`landscape` or `portrait`), `duration_seconds` (5–300). Call the list tools only if the user asked for a specific presenter, voice or brand.
   - **Manual** (`create_manual_video`) — the user wants a specific shot or a specific model, or is animating an image. Call **`list_models`** with `mode: "manual"` and the requested `duration_seconds` / `aspect_ratio` so only models that can satisfy the request come back. Use only values that model lists for `duration_seconds`, `aspect_ratio` and `resolution`. Never invent a model id.
4. **`estimate_credits`** — with the chosen mode, model, duration and resolution. Show the estimate and get a clear go-ahead unless the user already asked you to generate.
5. **Create** — `create_auto_video` or `create_manual_video`. Pass an `idempotency_key` (any unique string for this request) and a short `name`. For image-to-video, `image_url` must be a public https URL. Use `negative_prompt`, `seed`, `generate_audio` and `style` only when the chosen model supports them.
6. **`wait_for_video`** with the returned id. If it returns `timed_out: true`, tell the user it is still rendering and call it again; repeat a reasonable number of times, then hand back the id so they can check later.
7. **`get_video`** — when `status` is `completed`, return `video_url`. If `failed`, report the `error` (credits for failed renders are refunded). If `preview`, explain that the video is held for review and `render_preview` renders it — which spends credits, so ask first.

## Writing the prompt

A specific prompt saves credits. For manual mode, write subject, action, setting, camera, light, style, and a format line (or use the `ai-video-prompt-writer` skill). For auto mode, describe the audience, the message, the tone and the length; Vivideo writes the script. Guides: [text-to-video prompt guide](https://vivideo.ai/guides/text-to-video-prompt-guide), [how to choose an AI video model](https://vivideo.ai/guides/how-to-choose-an-ai-video-model).

## Errors

- **400 on create** — usually a duration, aspect ratio or resolution the model does not offer. The error lists the allowed values; pick one, tell the user, and retry once.
- **Authentication errors** — re-run `check_configuration`; the key may be missing, mistyped or revoked. Ask the user to fix the config; never ask them to paste the key into chat.
- **Insufficient credits / not premium** — stop. Do not retry.
- **Network error or timeout on a create call** — do not blindly create again: that can produce a second charged render. Check `list_videos` for the new video first, or retry with the **same** `idempotency_key`.
- **Long renders** — normal. Keep using `wait_for_video`; do not poll `get_video` in a tight loop.

## Rules

- Never invent model ids, video ids, prices, credit balances or statuses. Every such value comes from a tool result.
- Creating a video spends the user's credits and cannot be undone. Confirm before creating unless the user clearly asked for it, and never create more videos than requested.
- Say clearly when a video is queued or still processing; do not imply it is done.
- Only generate people with their consent or fully fictional people; follow the [ethics of AI video](https://vivideo.ai/blog/ethics-of-ai-video).

## More

- [Vivideo CLI](https://vivideo.ai/features/cli) (`@vivideo/cli`, command `vivideo`) for scripts and CI
- [Vivideo API](https://vivideo.ai/features/api) and the [AI video API guide](https://vivideo.ai/blog/ai-video-api-guide)
- [Integrations](https://vivideo.ai/features/integrations) and [AI video automation](https://vivideo.ai/features/ai-video-automation)
- [Auto-generate](https://vivideo.ai/features/auto-generate) and [manual mode](https://vivideo.ai/features/manual-mode) in the web app
