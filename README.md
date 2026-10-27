# Unmerge MCP server and agent skill

[Unmerge](https://unmerge.app) splits a flat image into separate transparent PNG layers, with the hidden parts behind objects filled in. This repository holds the connection details for its remote MCP server and an [Agent Skill](skills/unmerge/SKILL.md) that teaches an agent to use it. The service itself is hosted; there is nothing to install or run.

- **MCP endpoint:** `https://unmerge.app/mcp` (Streamable HTTP)
- **Auth:** `Authorization: Bearer <key>`. Create a key at [unmerge.app/account/api](https://unmerge.app/account/api) (needs a confirmed email).
- **Registry name:** `app.unmerge/unmerge` in the [official MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=app.unmerge/unmerge)
- **Docs:** [MCP](https://unmerge.app/docs/mcp) · [HTTP API](https://unmerge.app/docs/api)

## Connect

Put the key in an environment variable first:

```bash
export UNMERGE_API_KEY=um_...
```

**Claude Code**

```bash
claude mcp add --transport http unmerge https://unmerge.app/mcp --header "Authorization: Bearer $UNMERGE_API_KEY"
```

**Cursor** (`~/.cursor/mcp.json`)

```json
{
  "mcpServers": {
    "unmerge": {
      "url": "https://unmerge.app/mcp",
      "headers": { "Authorization": "Bearer um_..." }
    }
  }
}
```

**Codex** (`~/.codex/config.toml`)

```toml
[mcp_servers.unmerge]
url = "https://unmerge.app/mcp"
bearer_token_env_var = "UNMERGE_API_KEY"
```

## Tools

| Tool | What it does | Cost |
| --- | --- | --- |
| `split_into_layers` | Split a PNG, JPG or WebP (https URL or base64) into named layers. Optional `prompt` says what to separate. Waits up to 50 seconds. | 1 credit per layer returned, at most 17; needs 5 credits to start |
| `get_layers` | Status of a split; when ready, each layer's name, position and a PNG link valid for one hour | Free |
| `unlock_layers` | For status `needs_credits`: pay the credits owed so the files become available | The credits owed, once |
| `get_credits` | Credits left on the account | Free |

Listing tools works without a key; calling one needs it.

## Pricing

The same credits as the website: one credit is one layer returned. See [unmerge.app/pricing](https://unmerge.app/pricing).

## Agent skill

[`skills/unmerge/SKILL.md`](skills/unmerge/SKILL.md) follows the [Agent Skills](https://agentskills.io) format. Install it with

```bash
npx skills add unmerge-app/unmerge-mcp
```

or copy the folder into your agent's skills directory (for Claude Code, `~/.claude/skills/unmerge/`). It uses the MCP tools when they are connected and the HTTP API with `curl` otherwise.

## Support

support@unmerge.app

## License

The files in this repository are MIT licensed. The Unmerge service has its own [terms](https://unmerge.app/terms).
