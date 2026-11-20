# Connect the Repping MCP server

Server URL (Streamable HTTP): `https://www.repping.ai/api/mcp`
Auth: OAuth in the browser (sign in to Repping, pick a workspace, allow). No API key.
Plan: MCP access needs the Creator plan — https://www.repping.ai/pricing

- **Claude Code:** `claude mcp add --transport http repping https://www.repping.ai/api/mcp` then run `/mcp` and authenticate.
- **Claude.ai / Claude Desktop:** Settings → Connectors → Add custom connector → paste the URL → choose "Register automatically" for OAuth.
- **Codex CLI:** `codex mcp add repping --url https://www.repping.ai/api/mcp` then `codex mcp login repping`.
- **Cursor / VS Code / Windsurf / Gemini CLI:** add an HTTP MCP server named `repping` with the URL above (see the repo README for JSON).
- **ChatGPT:** Plugins → Add custom MCP server → paste the URL (Business/Enterprise workspaces).

After connecting, call `repping_profile` to confirm it works.
