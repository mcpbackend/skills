---
name: mcpbackend
description: Build and manage McpBackend projects through its hosted MCP server. Use when an agent needs to create or inspect McpBackend projects, design tables and indexes, configure authentication or row-level security, mint scoped API keys, manage webhooks or team access, check usage, or connect application code to a generated REST API.
---

# McpBackend

Use McpBackend's hosted MCP tools to provision and manage application backends. Keep project configuration in the MCP control plane; use the generated REST API for application data at runtime.

## Connect

Use the Streamable HTTP endpoint `https://mcp.mcpbackend.com/mcp`. Complete the OAuth flow in the browser when prompted. Never ask the user to paste an access token, email sign-in code, or dashboard session into chat or source code.

If the McpBackend tools are unavailable, help the user add the endpoint to their MCP client before continuing. Do not substitute guessed HTTP control-plane calls for unavailable MCP tools.

## Build a backend

1. Establish the application's entities, relationships, expected queries, authentication needs, and who may read or write each record. Ask whether EU data residency is required before creating the project because jurisdiction is selected at creation.
2. Inspect before changing. Use `list_projects` or `get_project`, then `get_schema` and `get_project_api` to find the intended project, its current schema, and its exact runtime API contract. Do not assume a table, column, route, or API host.
3. Plan the smallest relational schema that supports the requested flows. Add indexes for fields the application will filter, sort, join, or enforce as unique.
4. Create the project and tables, then read the schema and project API contract again to verify the result. Use `add_column` only for additive changes to existing tables.
5. Configure access explicitly. Enable end-user authentication when the app has users. Set every application table to `public`, `authenticated`, or `owner` reads and writes based on the requested behavior; never infer that public writes are acceptable. Read [references/client-auth.md](references/client-auth.md) before implementing signup, login, session restoration, or authenticated browser access.
6. Create credentials only after the schema and access rules are settled. Prefer per-table, least-privilege API keys. Treat returned API key and webhook secrets as one-time values: put them in the user's secret store or environment file and never commit or repeat them unnecessarily.
7. Verify the final schema, access modes, and usage. Summarize project ID, tables, indexes, auth and RLS choices, created integrations, and the next application-side step. Do not reproduce secrets in the summary.

## Manage existing projects

- Read live state before proposing code or schema changes.
- Prefer additive, reversible changes. Explain when the available MCP tools cannot perform a requested rename, deletion, or migration.
- Create webhooks only for HTTPS endpoints supplied or approved by the user. State which event patterns will be delivered and that signatures must be verified.
- Add, update, or remove team members only when explicitly requested. Confirm the email address and intended full or project-scoped access before changing it.
- Check `get_usage` when a task may add substantial data or traffic, or when the user asks about capacity.

## Tool reference

Read [references/tools.md](references/tools.md) for the current tool catalog, important inputs, and operation-specific guardrails.
