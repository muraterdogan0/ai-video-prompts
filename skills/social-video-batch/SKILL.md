---
name: social-video-batch
description: Plan and generate a batch of short-form social videos (TikTok, Instagram Reels, YouTube Shorts, LinkedIn, Facebook, Pinterest) — hooks, angles, A/B variants, per-platform aspect ratios and a posting plan — then render them through Vivideo. Use when the user wants several social videos at once, a content calendar, ad creative variants, or "a week of posts" for a product, brand or topic.
---

# Social video batch

Plan a set of short-form videos that share one goal but test different hooks and angles, then generate them. The plan is the valuable part; generation is the last step and spends credits, so it only happens after the user approves the plan.

## Step 1: Brief

Collect (ask only for what is missing and essential):

- **What** — product, offer, topic or message.
- **Who** — the audience and what they care about.
- **Goal** — awareness, clicks, sign-ups, sales, followers.
- **Platforms** — default to TikTok, Reels and Shorts if not stated.
- **Batch size** — default 5 concepts × 2 hook variants.
- **Assets** — product photos, logo, brand kit, a presenter.
- **Constraints** — claims that are not allowed, tone, words to avoid.

## Step 2: Angles

Pick 3–6 distinct angles so the batch actually tests something. Common ones:

- **Problem → solution** — show the pain, then the fix.
- **Demo** — the product doing the thing, fast.
- **Before / after** — transformation.
- **Myth vs fact** — correct a common belief.
- **List** — "3 ways to…", "5 mistakes…".
- **POV / day-in-the-life** — the viewer's world with the product in it.
- **Social proof style** — creator-style testimonial (fictional and disclosed, or real with consent).
- **Curiosity** — an odd or beautiful image that makes people stay to find out.

## Step 3: Hooks

The first 1–2 seconds decide the result. For each concept write two hook variants that differ in kind, not just wording:

- A **visual hook** (the opening image is already in motion and unusual).
- A **verbal hook** (an on-screen line or first spoken sentence: a bold claim, a question, a number, a "stop doing X").

Patterns that work: [how to hook viewers in short-form video](https://vivideo.ai/blog/how-to-hook-viewers-short-form-video).

## Step 4: Format per platform

| Platform | Aspect ratio | Length guidance |
| --- | --- | --- |
| TikTok | 9:16 | 7–30 s; hook at frame one |
| Instagram Reels | 9:16 (feed preview crops to 4:5) | 7–30 s |
| YouTube Shorts | 9:16 | 15–45 s |
| LinkedIn | 1:1 or 4:5 | 15–45 s, captions essential |
| Facebook feed | 4:5 or 1:1 | 15–30 s |
| Pinterest | 9:16 or 2:3 | 6–15 s, aesthetic first |
| X | 16:9 or 1:1 | 15–30 s |

Keep faces and key objects in the center 60% of a vertical frame so captions and platform buttons do not cover them. Most viewers watch muted: plan captions. Generate the master in 9:16 and reframe for others when the composition allows, rather than re-generating.

## Step 5: The plan (show this before generating)

Return a table, one row per video:

| # | Angle | Hook (visual / verbal) | Shots or prompt | Platform & ratio | Length | Mode / model | CTA |
| --- | --- | --- | --- | --- | --- | --- | --- |

Then:

- **Estimated cost** — from `estimate_credits` if the Vivideo MCP server is connected; otherwise say it will be estimated before rendering.
- **What is being tested** — e.g. "visual vs verbal hook across the same five concepts".
- **Posting order** — spread angles across days; do not post two variants of the same concept on the same day on the same account.

Ask the user to approve, cut or edit rows. Do not generate before approval.

## Step 6: Generate

If the Vivideo MCP server is available, follow the `generate-video-with-vivideo` skill for each approved row:

- Presenter-led, scripted videos → `create_auto_video` with `orientation: "portrait"` (or `landscape`), plus `avatar_id`, `voice_id` and `brand_kit_id` if the user wants a consistent presenter or brand.
- Single-shot visual videos (product, aesthetic, curiosity hooks) → `list_models` filtered by `aspect_ratio` and `duration_seconds`, then `create_manual_video`. Use a product photo as `image_url` when the real product must appear.
- Give each video a `name` that encodes the row (`b1-c3-hookA-tiktok`) and a unique `idempotency_key`.
- Create sequentially, tracking every returned id. Then wait for each with `wait_for_video`, and report a final table of names, statuses and `video_url`s.
- Stop the batch if credits run out or a call fails twice; report what finished and what did not.

Without the MCP server, deliver the plan plus copy-ready prompts the user can paste into Vivideo's [TikTok video maker](https://vivideo.ai/solutions/tiktok-video-maker), [Instagram Reels maker](https://vivideo.ai/solutions/instagram-reels-maker) or [YouTube Shorts maker](https://vivideo.ai/solutions/youtube-shorts-maker), or start from the [TikTok](https://vivideo.ai/templates/tiktok), [Reels](https://vivideo.ai/templates/instagram-reels) or [Shorts](https://vivideo.ai/templates/youtube-shorts) templates.

## Step 7: After rendering

- Add captions ([subtitle generator](https://vivideo.ai/tools/subtitle-generator)), and reframe for other platforms with the [video resizer](https://vivideo.ai/tools/video-resizer).
- Translate the winners for other markets with the [AI video translator](https://vivideo.ai/features/ai-video-translator).
- Suggest what to measure (3-second view rate for hooks, completion rate for pacing, clicks for CTA) and how to read the results: keep the winning hook, change the next variable.

## Rules

- No fake reviews or testimonials presented as real customers; label AI-generated testimonials ([guidance](https://vivideo.ai/blog/ai-testimonial-videos-ftc-compliant)).
- No real people without consent, no other brands' logos, no copyrighted characters or music.
- No claims the user has not provided or cannot support.

## Further reading

[How to make videos for social media](https://vivideo.ai/guides/how-to-make-videos-for-social-media) · [viral TikTok videos with AI](https://vivideo.ai/blog/how-to-create-viral-tiktok-videos-with-ai) · [Instagram Reels ideas](https://vivideo.ai/blog/instagram-reels-ideas) · [repurpose video into shorts](https://vivideo.ai/guides/how-to-repurpose-video-into-shorts) · [AI social video](https://vivideo.ai/solutions/ai-social-video) · [AI video for marketing](https://vivideo.ai/solutions/ai-video-for-marketing). Prompts for every format are in this repository's `prompts/social-shorts.md`, `prompts/ugc-ads.md` and `prompts/product-ads.md`.
