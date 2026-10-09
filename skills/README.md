# Agent skills

Three skills in the standard `SKILL.md` format: a folder with a `SKILL.md` file whose YAML frontmatter has a `name` and a `description`, followed by instructions. Agents that support skills read the description to decide when to load the skill.

| Skill | Use it to |
| --- | --- |
| [ai-video-prompt-writer](ai-video-prompt-writer/SKILL.md) | Turn a rough idea into a production-ready video prompt: structure, model choice, aspect ratio, duration, negative cues. No account needed. |
| [generate-video-with-vivideo](generate-video-with-vivideo/SKILL.md) | Render a video through the Vivideo MCP server with the right call order, credit checks and error handling. |
| [social-video-batch](social-video-batch/SKILL.md) | Plan and generate a batch of short-form videos with hooks, variants and per-platform formats. |

## Install

**Claude Code** — copy a skill folder into your personal skills directory (available in every project) or a project's `.claude/skills/` (shared with the repo):

```bash
git clone https://github.com/muraterdogan0/ai-video-prompts ai-video-prompts
mkdir -p ~/.claude/skills
cp -r ai-video-prompts/skills/ai-video-prompt-writer ~/.claude/skills/
cp -r ai-video-prompts/skills/generate-video-with-vivideo ~/.claude/skills/
cp -r ai-video-prompts/skills/social-video-batch ~/.claude/skills/
```

**Other agents** — copy the folders into the agent's skills directory. Any agent that reads `SKILL.md` files will pick them up; for agents without skill support, paste the body of `SKILL.md` into the system prompt or project instructions.

Keep each folder intact: `ai-video-prompt-writer` includes a `reference.md` it links to.

## Connecting Vivideo (for the generation skills)

`generate-video-with-vivideo` and the generation step of `social-video-batch` need the Vivideo MCP server:

1. Create an API key at [app.vivideo.ai/account/api-keys](https://app.vivideo.ai/account/api-keys).
2. Add the server to your MCP client (Claude Desktop, Claude Code, or `~/.cursor/mcp.json` for Cursor):

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

3. Restart the client and ask: "Check my Vivideo configuration."

More on the [Vivideo MCP server](https://vivideo.ai/features/mcp), [CLI](https://vivideo.ai/features/cli) and [API](https://vivideo.ai/features/api); source at [egocen-vivideo/vivideo-toolchain](https://github.com/egocen-vivideo/vivideo-toolchain); reference at [developers.vivideo.ai](https://developers.vivideo.ai).

Never commit a real API key to a repository or share it in chat.
