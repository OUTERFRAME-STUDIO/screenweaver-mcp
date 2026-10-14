# Installing the ScreenWeaver MCP server

ScreenWeaver is a remote MCP server. There is nothing to download or build.

- URL: `https://app.screenweaver.ai/api/mcp`
- Transport: Streamable HTTP
- Authentication: OAuth 2.1 (dynamic client registration, PKCE S256). No API key.

Steps for an agent installing it for a user:

1. Add a remote MCP server named `screenweaver` with the URL above, using the client's HTTP / Streamable HTTP transport.
   - Cline: add it under Remote Servers with that URL.
   - Claude Code: `claude mcp add --transport http screenweaver https://app.screenweaver.ai/api/mcp`
   - Cursor: `{ "mcpServers": { "screenweaver": { "url": "https://app.screenweaver.ai/api/mcp" } } }` in `.cursor/mcp.json`
   - VS Code: `{ "servers": { "screenweaver": { "type": "http", "url": "https://app.screenweaver.ai/api/mcp" } } }` in `.vscode/mcp.json`
2. On first use the client opens a ScreenWeaver sign-in page. The user signs in, picks the projects the assistant may open and allows it.
3. The account needs credits (ScreenWeaver Auteur or Studio plan). Reading and writing scripts and shots spend nothing; OCR and panel detection spend a few credits per page and ask first above 20 pages.
4. Call `get_guide` with no topic to read how ScreenWeaver works, then `list_projects`.
