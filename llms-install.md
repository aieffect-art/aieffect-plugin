# Installing the AI Effect MCP server (for AI agents)

AI Effect is a **remote** MCP server. There is nothing to clone, build or run locally.

- Endpoint: `https://www.aieffect.art/api/mcp`
- Transport: Streamable HTTP
- Auth: OAuth (browser sign-in) or an API key sent as `Authorization: Bearer <key>`

## Steps

1. Ask the user to create an API key at https://www.aieffect.art/plugin/keys (they sign in with their AI Effect account, click **Create key** and copy the key that starts with `aie_live_`). If the client supports OAuth for remote servers, the key can be skipped and the user signs in when prompted.
2. Add the server to the MCP settings. For Cline (`cline_mcp_settings.json`):

```json
{
  "mcpServers": {
    "aieffect": {
      "type": "streamableHttp",
      "url": "https://www.aieffect.art/api/mcp",
      "headers": {
        "Authorization": "Bearer aie_live_REPLACE_WITH_USER_KEY"
      },
      "disabled": false
    }
  }
}
```

3. Verify the connection by calling the `check_account` tool. It is free and returns the user's credit balance.

## Using it

- Call `list_models` with `type: "image"` or `type: "video"` before choosing a model.
- Every generation spends the user's credits: call `estimate_credits` first (free), tell the user the price and wait for confirmation.
- `generate_image` / `generate_video` need a stable `idempotencyKey`; poll `get_task` until the status is `completed` or `failed`.
- For local reference files, call `create_upload_url` and PUT the file to the returned URL.

More details: https://www.aieffect.art/plugin
