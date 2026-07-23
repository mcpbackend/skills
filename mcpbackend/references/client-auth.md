# Client authentication

Use project end-user authentication for browser and mobile applications whose users sign up and access their own data. Do not use a project API key in browser or mobile client code.

## Configure access

1. Enable authentication with `set_auth_enabled`.
2. Set each application table's read and write RLS modes explicitly:
   - `authenticated` allows any signed-in project user.
   - `owner` restricts rows to their owning user and is the normal choice for private per-user data.
   - `public` allows anonymous access; never choose public writes without an explicit requirement.
3. Call `get_project_api` and use its exact `authBaseUrl`, data API routes, and host. Never construct or substitute a control-plane URL.

## Implement the client flow

The auth routes require no project API key:

- `POST {authBaseUrl}/signup` with `{ "email": "...", "password": "..." }`
- `POST {authBaseUrl}/login` with `{ "email": "...", "password": "..." }`
- `GET {authBaseUrl}/me` with `Authorization: Bearer <token>`

Signup and login return `{ token, user }`. Store the token using the application's appropriate secure client-storage strategy, keep it out of logs and source control, and send it as `Authorization: Bearer <token>` on data API requests. Use `/me` to validate a restored session.

Tokens are scoped to one project and currently do not expire automatically. There is no refresh-token, password-reset, per-token revocation, or social OAuth flow. Do not invent those endpoints. Disabling project authentication stops its client tokens from being accepted.

## Keep credential types separate

- MCP OAuth authenticates the coding agent to the McpBackend control plane.
- End-user JWTs authenticate application users to one project's data API and remain subject to RLS.
- Project API keys are for trusted server-side integrations, bypass RLS, and are governed by their table permissions.

Never expose an MCP access token, dashboard session, email sign-in code, webhook secret, or project API key in generated client code.
