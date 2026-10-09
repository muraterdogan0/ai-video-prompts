# Contributing

Thanks for helping make this library better. Prompts, model notes, corrections and new skills are all welcome.

## Adding a prompt

1. Add it to the closest pack in `prompts/`, under the next number, with a short descriptive heading.
2. Put the prompt in a fenced code block. Include subject, action, setting, camera (name the move), lighting, style, and a format line such as `9:16, 8 seconds.`
3. Test it on at least one model and mention which one in your pull request. A link to the result helps.
4. Use `[BRACKETS]` for parts the reader should replace.

A prompt will not be merged if it names real people or celebrities, uses brands or logos you do not own, references copyrighted characters, or depicts deceptive content (fake news footage, fake reviews presented as real).

## Updating a model guide

Model capabilities change quickly. Keep advice general and durable, link a source for any specific claim (duration limits, audio support, resolutions), and do not add benchmark numbers you have not measured.

## Adding a skill

New skills go in `skills/<skill-name>/SKILL.md` with YAML frontmatter (`name` in lowercase-with-hyphens, and a `description` that says what the skill does and when to use it). Keep instructions concrete and testable, and list the skill in `skills/README.md`.

## Style

- English, plain and specific. No filler, no hype.
- One prompt per idea; avoid near-duplicates of existing prompts.
- Keep links relevant to the section they appear in.

## Pull requests

Small, focused pull requests are easiest to review. Open an issue first for larger changes such as a new pack or restructuring.
