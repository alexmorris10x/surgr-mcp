# Surgr MCP server

[Surgr](https://surgr.ai) is a social media scheduler for founders. Its hosted MCP server lets AI assistants draft posts for X, propose publishing times, read review feedback, and check results. You approve each post in Surgr before it can be sent.

**[Setup and product details](https://surgr.ai/mcp)** · **[X / Twitter scheduler](https://surgr.ai/use-cases/x-twitter-scheduler-for-founders)** · **[Surgr as a Buffer alternative](https://surgr.ai/alternatives/buffer-for-founders)**

This repository documents the hosted service. The server runs on Surgr; there is nothing here to deploy or install.

## Connection

- Endpoint: `https://surgr.ai/api/mcp`
- Transport: Streamable HTTP
- Authentication: `Authorization: Bearer <your Surgr agent connection key>`
- Create a named, scoped connection in [Surgr's Agents page](https://surgr.ai/agents). Keep the key private and use only the permissions your workflow needs.

### Claude Code

Set `SURGR_API_KEY` securely in your environment, then run:

```sh
claude mcp add --transport http surgr https://surgr.ai/api/mcp \
  --header "Authorization: Bearer $SURGR_API_KEY"
```

### Codex

Set `SURGR_API_KEY` in the environment used by Codex and add this to `~/.codex/config.toml`:

```toml
[mcp_servers.surgr]
url = "https://surgr.ai/api/mcp"
bearer_token_env_var = "SURGR_API_KEY"
```

### Cursor

Add a remote MCP server using this configuration. Replace the placeholder locally and keep the file containing your key out of version control:

```json
{
  "mcpServers": {
    "surgr": {
      "url": "https://surgr.ai/api/mcp",
      "headers": { "Authorization": "Bearer YOUR_SURGR_API_KEY" }
    }
  }
}
```

## Tools

| Tool | Purpose |
| --- | --- |
| `create_review_post` | Create an unapproved draft and optionally propose a future time. |
| `update_review_post` | Revise an unapproved draft with revision checking. |
| `get_review_feedback` | Read the current revision, requested changes, and activity. |
| `list_schedule` | Read proposed, scheduled, and recently published posts with review and delivery state. |
| `schedule_approved_post` | For connections with this permission, dispatch a valid existing approval at its approved time. |
| `read_results` | Read publication receipts and available metrics; unavailable or stale metrics remain labeled. |

Agents cannot create human approval or change its approved time. Publishing starts with X; availability of other channels depends on platform approval and your connected account. Check Channels in Surgr for your account's current availability.

See [Surgr MCP documentation](https://surgr.ai/mcp) for current access and pricing. The same scoped key can also access the [Surgr REST API](https://surgr.ai/api/v1).

## Example workflow

Ask your assistant to draft three X posts about a product update and propose times. Review and approve them in Surgr, then check the schedule and publication receipts with your assistant.

## Support

For product access and setup, visit [surgr.ai](https://surgr.ai). For documentation corrections, open an issue in this repository. Never include API keys, private post content, or account information in a public issue.
