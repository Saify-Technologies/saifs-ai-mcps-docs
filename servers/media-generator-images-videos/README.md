# Media Generator — Images, Videos & Audios

Generates and edits marketing media: images, logos, background removal and upscaling, text-to-video and image-to-video, soundtracks for silent video, music, full songs from lyrics, and text-to-speech in many languages. Generation uses account credits.

| | |
|---|---|
| **Registry name** | `ai.saifs/media-generator-images-videos` |
| **Endpoint** | `https://mcp.saifs.ai/mcp` |
| **Transport** | Streamable HTTP (remote, nothing to install) |
| **Auth** | OAuth 2.1 — PKCE (S256) and dynamic client registration; your client opens a sign-in page on first use |
| **Publisher** | [SAIFS AI](https://saifs.ai) |

## Install

### VS Code / GitHub Copilot

Add to `.vscode/mcp.json` (workspace) or your user `mcp.json`:

```json
{
  "servers": {
    "saifs-media-generator": {
      "type": "http",
      "url": "https://mcp.saifs.ai/mcp"
    }
  }
}
```

Then start the server from the MCP view and sign in when prompted.

### Claude (claude.ai, Desktop)

Settings → Connectors → **Add custom connector**, and paste:

```
https://mcp.saifs.ai/mcp
```

### Claude Code

```bash
claude mcp add --transport http saifs-media-generator https://mcp.saifs.ai/mcp
```

### Any other MCP client

Point it at `https://mcp.saifs.ai/mcp` using the Streamable HTTP transport. The server returns `401` with a
`WWW-Authenticate: Bearer … resource_metadata=…` challenge, so OAuth-aware clients sign in automatically.

## Tools

| Tool | What it does |
|---|---|
| `generate_image` | Generate an image from a text prompt. |
| `edit_image_with_text` | Edit an existing image with a text instruction. |
| `logo_generator` | Generate a logo from a description. |
| `remove_background` | Remove the background from an image. |
| `upscale_image` | Upscale an image 2–4×. |
| `generate_video` | Generate a video from a text prompt. |
| `image_to_video` | Animate a still image into a video. |
| `video_to_audio` | Generate a soundtrack for a muted video and merge it in. |
| `music_generator` | Generate instrumental music (up to 30 s) in a chosen category. |
| `song_generator` | Generate a full song from lyrics and genre tags. |
| `text_to_speech` | Convert text to speech in a chosen language and voice. |
| `get_music_categories` | List available music categories. |
| `get_languages` | List available speech languages. |
| `check_token` | Show account status and remaining generation credits. |
| `search` | Search the server's tools. |
| `fetch` | Get details of one tool. |
| `health` | Health check. |

## Example prompts

- “Create a product banner for our Diwali sale with a festive gold theme.”
- “Make a 15-second upbeat jingle for our new app launch.”
- “Turn this product photo into a short rotating video.”

## Support

- Website: https://saifs.ai
- Issues: https://github.com/Saify-Technologies/saifs-ai-mcps-docs/issues
- Privacy: https://saifs.ai/privacy_policy
