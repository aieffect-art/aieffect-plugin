# AI Effect plugin

Generate AI images and videos right inside your AI agent.

[AI Effect](https://www.aieffect.art) gives agents one MCP server with the image and video models from the website: Kling, Veo, Seedance, Hailuo, PixVerse, Grok Imagine, GPT Image, Nano Banana, Midjourney, Seedream, FLUX and more. Your agent can check the exact credit cost before generating, upload your reference files, start the generation, wait for it to finish and hand you the download link.

- MCP endpoint: `https://www.aieffect.art/api/mcp` (Streamable HTTP)
- Sign-in: OAuth with your AI Effect account (no API key needed). API keys are also supported, see [aieffect.art/plugin](https://www.aieffect.art/plugin).
- Pricing: generations use your AI Effect credits, the same wallet and prices as the website. Failed generations are refunded.

## Install

### Claude Code

```bash
claude plugin marketplace add aieffect/aieffect-plugin
claude plugin install aieffect@aieffect
```

Start a new session, run `/mcp`, select `aieffect` and sign in.

Or add only the MCP server:

```bash
claude mcp add --transport http aieffect https://www.aieffect.art/api/mcp
```

### Codex

```bash
codex mcp add aieffect --url https://www.aieffect.art/api/mcp
codex mcp login aieffect
```

### Claude (desktop app or claude.ai)

Settings → Connectors → Add custom connector. Name: `AI Effect`, URL: `https://www.aieffect.art/api/mcp`. Click Connect and sign in.

### Any other MCP client (ChatGPT, Cursor, VS Code, Windsurf, Gemini CLI…)

Add a remote (HTTP) MCP server with the URL `https://www.aieffect.art/api/mcp` and sign in when prompted, or send an API key as `Authorization: Bearer $AIEFFECT_API_KEY`. Client-by-client instructions: [aieffect.art/plugin](https://www.aieffect.art/plugin).

## Tools

| Tool | What it does | Spends credits |
| --- | --- | --- |
| `check_account` | Credit balance and this connection's daily cap | No |
| `list_models` | Image or video models with allowed resolutions, aspect ratios, durations and reference inputs | No |
| `estimate_credits` | Exact credit cost of a generation (dry run) | No |
| `generate_image` | Text-to-image or image editing with reference images | Yes |
| `generate_video` | Text-to-video, first/last-frame or reference-to-video | Yes |
| `get_task` | Status and download links of a task | No |
| `list_tasks` | Your recent images and videos | No |
| `create_upload_url` | Upload URL for a local reference file | No |

`generate_image` and `generate_video` require an idempotency key, so retrying after a timeout never charges twice.

## Example prompts

- "Make a 5-second vertical product video of ./scarf.png slowly rotating on a marble table. Tell me the cost first."
- "Generate 4 square thumbnails for a coffee brand launch post, warm morning light, minimal style."
- "Compare the price of a 10-second 1080p video on Kling 3.0 Pro and Seedance 2.0 before generating anything."

## Spending controls

- The agent quotes the exact price and asks before each generation.
- Set a daily credit cap per connection, and see or revoke every connected agent at [aieffect.art/plugin/keys](https://www.aieffect.art/plugin/keys).

## What's in this repository

- `.claude-plugin/` – Claude Code plugin manifest and marketplace
- `plugin.json`, `mcp.json`, `.agents/plugins/marketplace.json` – Agent Plugins / Codex manifests
- `.mcp.json` – MCP server configuration for Claude Code
- `server.json` – MCP Registry entry (`io.github.aieffect/aieffect`)
- `skills/aieffect/` – a skill that teaches the agent the quote → confirm → generate → poll workflow

The plugin contains no code that runs on your machine; it only points your agent at the AI Effect MCP server.

## Privacy and terms

- Privacy policy: https://www.aieffect.art/privacy-policy
- Terms of service: https://www.aieffect.art/terms-of-service
- Support: https://www.aieffect.art

## License

MIT
