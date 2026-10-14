# ScreenWeaver MCP Server

Work on your screenplay and storyboard from Claude, ChatGPT, Cursor, VS Code or any MCP client. [ScreenWeaver](https://www.screenweaver.ai) is an AI filmmaking studio: a screenplay editor, a storyboard tied to the script, and a production canvas. This server lets your assistant read and write a ScreenWeaver project directly.

- **Remote server, nothing to install:** `https://app.screenweaver.ai/api/mcp` (Streamable HTTP).
- **Sign in, no API key:** the server speaks OAuth 2.1. You sign in to ScreenWeaver, choose the projects the assistant may open, and allow it. Revoke it any time from your account, in **Connected apps**.
- **22 tools:** create a project, import a Final Draft, WriterDuet or Fountain script, run OCR on a PDF or a scan, edit scenes, turn notes into storyboard shots, cut comic or storyboard pages into panels and connect them as references.
- **Safe by default:** every script edit or import saves the previous version to History, deletions need your explicit confirmation, and no tool deletes a project or a story element.

Full walkthrough per client: [screenweaver.ai/mcp](https://www.screenweaver.ai/mcp).

## Who can use it

The MCP is reserved for ScreenWeaver accounts with credits, included in the Auteur and Studio plans ([pricing](https://www.screenweaver.ai/#pricing)). Reading, writing and importing a script spend nothing. Two steps cost credits: OCR on scanned pages (about half a credit per page) and finding the panels of comic or storyboard pages (about one credit per page). Above 20 pages, the assistant asks you before it runs.

## Connect

**Claude (web, desktop, mobile):** Settings, Connectors, Add custom connector. Paste `https://app.screenweaver.ai/api/mcp`, click Connect and sign in.

**Claude Code:**

```bash
claude mcp add --transport http screenweaver https://app.screenweaver.ai/api/mcp
```

Then type `/mcp` in Claude Code, pick screenweaver and sign in in the browser.

**ChatGPT:** Settings, Apps, turn on Developer mode in the advanced settings, then Create: paste the URL and choose OAuth. Which plans can add a custom app changes over time; see OpenAI's help center.

**Cursor** (`.cursor/mcp.json` or `~/.cursor/mcp.json`):

```json
{
  "mcpServers": {
    "screenweaver": { "url": "https://app.screenweaver.ai/api/mcp" }
  }
}
```

**VS Code** (`.vscode/mcp.json`, used from Copilot Chat in agent mode):

```json
{
  "servers": {
    "screenweaver": { "type": "http", "url": "https://app.screenweaver.ai/api/mcp" }
  }
}
```

**Other clients:** any client that supports remote MCP servers over Streamable HTTP with OAuth sign-in. Paste the URL; the client discovers the sign-in page by itself.

## Tools

| Tool | What it does |
|---|---|
| `list_projects` | Projects you can open, with your role in each. |
| `list_organisations` | Organisations where you can create projects. |
| `get_project_outline` | Episodes, scenes in order, shot and story element counts. |
| `get_scene` | One scene, element by element, with linked characters and place. |
| `search_script` | Every line that contains a word or a phrase. |
| `list_entities` | Characters, places and objects, with their descriptions. |
| `list_shots` | Storyboard shots in timeline order: framing, camera, duration, action. |
| `get_group` | A group of the production canvas: generators, prompts, references, images. |
| `view_media` | Imported or generated images, or a shot's storyboard panel. |
| `get_guide` | ScreenWeaver's built-in guide, so the assistant knows how the app works. |
| `create_project` | A new film or series with an empty script. |
| `set_project_aspect_ratio` | The project's frame shape (16:9, 9:16, 2.39:1, 4:3, 3:4). |
| `import_script` | Final Draft, WriterDuet or Fountain import, after a preview you check. |
| `extract_file_text` | Text from a PDF or photos of pages, with OCR for scans. |
| `edit_script` | Insert, update or delete script elements; deletes need your yes. |
| `create_shots` | Shots in a scene, in order, with framing, camera movement and duration. |
| `update_shot` | Action, framing, camera, duration or continuation of one shot. |
| `delete_shots` | Deletes shots after a preview you confirm. |
| `create_upload` | Upload slot for a PDF or an image, up to 25 MB. |
| `import_media` | Comic pages, boards or reference pictures as images, with automatic panel cutting. |
| `add_reference_images` | Connects images to a shot's panel, a story element or the art direction. |
| `set_prompt` | Rewrites a generator's prompt or a story element's text. |

No tool generates images or video yet: the assistant prepares shots, prompts and references, and you run the generation in the app.

## Example requests

- "Import the Fountain script I'm pasting into my project The Yellow Umbrella. Show me the preview first."
- "Here is my script as a PDF. Import it as a screenplay into a new film project called Night Ferry."
- "Read scene 4 and create the storyboard shots for a one-camera, handheld shoot."
- "Import this manga chapter, cut the pages into panels and create one shot per panel in scene 2."
- "Search the script for every mention of the umbrella and tell me where it is missing."

## OAuth details (for client authors)

- Protected resource metadata: `https://app.screenweaver.ai/.well-known/oauth-protected-resource`; authorization server metadata: `https://app.screenweaver.ai/.well-known/oauth-authorization-server`.
- Authorization code with PKCE (S256), dynamic client registration (RFC 7591), rotated refresh tokens, revocation (RFC 7009).
- An unauthenticated request gets `401` with `WWW-Authenticate: Bearer resource_metadata="…"`.

## Privacy and support

ScreenWeaver logs which tool was called and when, never your prompts or your script text. Privacy policy: [screenweaver.ai/privacy](https://www.screenweaver.ai/privacy). Terms: [screenweaver.ai/terms](https://www.screenweaver.ai/terms). Support: hello@screenweaver.ai.

## License

The documentation in this repository is MIT licensed. The ScreenWeaver service is proprietary and governed by its terms.
