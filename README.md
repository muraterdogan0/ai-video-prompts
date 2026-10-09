# AI Video Prompts

A practical, copy-ready library of prompts, model guides and agent skills for generating AI video.

Everything here is written to be pasted straight into a text-to-video or image-to-video model: each prompt names the shot, the camera move, the light, the style, the length and the aspect ratio, so you get a usable clip on the first or second try instead of the tenth. The model guides explain how the major video models read a prompt differently, and the `skills/` folder packages the same knowledge for AI agents (Claude Code, Claude Desktop, Cursor and any agent that loads `SKILL.md` files).

The prompts are model-agnostic. We test and run them in [Vivideo](https://vivideo.ai), an AI video generator that puts many of the models below in one app, so the "try it" links point there — but the prompts work anywhere the same models are available.

## Contents

- [Quick start: generate any prompt in Vivideo](#quick-start-generate-any-prompt-in-vivideo)
- [Anatomy of a good video prompt](#anatomy-of-a-good-video-prompt)
- [Prompt packs by use case](#prompt-packs-by-use-case)
- [Model prompting guides](#model-prompting-guides)
- [Agent skills](#agent-skills)
- [Finishing the clip](#finishing-the-clip)
- [Further reading](#further-reading)
- [Contributing](#contributing)
- [License](#license)

## Quick start: generate any prompt in Vivideo

**In the browser.** Open the [AI video generator](https://vivideo.ai/tools/ai-video-generator), paste a prompt from any pack, pick a model and render. Use [text to video](https://vivideo.ai/features/text-to-video) for prompts that start from words alone, and [image to video](https://vivideo.ai/features/image-to-video) when you have a product shot, a listing photo or a character frame you want to animate. Vivideo is [free to start](https://vivideo.ai/pricing), with no watermark and no credit card.

**Two ways to work:**

- [Manual mode](https://vivideo.ai/features/manual-mode) — you choose the model, duration, aspect ratio and resolution for each shot. This is how the prompts in this repo are meant to be used.
- [Agentic chat](https://vivideo.ai/features/agentic-chat) and [auto-generate](https://vivideo.ai/features/auto-generate) — describe the whole video and Vivideo writes, plans and produces it (up to 10 minutes long), including [AI avatars](https://vivideo.ai/features/ai-avatar-generator) and [AI voices](https://vivideo.ai/features/ai-voice-generator).

**On your phone.** Vivideo runs on [iOS](https://apps.apple.com/us/app/vivideo-ai-ai-video-generator/id6755797079) and [Android](https://play.google.com/store/apps/details?id=com.vivideo.ai), and as a [ChatGPT app](https://chatgpt.com/g/g-6920d74f5328819180b993e3976d7f1e-ai-video-generator-vivideo).

**From an AI agent or code.** Vivideo publishes an [MCP server](https://vivideo.ai/features/mcp), a [CLI](https://vivideo.ai/features/cli) and an [API](https://vivideo.ai/features/api). Create a key at [app.vivideo.ai/account/api-keys](https://app.vivideo.ai/account/api-keys), then add the server to Claude Desktop or Claude Code:

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

Cursor uses the same shape in `~/.cursor/mcp.json`. The CLI is `@vivideo/cli` (`npx @vivideo/cli --help`). Full source for the toolchain is at [egocen-vivideo/vivideo-toolchain](https://github.com/egocen-vivideo/vivideo-toolchain) and API reference at [developers.vivideo.ai](https://developers.vivideo.ai). The [generate-video-with-vivideo](skills/generate-video-with-vivideo/SKILL.md) skill teaches an agent the right call order.

## Anatomy of a good video prompt

A video model is a cinematographer that only hears one paragraph. Give it the decisions a director of photography would need, in roughly this order:

| Part | What to decide | Example |
| --- | --- | --- |
| **Subject** | Who or what, with 2–3 concrete visual details | "a silver-haired fisherman in a yellow oilskin jacket" |
| **Action** | One clear main action, with a verb that moves | "hauls a dripping net over the side of a wooden boat" |
| **Setting** | Place, time of day, weather, era | "grey North Sea at dawn, light drizzle" |
| **Camera** | Shot size, angle, movement, lens | "low-angle medium shot, slow push-in, 35mm lens" |
| **Lighting** | Source, direction, quality, color | "soft overcast light, cold blue tones, warm lantern on deck" |
| **Style** | Medium and look | "photorealistic, documentary feel, subtle film grain" |
| **Duration & format** | Length and aspect ratio | "8 seconds, 16:9" |
| **Audio** | Dialogue, sound effects, ambience, music (models with native audio) | "waves slapping the hull, gulls, no music" |

Rules that hold across almost every model:

1. **One shot, one main action.** A 5–10 second clip can carry one clear beat. Two subjects doing two complex things usually means neither looks right. Build longer sequences from several shots — see [how to make AI videos longer than 60 seconds](https://vivideo.ai/blog/how-to-make-ai-videos-longer-than-60-seconds).
2. **Describe what the camera sees, not what the story means.** "She is nervous about the interview" gives the model nothing; "she smooths her blazer twice and glances at the door" does.
3. **Name the camera move.** Static, slow push-in, pull-back, pan, tilt, tracking, orbit, crane, handheld, drone. If you say nothing, the model guesses.
4. **Light is the cheapest way to look expensive.** Golden hour, overcast softbox, neon rim light, practical lamps — pick one and say where it comes from.
5. **Put dialogue in quotes and name the speaker.** For models with native audio, write `The barista says: "Oat or whole?"` and describe ambience separately. Which models generate sound is tracked in [which AI video models have native audio](https://vivideo.ai/research/which-ai-video-models-have-native-audio).
6. **Say what you do not want** when the tool has a negative prompt field: "no text, no logos, no extra fingers, no warped faces, no camera shake".
7. **Set duration and aspect ratio in the settings when the model exposes them.** The prompts here end with a format line such as `9:16, 8 seconds`; if your tool has separate controls, set them there and keep or drop the line. The [aspect ratio calculator](https://vivideo.ai/tools/aspect-ratio-calculator) helps when you need exact pixel sizes, and [vertical vs horizontal video](https://vivideo.ai/blog/vertical-vs-horizontal-video) covers which to choose.

The long version, with worked examples, is Vivideo's [text-to-video prompt guide](https://vivideo.ai/guides/text-to-video-prompt-guide) and [how to write AI video prompts](https://vivideo.ai/blog/how-to-write-ai-video-prompts). For what actually separates prompts that work from prompts that don't, see the [analysis of 40,000 AI video prompts](https://vivideo.ai/blog/ai-video-prompt-analysis-40000-videos). Unfamiliar terms are defined in the [AI video glossary](https://vivideo.ai/glossary), and the [prompt-writer reference](skills/ai-video-prompt-writer/reference.md) in this repo lists camera, lens, lighting and style vocabulary.

## Prompt packs by use case

| Pack | What's inside | Related |
| --- | --- | --- |
| [Product ads](prompts/product-ads.md) | Hero shots, macro reveals, liquid splashes, launch teasers | [AI ad videos](https://vivideo.ai/solutions/ai-ad-video) |
| [UGC ads](prompts/ugc-ads.md) | Phone-shot creator ads, unboxings, reactions, testimonials | [AI UGC videos](https://vivideo.ai/solutions/ai-ugc-video) |
| [Social shorts](prompts/social-shorts.md) | TikTok, Reels and Shorts hooks, loops and trends | [Social video](https://vivideo.ai/solutions/ai-social-video) |
| [Explainer videos](prompts/explainer-videos.md) | Motion-graphic concepts, SaaS walkthroughs, faceless narration | [AI explainer videos](https://vivideo.ai/solutions/ai-explainer-video) |
| [Real estate](prompts/real-estate.md) | Listing tours, drone exteriors, twilight shots, staging | [Real estate video](https://vivideo.ai/solutions/ai-video-for-real-estate) |
| [Ecommerce](prompts/ecommerce.md) | Catalog motion, fashion, 360 spins, lifestyle placement | [Ecommerce video](https://vivideo.ai/solutions/ai-video-for-ecommerce) |
| [Education & training](prompts/education-training.md) | Lessons, science visuals, onboarding, safety training | [Education video](https://vivideo.ai/solutions/ai-video-for-education) |
| [Cinematic storytelling](prompts/cinematic-storytelling.md) | Short-film shots, genre looks, multi-shot sequences | [AI movie generator](https://vivideo.ai/solutions/ai-movie-generator) |
| [Talking avatars](prompts/talking-avatars.md) | Presenter scripts, spokesperson setups, multilingual dubbing | [AI avatar generator](https://vivideo.ai/features/ai-avatar-generator) |
| [Music & anime](prompts/music-and-anime.md) | Music-video shots, loops, anime and stylized animation | [AI music videos](https://vivideo.ai/solutions/ai-music-video) |
| [Food & restaurants](prompts/food-and-restaurants.md) | Plating, pours, sizzle, recipe steps, menu promos | [Restaurant video](https://vivideo.ai/solutions/ai-video-for-restaurants) |
| [Travel](prompts/travel.md) | Destination reels, hotel promos, aerials, itineraries | [Travel video](https://vivideo.ai/solutions/ai-video-for-travel) |

Want more? Vivideo keeps a browsable [prompt library](https://vivideo.ai/prompts), a gallery of [example videos](https://vivideo.ai/examples) and ready-made [video templates](https://vivideo.ai/templates).

## Model prompting guides

Each model reads prompts a little differently. These guides cover strengths, prompt tips and three example prompts per model. Start with [how to choose an AI video model](https://vivideo.ai/guides/how-to-choose-an-ai-video-model) and the [model strengths map](https://vivideo.ai/research/ai-video-model-strengths-map) if you are not sure which to pick; the [index](models/README.md) has a quick comparison.

| Guide | Model page |
| --- | --- |
| [Veo](models/veo.md) | [Veo 3](https://vivideo.ai/ai-models/veo-3) |
| [Sora 2](models/sora-2.md) | [Sora 2](https://vivideo.ai/ai-models/sora-2) |
| [Kling](models/kling.md) | [Kling](https://vivideo.ai/ai-models/kling-ai-video) |
| [Seedance](models/seedance.md) | [Seedance 2.5](https://vivideo.ai/ai-models/seedance-2-5) |
| [Hailuo](models/hailuo.md) | [Hailuo](https://vivideo.ai/ai-models/hailuo-ai) |
| [WAN](models/wan.md) | [WAN](https://vivideo.ai/ai-models/wan-video) |
| [PixVerse](models/pixverse.md) | [PixVerse](https://vivideo.ai/ai-models/pixverse) |
| [LTX Video](models/ltx-video.md) | [LTX Video](https://vivideo.ai/ai-models/ltx-video) |
| [Luma Ray](models/luma-ray.md) | [Luma Ray](https://vivideo.ai/ai-models/luma-ai) |
| [Vidu](models/vidu.md) | [Vidu](https://vivideo.ai/ai-models/vidu-ai) |
| [Hunyuan Video](models/hunyuan-video.md) | [Hunyuan Video](https://vivideo.ai/ai-models/hunyuan-video) |
| [Grok Video](models/grok-video.md) | [Grok Video](https://vivideo.ai/ai-models/grok-video) |
| [Marey](models/marey.md) | [Marey](https://vivideo.ai/ai-models/marey-realism) |

All models available in Vivideo are listed on the [AI models](https://vivideo.ai/ai-models) page.

## Agent skills

The [`skills/`](skills/README.md) folder contains three skills in the standard `SKILL.md` format:

- [**ai-video-prompt-writer**](skills/ai-video-prompt-writer/SKILL.md) — turns a rough idea into a production-ready prompt with model choice, aspect ratio, duration and negative cues.
- [**generate-video-with-vivideo**](skills/generate-video-with-vivideo/SKILL.md) — generates a video through the Vivideo MCP server with the right call order, credit checks and error handling.
- [**social-video-batch**](skills/social-video-batch/SKILL.md) — plans and generates a batch of short-form videos with hooks, variants and per-platform aspect ratios.

Install by copying a skill folder into `~/.claude/skills/` (or your agent's skills directory). Details in [skills/README.md](skills/README.md).

## Finishing the clip

Generation is rarely the last step. Free browser tools from Vivideo for the usual clean-up:

- Cut and join: [video trimmer](https://vivideo.ai/tools/video-trimmer), [video merger](https://vivideo.ai/tools/video-merger), [loop video](https://vivideo.ai/tools/loop-video), [reverse video](https://vivideo.ai/tools/reverse-video), [speed controller](https://vivideo.ai/tools/video-speed-controller)
- Reframe: [video resizer](https://vivideo.ai/tools/video-resizer), [crop video](https://vivideo.ai/tools/crop-video), [rotate video](https://vivideo.ai/tools/rotate-video)
- Text and sound: [subtitle generator](https://vivideo.ai/tools/subtitle-generator), [add text to video](https://vivideo.ai/tools/add-text-to-video), [add music to video](https://vivideo.ai/tools/add-music-to-video), [mute video](https://vivideo.ai/tools/mute-video), [video to MP3](https://vivideo.ai/tools/video-to-mp3)
- Files: [video compressor](https://vivideo.ai/tools/video-compressor), [video converter](https://vivideo.ai/tools/video-converter), [MOV to MP4](https://vivideo.ai/tools/mov-to-mp4), [WebM to MP4](https://vivideo.ai/tools/webm-to-mp4), [MP4 to GIF](https://vivideo.ai/tools/mp4-to-gif)
- Planning: [script timer](https://vivideo.ai/tools/script-timer) (how long a voiceover will run), [video size calculator](https://vivideo.ai/tools/video-size-calculator)

The full list is on the [tools](https://vivideo.ai/tools) page.

## Further reading

- [How to make an AI video](https://vivideo.ai/guides/how-to-make-an-ai-video) — the end-to-end workflow
- [Text to video AI: a beginner's guide](https://vivideo.ai/blog/text-to-video-ai-ultimate-beginners-guide)
- [Image to video guide](https://vivideo.ai/guides/image-to-video-guide)
- [How to write a video script](https://vivideo.ai/guides/how-to-write-a-video-script)
- [AI video render time benchmark](https://vivideo.ai/research/ai-video-render-time-benchmark)
- [AI video API guide](https://vivideo.ai/blog/ai-video-api-guide)
- [Ethics of AI video](https://vivideo.ai/blog/ethics-of-ai-video) and [is AI-generated video detectable?](https://vivideo.ai/blog/is-ai-generated-video-detectable)
- All [guides](https://vivideo.ai/guides), the [blog](https://vivideo.ai/blog) and [research](https://vivideo.ai/research)

## Contributing

New prompts, model notes and fixes are welcome. Prompts should be tested on at least one model, specific (shot, camera, light, style, format) and free of real people's names, trademarks and copyrighted characters. See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

[MIT](LICENSE) © 2026 Vivideo. Use the prompts in commercial and personal work; attribution is appreciated but not required.
