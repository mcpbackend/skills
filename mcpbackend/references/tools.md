# McpBackend MCP tools

The hosted server exposes these tools through OAuth-authenticated Streamable HTTP.

| Tool | Use |
| --- | --- |
| `list_projects` | List projects available to the signed-in account. |
| `create_project` | Create an isolated backend. Pass `jurisdiction: "eu"` only when EU residency is required. |
| `get_project` | Read one project's metadata by `projectId`. |
| `get_project_api` | Return the canonical data API URL, table and record route templates, auth base URL, integration guidance, and generated OpenAPI contract. Call it before writing application API code; never guess routes or hosts. |
| `get_schema` | Read live tables and columns before planning or changing a backend. This describes storage, not runtime routes. |
| `create_table` | Create a table with columns and optional indexes. An integer `id` is added unless a primary key or `id` is supplied. |
| `add_column` | Add one column. A `NOT NULL` column on an existing table requires a default value. |
| `set_table_rls` | Set table read and write modes to `public`, `authenticated`, or `owner`. Owner mode uses `owner_id` unless `ownerColumn` is supplied. |
| `create_api_key` | Mint a full-access or per-table scoped key. The secret is returned once. Prefer scoped permissions. |
| `set_auth_enabled` | Enable or disable end-user email/password authentication for a project. |
| `create_webhook` | Subscribe an HTTPS endpoint to events such as `posts.created`, `*.deleted`, or `*`. The signing secret is returned once. |
| `get_usage` | Read current-month usage and plan limits for a project. |
| `list_team_members` | List members, access scopes, and projects available for assignment. |
| `add_team_member` | Invite a member with `full` access or `project` access plus `projectIds`. |
| `update_team_member` | Change an existing member's access scope. |
| `remove_team_member` | Revoke a member's shared project access. |

For an existing project, call `list_projects` or `get_project`, then call both `get_schema` and `get_project_api`. For a newly created project, call `get_project_api` after creating its tables.

## Column definitions

Supported types are `text`, `integer`, `real`, `boolean`, `timestamp`, `json`, `uuid`, and `blob`. A column may specify `notNull`, `unique`, `primaryKey`, `defaultValue`, and a foreign-key `references` object. Foreign keys may set `onDelete` and `onUpdate` to `NO ACTION`, `RESTRICT`, `CASCADE`, or `SET NULL`.

Use stable, lowercase snake_case names. Make intentional choices for nullability, uniqueness, defaults, foreign-key behavior, and indexes rather than leaving security or data integrity implicit.
