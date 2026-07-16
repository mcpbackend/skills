# McpBackend Agent Skill

The official Agent Skill for building and managing application backends with [McpBackend](https://mcpbackend.com). It teaches compatible coding agents how to inspect projects, design schemas and indexes, configure authentication and row-level security, create scoped API keys and webhooks, manage team access, and verify changes through the hosted MCP server.

## Install

Install the `mcpbackend` skill with the open Skills CLI:

```bash
npx skills add https://github.com/mcpbackend/skills --skill mcpbackend
```

The CLI detects compatible agents installed on your machine and lets you choose where to install the skill.

To install it for every detected agent without prompts:

```bash
npx skills add https://github.com/mcpbackend/skills --skill mcpbackend --agent '*' --yes
```

Add `--global` to make the skill available across all of your projects:

```bash
npx skills add https://github.com/mcpbackend/skills --skill mcpbackend --agent '*' --global --yes
```

The standard skill files live in [`mcpbackend/`](./mcpbackend). You can review [`SKILL.md`](./mcpbackend/SKILL.md) and the [tool reference](./mcpbackend/references/tools.md) before installing.

## Connect the MCP server

The skill provides the workflow; the MCP server provides the live tools. Connect your agent to:

```text
https://mcp.mcpbackend.com/mcp
```

For Claude Code:

```bash
claude mcp add --transport http mcpbackend https://mcp.mcpbackend.com/mcp
```

For Cursor and other clients that accept `mcp.json`:

```json
{
  "mcpServers": {
    "mcpbackend": {
      "url": "https://mcp.mcpbackend.com/mcp"
    }
  }
}
```

Your client opens McpBackend sign-in and consent in the browser using OAuth 2.1. Do not paste dashboard sessions, email sign-in codes, or access tokens into configuration files.

## Use

Invoke the skill directly:

```text
Use $mcpbackend to create a secure backend for my client portal.
```

Compatible agents can also activate it automatically for McpBackend tasks. The skill guides the agent to inspect live state first, make minimal schema changes, set access rules explicitly, use least-privilege credentials, and verify the final backend without exposing secrets.

## Links

- [MCP server and Agent Skill guide](https://mcpbackend.com/mcp-and-skill/)
- [McpBackend dashboard](https://app.mcpbackend.com)
- [McpBackend Agent Skill source](./mcpbackend/SKILL.md)

## License

MIT
